# Enhanced Combined System Maintenance Script

## Overview
The **Enhanced Combined System Maintenance Script** (`EnhancedCombinedSystemMaintenance.ps1`) is a comprehensive PowerShell script designed for Windows 11 system maintenance. It automates critical tasks such as OS maintenance (DISM, SFC), disk maintenance (CHKDSK, fsutil, Contig), shadow copy maintenance, drive optimization (defrag), temporary file cleanup, hardware/driver checks, memory/update checks, and malware scanning with Windows Defender. The script includes user prompts, detailed logging, multi-drive support, and log analysis tailored for Windows 11.

**Current Version**: 2.7.1 (Released August 17, 2025)

## Features
- **OS Maintenance**: Runs DISM (`CheckHealth`, `ScanHealth`, `RestoreHealth`, `AnalyzeComponentStore`, `StartComponentCleanup`) and SFC (`/scannow`) to repair system files and component store.
- **Disk Maintenance**: Performs CHKDSK scans, fsutil repair checks, and Contig (Sysinternals) for MFT optimization on NTFS drives.
- **Shadow Copy Maintenance**: Checks and repairs/deletes corrupted shadow copies, creates a restore point if needed.
- **Drive Optimization**: Optimizes SSDs (TRIM) and defragments HDDs using `defrag /C /O /V`.
- **Temp File Cleanup**: Cleans system temp, user temp, and Windows Update cache, tracking freed space.
- **Hardware/Driver Checks**: Verifies disk health (SMART) and driver status, with recommendations for issues.
- **Memory/Update Checks**: Detects failed updates and schedules memory diagnostics if requested.
- **Malware Scan**: Runs a quick Windows Defender scan, with third-party AV detection.
- **Logging**: Generates detailed logs in UTF-8 encoding, with streamlined output (task start, command, filtered output, result, summary).
- **Log Analysis**: Analyzes DISM.log, CBS.log, and event logs for Windows 11-specific errors and repairs.
- **User Interaction**: Prompts for confirmation at each major step, with defaults to streamline execution.
- **Version Management**: Displays script version (2.7.0) at start and in a new "Final Summary" section, using a single `$scriptVersion` variable for consistency.
- **Enhanced Logging**: Logs script version at start and in the "Final Summary" section of the log file.

## Requirements
- **Operating System**: Windows 11
- **PowerShell**: Version 5.1 or later (included with Windows 11)
- **Privileges**: Must run as Administrator
- **Optional**: Sysinternals Suite (downloaded automatically if missing, with user consent)
- **Internet**: Required for DISM /restorehealth (if no local repair source) and Sysinternals download

## Installation
1. Download `EnhancedCombinedSystemMaintenance.ps1` from this repository.
2. Open PowerShell as Administrator:
   - Right-click Start, select "Windows PowerShell (Admin)" or "Terminal (Admin)".
3. Navigate to the script directory:
   ```powershell
   cd path\to\script
   ```
4. Run the script:
   ```powershell
   .\EnhancedCombinedSystemMaintenance.ps1
   ```

## Usage
1. Launch the script as Administrator.
2. The script displays the version (2.7.0) at the start and checks for pending updates/restarts.
3. For each section (OS Maintenance, Disk Maintenance, etc.), confirm or skip via prompts (default: Y).
4. Review console output for real-time progress and results.
5. At completion, view the "Log Analysis Summary" and new "Final Summary" (including script version) for a detailed overview.
6. Check the log file (`CombinedSystemMaintenanceLog_<hostname>_<date>.txt`) for detailed output, including the script version at start and in the Final Summary.

## Output
- **Console**: Displays version, progress, results, and summaries for each section, with color-coded status (Green=Success, Yellow=Warning, Red=Error).
- **Log File**: Generated in the script directory (`CombinedSystemMaintenanceLog_<hostname>_<date>.txt`), with UTF-8 encoding, including version, commands, filtered output, results, and summaries.
- **Summary**: Includes pending updates/restarts, errors, repairs, optimizations, and recommendations, with script version in the Final Summary.

## Troubleshooting
- **"Running scripts is disabled"**: Run `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned` and try again.
- **DISM /restorehealth fails**: Ensure internet connectivity or specify a local repair source (e.g., `WIM:D:\sources\install.wim:1`).
- **Contig unavailable**: The script prompts to download Sysinternals Suite if missing.
- **Log file issues**: Verify UTF-8 encoding (handled automatically in version 2.5.9+).
- **Errors in logs**: Review `DISM.log`, `CBS.log`, or event logs (System/Application) for details, as recommended in the summary.

## Changelog
See [CHANGELOG.md](CHANGELOG.md) for detailed version history.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributing
Contributions are welcome! Please submit a pull request or open an issue for suggestions or bug reports.
