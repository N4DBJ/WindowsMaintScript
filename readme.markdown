# System Maintenance Script

## Overview
This PowerShell script automates system maintenance tasks on Windows by running DISM, SFC, CHKDSK, and Contig commands with user confirmation prompts. It logs all command outputs to a file named with the hostname and date (e.g., `SystemMaintenanceLog_DESKTOP123_20250815.txt`) and analyzes `DISM.log`, `CBS.log`, and Windows Event Log (for CHKDSK) to provide a summary of errors and repairs. It supports an optional repair source for the DISM `/restorehealth` command, log rotation, and MFT defragmentation on a user-specified drive.

## Features
- Executes the following commands with Yes/No prompts (default: Yes):
  - `dism /online /cleanup-image /checkhealth`: Checks component store for corruption.
  - `dism /online /cleanup-image /scanhealth`: Performs a deeper corruption scan.
  - `dism /online /cleanup-image /restorehealth`: Repairs component store (optional `/source` for repair files).
  - `dism /online /cleanup-image /AnalyzeComponentStore`: Analyzes component store size.
  - `dism /online /cleanup-image /StartComponentCleanup`: Cleans up superseded components.
  - `sfc /scannow`: Scans and repairs system files.
  - `chkdsk /scan c:`: Scans C: drive for file system errors.
  - `contig -v -s <drive>:\$MFT`: Defragments the Master File Table on a user-specified drive (default: C:).
- Logs all command outputs to a file named `SystemMaintenanceLog_HOSTNAME_YYYYMMDD.txt` with rotation (e.g., `_1`, `_2` if file exists).
- Analyzes `DISM.log`, `CBS.log`, and Event Log (Wininit, Event ID 1001) for errors and repairs.
- Displays and logs a summary of error and repair counts.
- Supports an optional repair source for `/restorehealth` (e.g., a WIM file).
- Includes verbose comments and versioning for maintainability.

## Requirements
- Windows operating system (Windows 10 or later recommended).
- PowerShell 5.1 or later (included with Windows).
- Administrative privileges (run in an elevated PowerShell prompt).
- Access to `C:\Windows\Logs\DISM\dism.log` and `C:\Windows\Logs\CBS\cbs.log`.
- Event Log access for CHKDSK analysis.
- Optional: A Windows installation media (e.g., ISO or WIM file) for `/restorehealth` source.
- `contig.exe` from Sysinternals (download from https://learn.microsoft.com/sysinternals/downloads/contig) in PATH for MFT defragmentation.

## Installation
1. Download or clone the repository to your local machine.
2. Ensure `RunSystemMaintenance.ps1`, `changelog.txt`, and `readme.md` are in the same directory.
3. For the Contig command:
   - Download `contig.exe` from the Sysinternals website (https://learn.microsoft.com/sysinternals/downloads/contig).
   - Place it in a directory included in your system PATH (e.g., `C:\Windows\System32`) or specify its path in the script.

## Usage
1. Open an elevated PowerShell prompt (Run as Administrator).
2. Navigate to the script directory:
   ```powershell
   cd path\to\script
   ```
3. (Optional) Edit the script to set a repair source for `/restorehealth`:
   - Open `RunSystemMaintenance.ps1`.
   - Modify the `$repairSource` variable (e.g., `$repairSource = "WIM:D:\sources\install.wim:1"`).
   - If left as `$null` or empty, `/restorehealth` uses Windows Update.
4. Run the script:
   ```powershell
   .\RunSystemMaintenance.ps1
   ```
5. Follow the Yes/No prompts for each command and the log analysis step (press Enter to accept default "Yes").
6. When prompted, enter a drive letter for Contig MFT defragmentation (e.g., `C` or `D`; default: `C`).
7. Review the console output and the log file (e.g., `SystemMaintenanceLog_HOSTNAME_YYYYMMDD.txt`) for command results and analysis summary.

## Output
- **Console**: Displays command execution status, errors, and a summary of errors/repairs from logs.
- **Log File**: Named `SystemMaintenanceLog_HOSTNAME_YYYYMMDD.txt` (e.g., `SystemMaintenanceLog_DESKTOP123_20250815.txt`). If the file exists, a numeric suffix (e.g., `_1`, `_2`) is appended. Contains all command outputs, skipped commands, and the log analysis summary.
- **Log Analysis**:
  - Counts errors (e.g., "error", "failed", "corruption") and repairs (e.g., "repaired", "fixed") in `DISM.log`, `CBS.log`, and CHKDSK Event Log.
  - Summarizes total errors and repairs.

## Customization
- **Change Default Prompt**: Modify the `Default` parameter in the `Confirm-Action` function or add a `Default` field to the `$commands` array (e.g., `Default = "N"`).
- **Repair Source**: Set `$repairSource` to a WIM file path (e.g., `WIM:D:\sources\install.wim:1`) or leave as `$null` for Windows Update.
- **Contig Path**: If `contig.exe` is not in PATH, modify the script to specify its full path (e.g., `C:\Tools\contig.exe`).
- **Log File Location**: Log files are created in the script's directory with hostname and date. Modify `$baseLogFile` for a different naming pattern.
- **Event Log Range**: Adjust `AddHours(-24)` or `-MaxEvents 10` in the CHKDSK Event Log analysis for a different time range or event count.
- **Log Patterns**: Modify `$errorPattern`, `$repairPattern`, `$chkdskErrorPattern`, or `$chkdskRepairPattern` for more specific error/repair detection.

## Notes
- Run the script in an elevated PowerShell session to avoid permission errors.
- CHKDSK analysis uses the Application Event Log (Wininit, Event ID 1001) for the last 24 hours.
- Log parsing uses keyword-based patterns, which may include false positives. Refine patterns for precision if needed.
- Large logs (e.g., CBS.log) or Event Logs may take a few seconds to parse.
- Ensure the repair source (if specified) is accessible and matches the Windows version/edition.
- Log rotation ensures existing logs are not overwritten, with new files appended with a numeric suffix.
- The Contig command requires `contig.exe` from Sysinternals. If not found, the script will log instructions to download it.

## Changelog
See `changelog.txt` for version history and detailed changes.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.