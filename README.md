```markdown
# Windows System Maintenance Script
## Beta, use at own risk ##
## Overview
The `WinSystemMaintenance_2.8.6.ps1` PowerShell script is a comprehensive tool for maintaining Windows systems (Windows 10/11 and Server 2019+). It performs OS repairs, disk checks, shadow copy management, drive optimization, temp file cleanup, hardware/driver verification, memory/update checks, malware scans, and log analysis. The script prompts for confirmation at each step, logs all actions in a timestamped file, and provides detailed summaries with recommendations.

## Requirements
- **Operating System**: Windows 10/11 or Server 2019+
- **Permissions**: Must run as Administrator (`#Requires -RunAsAdministrator`)
- **Internet Access**: Required for DISM /restorehealth (optional local source supported, e.g., `WIM:D:\sources\install.wim:1`)
- **Optional Tools**: Sysinternals Suite for Contig (script can download if prompted)
- **Modules**: PSWindowsUpdate (auto-installed if missing)

## Usage
1. Save the script as `WinSystemMaintenance_2.8.6.ps1`.
2. Open PowerShell as Administrator.
3. Run the script: `.\WinSystemMaintenance_2.8.6.ps1`
4. Follow prompts to confirm each maintenance section (Y/N, default options provided).
5. Review the generated log file (e.g., `CombinedSystemMaintenanceLog_<hostname>_<date>.txt`) for detailed results.

## Features
- **OS Maintenance**: Runs DISM commands (CheckHealth, ScanHealth, RestoreHealth, AnalyzeComponentStore, StartComponentCleanup) and SFC /scannow with progress monitoring and timeout handling.
- **Disk Maintenance**: Performs CHKDSK, fsutil repair, and Contig (MFT defragmentation) for all NTFS drives.
- **Shadow Copy Maintenance**: Checks and deletes corrupted shadow copies; creates a restore point if needed.
- **Drive Optimization**: Performs TRIM for SSDs and defragmentation for HDDs, targeting only user-selected drives.
- **Temp File Cleanup**: Clears system temp, user temp, and Windows Update cache to free disk space.
- **Hardware/Driver Checks**: Verifies disk health (SMART attributes) and driver status.
- **Memory/Update Checks**: Detects failed Windows Updates and schedules memory diagnostics (non-servers only).
- **Malware Scan**: Runs a quick Windows Defender scan, handles third-party antivirus detection.
- **Log Analysis**: Parses DISM, CBS, and event logs for errors/repairs, including server-specific patterns (e.g., WSUS).
- **Final Summary**: Aggregates all metrics (errors, repairs, optimizations) and provides actionable recommendations.

## Changelog
- See the CHANGELOG for prior changelog details.

## Notes for Servers
- Skips memory diagnostic as it’s not applicable to server environments.
- Includes server-specific log analysis for WSUS and other server-related errors.
- Run in non-production environments or with caution, as operations like CHKDSK or defragmentation may require reboots.

## Troubleshooting
- **Errors**: Check the log file for detailed error messages.
- **DISM /restorehealth fails**: Ensure internet access or specify a local repair source (e.g., `WIM:D:\sources\install.wim:1`).
- **Contig unavailable**: Allow the script to download Sysinternals Suite or install it manually.
- **Permissions issues**: Ensure PowerShell is run as Administrator.

## License
MIT License. For issues or contributions, check the Git repository or contact the author.
