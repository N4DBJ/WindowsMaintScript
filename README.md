# WindowsMaintScript

## Overview
The `WindowsMaintScript` is a comprehensive PowerShell script designed for Windows system maintenance. It combines multiple system maintenance tasks into a single script, including TRIM optimization, operating system maintenance (DISM/SFC), disk maintenance (CHKDSK/fsutil/Contig), shadow copy maintenance, drive optimization, hardware/driver checks, memory/update checks, malware scanning with Windows Defender, and Volume Shadow Copy Service (VSS) verification. The script provides user prompts for each section, detailed logging with a timestamped and hostname-specific log file, real-time progress display, and result-driven recommendations based on log and event analysis.

## Current Version
**2.5.7** (August 16, 2025)

## Features
- **Initial System State Check**:
  - Detects pending Windows Updates using `PSWindowsUpdate` module (with fallback to `Get-HotFix`).
  - Identifies pending restarts from sources like Windows Update, Component-Based Servicing, Pending File Rename Operations, and SCCM Client.
  - Prompts for system restart if pending updates or restarts are detected (default: Yes).
- **TRIM Optimization**:
  - Checks and enables TRIM for SSDs using `fsutil`.
- **OS Maintenance (DISM/SFC)**:
  - Executes DISM commands (`CheckHealth`, `ScanHealth`, `RestoreHealth`, `AnalyzeComponentStore`, `StartComponentCleanup`) and SFC (`sfc /scannow`).
  - Displays real-time progress for DISM commands using `Write-Progress`.
  - Skips unnecessary steps (e.g., `RestoreHealth` if no corruption is found).
  - Supports optional repair source for `RestoreHealth`.
- **Disk Maintenance**:
  - Performs CHKDSK scans and repairs (`/f`, `/sdcleanup`) on all NTFS drives.
  - Uses `fsutil` to check and repair corruption.
  - Defragments Master File Table (MFT) using `Contig` from Sysinternals Suite (if available).
- **Shadow Copy Maintenance**:
  - Checks and deletes corrupted shadow copies using `vssadmin` and `fsutil`.
  - Creates a new restore point on C: if corrupted shadow copies are detected.
- **Drive Optimization**:
  - Optimizes SSDs with TRIM (`defrag /O`) and prompts for defragmentation on HDDs.
- **Hardware and Driver Checks**:
  - Checks disk health using SMART data (`Get-PhysicalDisk`, `Get-StorageReliabilityCounter`).
  - Identifies problematic drivers using `Win32_PnPSignedDriver`.
- **Memory and Update Checks**:
  - Detects failed updates and schedules Windows Memory Diagnostic if requested.
- **Malware Scan**:
  - Runs a quick Windows Defender scan (`Start-MpScan`).
  - Checks for third-party antivirus and Windows Defender service status.
- **VSS Verification**:
  - Ensures the Volume Shadow Copy Service is running.
- **Temp File Cleanup**:
  - Cleans `C:\Windows\Temp`, user temp (`%TEMP%`), and Windows Update cache (`SoftwareDistribution\Download`).
  - Calculates and logs freed disk space.
- **Log Analysis**:
  - Analyzes `DISM.log`, `CBS.log`, system/application event logs, and script logs.
  - Summarizes errors, repairs, pending updates, restarts, and provides tailored recommendations.
- **Sysinternals Integration**:
  - Optionally downloads and installs Sysinternals Suite for `Contig` if not present.
  - Adds Sysinternals path to system PATH.
- **User Interaction**:
  - Prompts for each maintenance section with configurable defaults (Y/N).
  - Displays on-screen summaries for each section with errors, repairs, and skipped actions.
- **Logging**:
  - Creates a unique log file (`CombinedSystemMaintenanceLog_<hostname>_<date>.txt`).
  - Logs all commands, outputs, errors, and summaries.

## Requirements
- **Operating System**: Windows 10 or later.
- **PowerShell**: Version 5.1 or higher (included with Windows 10/11).
- **Permissions**: Must run as Administrator (`#Requires -RunAsAdministrator`).
- **Optional Dependencies**:
  - `PSWindowsUpdate` module for update checks (auto-installed if missing).
  - Sysinternals Suite for `Contig` (optionally downloaded during execution).
  - Internet access for downloading Sysinternals Suite or `PSWindowsUpdate`.
- **Tools Used**:
  - Built-in Windows tools: `DISM`, `SFC`, `CHKDSK`, `fsutil`, `defrag`, `vssadmin`, `pnputil`, `wuauclt`, `mdsched`, `Start-MpScan`.
  - Sysinternals: `Contig` (optional for MFT defragmentation).

## Usage
1. **Run the Script**:
   - Save the script as `EnhancedCombinedSystemMaintenance.ps1`.
   - Open PowerShell as Administrator:
     ```powershell
     Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
     .\EnhancedCombinedSystemMaintenance.ps1
     ```
2. **Follow Prompts**:
   - The script prompts for each maintenance section (e.g., "Proceed with OS Maintenance (DISM and SFC)? (Y/N, default: Y)").
   - Respond with `Y` (Yes), `N` (No), or press Enter for the default.
3. **Review Output**:
   - Console output shows real-time progress, section summaries, and recommendations.
   - A log file is generated in the script’s directory (e.g., `CombinedSystemMaintenanceLog_HOSTNAME_YYYYMMDD.txt`).
4. **Post-Execution**:
   - Check the log file for detailed results.
   - Follow recommendations (e.g., reboot for CHKDSK, run full malware scan, update drivers).

## Installation
- Download `EnhancedCombinedSystemMaintenance.ps1` from the repository.
- Optionally, download Sysinternals Suite manually and place it in `C:\SysinternalsSuite` for `Contig` support.
- Ensure PowerShell execution policy allows running scripts:
  ```powershell
  Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
  ```

## Changelog Summary
- **2.5.7 (2025-08-16)**:
  - Fixed DISM command execution error in OS Maintenance Section where `Start-Process` passed the entire command string including "dism" as an argument, causing "Error: 87". Updated `$osCommands` to store only arguments for DISM commands, excluding "dism" prefix, and adjusted `Start-Process` to use the correct argument list.
- **2.5.6 (2025-08-16)**:
  - Fixed `Start-Process` error in OS Maintenance Section where `RedirectStandardOutput` and `RedirectStandardError` used the same temporary file for DISM commands. Now uses separate files for output and error streams, combining them for logging and display.
- **2.5.5 (2025-08-16)**:
  - Added Initial System State Check for pending updates and restarts.
  - Prompts for restart if updates or restarts are pending.
  - Updated Log Analysis to include pending updates/restarts.
- **2.5.4 (2025-08-16)**:
  - Added real-time DISM progress display with `Write-Progress`.
  - Cleaned up DISM log output to exclude repetitive progress lines.
  - Added Windows Defender and third-party antivirus checks.
  - Fixed PowerShell parsing errors in Shadow Copy Maintenance.
  - Changed `Contig` command to remove recursive flag and add `-accepteula`.
- **2.5.3 (2025-08-16)**:
  - Fixed parsing errors in Shadow Copy Maintenance.
  - Updated `Contig` for non-interactive execution.
- **2.5.2 (2025-08-16)**:
  - Added result-driven DISM prompts, section summaries, temp file cleanup, and improved SSD/HDD detection.
- **2.5.1 (2025-08-15)**:
  - Added multi-drive support, SSD/HDD detection, and optional Sysinternals download.
- **2.5.0 (2025-08-14)**:
  - Combined multiple maintenance tasks into a single script with logging and prompts.
- **2.0.0 (2025-08-10)**:
  - Initial release with basic DISM, SFC, CHKDSK, and defrag functionality.

For the full changelog, see [changelog.txt](changelog.txt).

## Notes
- **Reboots**: Some tasks (e.g., CHKDSK `/f`, memory diagnostics) may require a reboot. The script prompts when necessary.
- **SSD Caution**: Avoid running `CHKDSK /r` or full defragmentation on SSDs to prevent wear.
- **Log File**: Generated in the script’s directory with a unique name based on hostname and date.
- **Recommendations**: The script provides actionable recommendations based on errors, pending updates, or hardware issues.

## Contributing
- Submit issues or pull requests via the repository (if hosted).
- Test on Windows 10/11 systems and report compatibility issues.
- Suggest additional maintenance tasks or optimizations.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details (if applicable).

## Contact
For support, contact the script author via the repository or xAI support channels (e.g., https://x.ai).