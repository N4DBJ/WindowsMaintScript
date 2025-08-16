# Enhanced Combined System Maintenance Script

## Overview
The `EnhancedCombinedSystemMaintenance.ps1` (version 2.5.9) is a PowerShell script designed for comprehensive system maintenance on Windows 11 systems. It automates critical tasks such as OS integrity checks, disk maintenance, shadow copy management, drive optimization, temporary file cleanup, hardware/driver checks, memory/update checks, malware scanning, and log analysis. The script includes user prompts for control, detailed logging in UTF-8 format for compatibility with log upload systems, and enhanced error detection tailored for Windows 11.

## Features
- **Initial System State Check**: Detects pending Windows Updates and restarts, with prompts to restart if needed.
- **OS Maintenance**: Runs DISM (`/checkhealth`, `/scanhealth`, `/restorehealth`, `/AnalyzeComponentStore`, `/StartComponentCleanup`) and SFC (`/scannow`) to check and repair system integrity.
- **Disk Maintenance**: Performs CHKDSK scans, `fsutil` repair checks, and Contig (if Sysinternals Suite is installed) for NTFS drives.
- **Shadow Copy Maintenance**: Checks and deletes corrupted shadow copies, creates a new restore point for C: if needed.
- **Drive Optimization**: Executes TRIM for SSDs and defragmentation for HDDs using `defrag /C /O /V`.
- **Temp File Cleanup**: Clears system temp, user temp, and Windows Update cache folders.
- **Hardware and Driver Checks**: Verifies disk health via SMART data and checks for problem drivers.
- **Memory and Update Checks**: Identifies failed updates and optionally schedules Windows Memory Diagnostic.
- **Malware Scan**: Runs a quick Windows Defender scan, with support for third-party antivirus detection.
- **VSS Service Check**: Ensures the Volume Shadow Copy service is running.
- **Log Analysis**: Analyzes DISM.log, CBS.log, and event logs for errors, repairs, and recommendations, with Windows 11-specific patterns.
- **Logging**: Generates detailed logs in UTF-8 encoding (`CombinedSystemMaintenanceLog_<hostname>_<date>.txt`) to ensure compatibility with log upload systems.

## Requirements
- **Operating System**: Windows 11
- **PowerShell**: Windows PowerShell 5.1 or later
- **Permissions**: Must be run as Administrator (`#Requires -RunAsAdministrator`)
- **Optional**:
  - Sysinternals Suite for Contig (downloaded automatically if permitted).
  - Internet connection for DISM `/restorehealth` (if no local repair source is specified).
  - PSWindowsUpdate module for detailed update checks (installed automatically if permitted).

## Usage
1. **Run the Script**:
   - Open PowerShell as Administrator.
   - Navigate to the script directory: `cd path\to\script`
   - Execute: `.\EnhancedCombinedSystemMaintenance.ps1`
2. **Follow Prompts**:
   - The script prompts for each major section (e.g., "Proceed with OS Maintenance? (Y/N, default: Y)").
   - Respond with `Y` (Yes) or `N` (No), or press Enter for the default.
   - For critical actions (e.g., restart, defrag for HDDs), defaults may be `N` to prevent unintended long operations.
3. **Review Output**:
   - Console output displays progress, results, and summaries in color (Green for success, Yellow for warnings, Red for errors).
   - A log file (`CombinedSystemMaintenanceLog_<hostname>_<date>.txt`) is generated in UTF-8 encoding for compatibility with upload systems.
4. **Check Recommendations**:
   - The Log Analysis section provides actionable recommendations (e.g., review DISM.log, run CHKDSK /f /r, update drivers).

## Log File
- **Location**: Same directory as the script.
- **Format**: `CombinedSystemMaintenanceLog_<hostname>_<date>.txt` (e.g., `CombinedSystemMaintenanceLog_DESKTOP-123_20250816.txt`).
- **Encoding**: UTF-8 to ensure compatibility with systems rejecting non-standard encodings (e.g., UTF-16).
- **Content**: Includes script version, timestamps, commands, filtered output, results, summaries, and recommendations.

## Notes
- **UTF-8 Encoding**: Version 2.5.9 uses UTF-8 for all log writes to resolve "Unsupported text encoding" errors during log uploads.
- **Streamlined Logging**: Logs include only essential information (task start, command, filtered output, result, summary) to avoid redundancy.
- **Windows 11 Compatibility**: Enhanced error detection and recommendations for Windows 11-specific issues (e.g., component store corruption, CBS errors).
- **Timeouts and Stall Detection**: DISM `/restorehealth` includes a 1-hour timeout and 10-minute stall detection to prevent hangs.
- **Sysinternals Suite**: If not installed, the script prompts to download and extract it to `C:\SysinternalsSuite` for Contig support.
- **Repair Source**: Specify a local repair source for DISM `/restorehealth` (e.g., `WIM:D:\sources\install.wim:1`) in the `$repairSource` variable if no internet is available.

## Example Log Output
```
2025-08-16 15:30:00 - Script Version: 2.5.9
2025-08-16 15:30:01 - Starting Initial System State Check...
2025-08-16 15:30:02 - No pending updates found.
2025-08-16 15:30:03 - No pending restarts detected.
2025-08-16 15:30:04 - Starting: CheckHealth - Check for component store corruption (dism /online /cleanup-image /checkhealth)
2025-08-16 15:30:04 - Command: dism /online /cleanup-image /checkhealth
2025-08-16 15:30:05 - No component store corruption detected.
2025-08-16 15:30:05 - Result: Success: No corruption detected
...
2025-08-16 15:35:00 - Log Analysis Summary:
2025-08-16 15:35:01 - DISM.log Errors/Failures: 0
2025-08-16 15:35:01 - DISM.log Repairs: 0
...
```

## Troubleshooting
- **Log Upload Issues**: If you encounter "Unsupported text encoding" errors, verify the log file is UTF-8 encoded. Convert existing logs using:
  ```powershell
  Get-Content -Path "path\to\logfile.txt" | Set-Content -Path "converted_logfile.txt" -Encoding UTF8
  ```
- **DISM Errors**: Check `C:\Windows\Logs\DISM\dism.log` and `C:\Windows\Logs\CBS\cbs.log` for details.
- **CHKDSK Scheduled**: Reboot to complete fixes if dirty volumes are detected.
- **No Internet**: Specify a local repair source for DISM `/restorehealth` or ensure connectivity.

## License
This script is provided "as is" without warranty. Use at your own risk.