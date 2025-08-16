# Changelog

## [2.5.9] - 2025-08-16
### Changed
- Changed log file encoding to UTF-8 for all writes to resolve "Unsupported text encoding" upload issues.
- Retained streamlined logging from 2.5.8 (task start, command, filtered output, result, summary only).
- Retained script version logging at start ("Script Version: 2.5.9").
- Retained Windows 11-specific error patterns and recommendations in Log Analysis section.

## [2.5.8] - 2025-08-16
### Added
- Added script version to log file at start.
- Enhanced Log Analysis section with Windows 11-specific error/repair patterns and recommendations.
### Changed
- Streamlined logging to remove redundant status lines (e.g., repetitive progress, "End of" unless critical).
- Enhanced console output in OS Maintenance to clearly display DISM results (e.g., "Result: Success: No corruption detected").
- Added full command to description line for each step (e.g., "dism /online /cleanup-image /checkhealth").

## [2.5.7] - 2025-08-16
### Added
- Added timeout (1 hour) and stalled progress detection (10 min) for DISM /restorehealth to prevent hangs.
- Added pre-check for internet connectivity if no repair source is specified.
- Added prompt to run SFC /scannow before /restorehealth if not already run.
- Added script version display in console.
### Fixed
- Fixed DISM command execution error by updating $osCommands to store only arguments and adjusting Start-Process.

## [2.5.5] - 2025-08-16
### Added
- Added Initial System State Check for pending updates and restarts with user prompts.
- Updated Log Analysis to include pending updates and restarts.

## [2.5.4] - 2025-08-16
### Added
- Added real-time DISM progress display using Write-Progress.
- Added Windows Defender and third-party antivirus checks.
### Changed
- Cleaned up DISM log output to exclude repetitive progress lines.
### Fixed
- Fixed PowerShell parsing errors in Shadow Copy Maintenance.
- Changed Contig command to remove recursive flag and add -accepteula.

## [2.5.3] - 2025-08-16
### Added
- Added Shadow Copy Maintenance section to check and delete corrupted shadow copies.
- Added support for creating a new restore point for C: after shadow copy cleanup.
### Fixed
- Improved error handling for CHKDSK commands to capture and log errors more reliably.

## [2.5.2] - 2025-08-16
### Added
- Added Hardware and Driver Checks section to verify disk health via SMART data and detect problem drivers.
- Added Memory and Update Checks section to identify failed updates and optionally schedule memory diagnostics.
### Changed
- Improved logging to include timestamps for all entries.

## [2.5.1] - 2025-08-16
### Added
- Added Drive Optimization section with TRIM for SSDs and defragmentation for HDDs using `defrag /C /O /V`.
- Added detection of drive types (SSD vs. HDD) using Get-PhysicalDisk.
### Fixed
- Fixed temp file cleanup to handle locked files gracefully.

## [2.5.0] - 2025-08-16
### Added
- Added comprehensive Log Analysis section to parse DISM.log, CBS.log, and event logs for errors and repairs.
- Added recommendations based on log analysis (e.g., review logs, run CHKDSK).
### Changed
- Refactored script to use a modular structure with separate sections for each maintenance task.

## [2.0.0] - 2025-08-16
### Added
- Added Temp File Cleanup section for system temp, user temp, and Windows Update cache.
- Added user prompts for each major section to allow skipping tasks.
- Added support for Sysinternals Suite download and Contig for MFT optimization.
### Changed
- Improved error handling with try-catch blocks for all external commands.
- Changed log file naming to include hostname and date (`CombinedSystemMaintenanceLog_<hostname>_<date>.txt`).

## [1.5.0] - 2025-08-16
### Added
- Added Disk Maintenance section with CHKDSK /scan and fsutil repair checks.
- Added basic logging to a text file for all commands and outputs.
### Fixed
- Fixed DISM /restorehealth to prompt for a local repair source if no internet is available.

## [1.0.0] - 2025-08-16
### Added
- Initial release with OS Maintenance (DISM /checkhealth, /scanhealth, /restorehealth, /StartComponentCleanup, SFC /scannow).
- Basic console output for results.
- Support for running as Administrator with `#Requires -RunAsAdministrator`.
### Notes
- Initial version focused on core system integrity checks for Windows 10/11.