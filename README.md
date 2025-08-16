# System Maintenance Script

## Overview

This PowerShell script automates comprehensive system maintenance tasks on Windows by running DISM, SFC, CHKDSK, Contig, defrag, temp file cleanup, hardware checks, driver checks, memory diagnostics, malware scans, and Event Log analysis, with user confirmation prompts. It logs all command outputs to a file named with the hostname and date (e.g., `CombinedSystemMaintenanceLog_DESKTOP123_20250816.txt`) and analyzes logs (`DISM.log`, `CBS.log`, Event Logs) to provide a detailed summary of errors, repairs, and recommendations. It supports multiple NTFS drives, SSD/HDD detection, optional repair sources for DISM, and Sysinternals Suite integration.

## Features

* Executes the following commands with Yes/No prompts (default: Yes unless specified):
  - **OS Maintenance**:
    - `dism /online /cleanup-image /checkhealth`: Checks component store for corruption.
    - `dism /online /cleanup-image /scanhealth`: Performs a deeper corruption scan.
    - `dism /online /cleanup-image /restorehealth`: Repairs component store (optional `/source` for repair files).
    - `dism /online /cleanup-image /AnalyzeComponentStore`: Analyzes component store size.
    - `dism /online /cleanup-image /StartComponentCleanup`: Cleans up superseded components.
    - `sfc /scannow`: Scans and repairs system files.
  - **Disk Maintenance** (all NTFS drives):
    - `fsutil dirty query <drive>`: Checks for dirty volumes.
    - `fsutil repair state <drive>`: Checks file system corruption.
    - `fsutil repair initiate <drive>`: Initiates repair for corrupted drives.
    - `chkdsk /scan <drive>`: Scans for file system errors.
    - `chkdsk /f /sdcleanup <drive>`: Repairs corruption if detected (reboot may be required).
    - `contig -v -s <drive>:\$MFT`: Defragments the Master File Table.
  - **Shadow Copy Maintenance** (all NTFS drives):
    - `vssadmin list shadows /for=<drive>`: Lists shadow copies.
    - `fsutil repair state <shadow_volume>`: Checks shadow copy corruption.
    - `vssadmin delete shadows /shadow=<id>`: Deletes corrupted shadow copies.
    - `Checkpoint-Computer`: Creates a new restore point for C: if shadow copies are corrupted.
  - **Drive Optimization**:
    - `fsutil behavior query DisableDeleteNotify`: Checks TRIM status for SSDs.
    - `defrag /C /O /V`: Runs TRIM for SSDs (automatic) or defragmentation for non-SSDs (prompted, default "No" due to long runtime).
  - **Temp File Cleanup**:
    - Clears `C:\Windows\Temp`, `%TEMP%`, and `C:\Windows\SoftwareDistribution\Download`.
    - Tracks and logs space freed (in MB) and errors.
    - Stops/starts Windows Update service (`wuauserv`) for update cache cleanup.
  - **Hardware and Driver Checks**:
    - `Get-PhysicalDisk` and `Get-StorageReliabilityCounter`: Checks SMART status (health, read/write errors, wear).
    - `Get-WmiObject Win32_PnPSignedDriver`: Scans for unsigned/problem drivers.
    - `pnputil /scan-devices`: Rescans devices if issues found.
  - **Memory and Update Checks**:
    - Registry check for pending reboots (`HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\Auto Update\RebootRequired`).
    - `Get-HotFix`: Checks for failed updates.
    - `wuauclt /detectnow`: Triggers update detection if needed.
    - `mdsched.exe /scheduleboot`: Schedules Windows Memory Diagnostic (prompted, default "No").
  - **Malware Scan**:
    - `Start-MpScan -ScanType QuickScan`: Runs quick Windows Defender scan.
    - Event Log analysis (IDs 1000, 1001) for threats.
* Logs all outputs to `CombinedSystemMaintenanceLog_HOSTNAME_YYYYMMDD.txt` with rotation (e.g., `_1`, `_2` if file exists).
* Analyzes `DISM.log`, `CBS.log`, and Event Logs (System/Application, IDs 7, 11, 50, 55, 98, 137, 140, 153, 219, 41, 7023, 2004, 1001, 12289, 12290) for errors and repairs, with recommendations (e.g., "Run full CHKDSK /f /r if disk errors detected").
* Supports SSD vs. HDD detection to optimize defrag (TRIM for SSDs, prompt for HDDs).
* Downloads Sysinternals Suite if `contig.exe` is missing (prompted, default "No").
* Supports optional repair source for DISM `/restorehealth` (e.g., WIM file).
* Includes verbose comments, versioning, and SSD-safe operations (e.g., no `chkdsk /r` unless critical).

## Requirements

* Windows operating system (Windows 10 or later recommended).
* PowerShell 5.1 or later (included with Windows).
* Administrative privileges (run in an elevated PowerShell prompt).
* Access to `C:\Windows\Logs\DISM\dism.log` and `C:\Windows\Logs\CBS\cbs.log`.
* Event Log access for analysis (System, Application, Windows Defender).
* Optional: Windows installation media (e.g., ISO or WIM file) for `/restorehealth` source.
* `contig.exe` from Sysinternals (download from https://learn.microsoft.com/sysinternals/downloads/contig) in `C:\SysinternalsSuite` or PATH for MFT defragmentation.

## Installation

1. Download or clone the repository to your local machine.
2. Ensure `EnhancedCombinedSystemMaintenance.ps1`, `changelog.txt`, and `readme.md` are in the same directory.
3. For the Contig command:
   - Download `contig.exe` from https://learn.microsoft.com/sysinternals/downloads/contig and place it in `C:\SysinternalsSuite`.
   - Alternatively, the script can download and extract the Sysinternals Suite to `C:\SysinternalsSuite` if prompted (requires internet access).

## Usage

1. Open an elevated PowerShell prompt (Run as Administrator).
2. Navigate to the script directory:
   ```powershell
   cd path\to\script
   ```
3. (Optional) Edit the script to set a repair source for `/restorehealth`:
   - Open `EnhancedCombinedSystemMaintenance.ps1`.
   - Modify the `$repairSource` variable (e.g., `$repairSource = "WIM:D:\sources\install.wim:1"`).
   - If left as `$null`, `/restorehealth` uses Windows Update.
4. Run the script:
   ```powershell
   .\EnhancedCombinedSystemMaintenance.ps1
   ```
5. Follow the Yes/No prompts for each section (OS Maintenance, Disk Maintenance, Shadow Copy Maintenance, Drive Optimization, Temp File Cleanup, Hardware/Driver Checks, Memory/Update Checks, Malware Scan, Log Analysis). Press Enter to accept defaults (usually "Yes", except "No" for non-SSD defrag and memory diagnostic).
6. Review the console output and log file (e.g., `CombinedSystemMaintenanceLog_HOSTNAME_YYYYMMDD.txt`) for results, analysis, and recommendations.
7. Reboot if prompted (e.g., for CHKDSK repairs or memory diagnostics).

## Output

* **Console**: Displays command execution status, errors, drive types (SSD/HDD), temp file cleanup results (space freed, errors), and a detailed summary of errors, repairs, and recommendations (e.g., "Temp File Cleanup Freed: 1500 MB", "Run full Defender scan if threats detected").
* **Log File**: Named `CombinedSystemMaintenanceLog_HOSTNAME_YYYYMMDD.txt` (e.g., `CombinedSystemMaintenanceLog_DESKTOP123_20250816.txt`). If the file exists, a numeric suffix (e.g., `_1`, `_2`) is appended. Contains all command outputs, skipped commands, drive type details, cleanup metrics, and analysis summary.
* **Log Analysis**:
  - Counts errors (e.g., "error", "failed", "corruption") and repairs (e.g., "repaired", "fixed") in `DISM.log`, `CBS.log`, and Event Logs.
  - Includes drive-specific metrics (e.g., MFT fragments, drive type), cleanup metrics (space freed, errors), and system-wide issues (disk, driver, hardware, VSS).
  - Provides recommendations based on findings (e.g., "Schedule regular cleanups if >1 GB freed").

## Customization

* **Change Default Prompts**: Modify the `Default` parameter in the `Confirm-Action` function (e.g., `Default = "N"`) for different section defaults.
* **Repair Source**: Set `$repairSource` to a WIM file path (e.g., `WIM:D:\sources\install.wim:1`) or leave as `$null` for Windows Update.
* **Contig Path**: If `contig.exe` is not in `C:\SysinternalsSuite` or PATH, modify the script to specify its full path (e.g., `C:\Tools\contig.exe`).
* **Log File Location**: Log files are created in the script’s directory. Modify `$baseLogFile` for a different naming pattern.
* **Event Log Range**: Adjust `AddHours(-48)` in the Event Log analysis for a different time range.
* **Log Patterns**: Modify `$errorPattern`, `$repairPattern`, `$chkdskErrorPattern`, etc., for more specific error/repair detection.
* **Drive Exclusion**: Filter drives in `$ntfsDrives` (e.g., `$ntfsDrives = $ntfsDrives | Where-Object { $_.DriveLetter -ne 'D' }`).
* **MFT Fragment Threshold**: Adjust the recommendation logic (e.g., warn if MFT fragments >50).
* **Temp File Cleanup**: Add additional paths (e.g., browser caches) or adjust thresholds for recommendations (e.g., warn if >500 MB freed).

## Notes

* Run the script in an elevated PowerShell session to avoid permission errors.
* Event Log analysis covers System and Application logs for disk (IDs 7, 11, 50, 55, 98, 137, 140, 153), driver (219, 7023), hardware/memory (41, 2004, 1001), and VSS (12289, 12290) issues over the last 48 hours.
* Shadow copy maintenance deletes corrupted copies (e.g., `\Device\HarddiskVolumeShadowCopy3`) and creates a new restore point for C: if needed.
* SSD optimization uses `defrag /C /O /V` for TRIM (automatic); HDD defragmentation is prompted (default "No") to avoid long runtimes.
* Temp file cleanup is SSD-safe, skips locked files, and restarts the Windows Update service after clearing the update cache.
* Log parsing uses keyword-based patterns, which may include false positives. Refine patterns for precision if needed.
* Large logs (e.g., CBS.log) or Event Logs may take a few seconds to parse.
* Ensure the repair source (if specified) matches the Windows version/edition.
* Backup critical data before running, as shadow copy deletion removes restore points/backups.
* The script is SSD-safe, avoiding `chkdsk /r` unless critical and using TRIM for SSDs.

## Changelog

See `changelog.txt` for version history and detailed changes.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## About

Personal attempt to automate maintenance tasks for Windows servers and desktops.