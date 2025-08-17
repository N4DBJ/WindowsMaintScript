# Changelog

All notable changes to the Enhanced Combined System Maintenance Script will be documented in this file.
Testing all uploads with: Invoke-ScriptAnalyzer -Path .\filename.ps1 -Severity Error -IncludeRule @('PSUseConsistentIndentation', 'PSUseConsistentWhitespace', 'PSAvoidUsingWriteHost', 'PSAvoidUsingInvokeExpression', 'PSUseDeclaredVarsMoreThanAssignments')
## [2.7.0] - 2025-08-17
### Added
- Introduced `$scriptVersion` variable to manage version display in a single location.
- Added script version display in a new "Final Summary" section at the end of script execution.
- Included script version in the "Final Summary" section of the log file.
### Changed
- Updated version display to use `$scriptVersion` for consistency in console and log outputs.
- Retained UTF-8 encoding for all log writes from version 2.5.9.
- Retained `Write-Log` function placement fix from version 2.6.0.

## [2.6.0] - 2025-08-16
### Fixed
- Resolved "Write-Log not recognized" error by moving `Write-Log` function definition before its first use.
### Changed
- Retained UTF-8 encoding for all log writes from version 2.5.9 to resolve "Unsupported text encoding" upload issues.
- Retained streamlined logging from version 2.5.8 (task start, command, filtered output, result, summary only).
- Retained script version logging at start ("Script Version: 2.6.0").
- Retained Windows 11-specific error patterns and recommendations in Log Analysis section.

## [2.5.9] - 2025-08-16
### Changed
- Updated log file encoding to UTF-8 for all writes to resolve "Unsupported text encoding" upload issues.
- Retained streamlined logging from version 2.5.8 (task start, command, filtered output, result, summary only).
- Retained script version logging at start ("Script Version: 2.5.9").
- Retained Windows 11-specific error patterns and recommendations in Log Analysis section.

## [2.5.8] - 2025-08-16
### Added
- Script version logged at the start of the log file.
- Enhanced Log Analysis section with Windows 11-specific error/repair patterns and recommendations.
### Changed
- Streamlined logging to remove redundant status lines (e.g., repetitive progress, "End of" unless critical).
- Improved console output in OS Maintenance to clearly display DISM results (e.g., "Result: Success: No corruption detected").
- Added full command to description line for each step (e.g., "dism /online /cleanup-image /checkhealth").

## [2.5.7] - 2025-08-16
### Added
- Timeout (1 hour) and stalled progress detection (10 min) for DISM /restorehealth to prevent hangs.
- Pre-check for internet connectivity if no repair source is specified.
- Prompt to run SFC /scannow before /restorehealth if not already run.
- Script version display in console.
### Fixed
- Corrected DISM command execution error by updating `$osCommands` to store only arguments and adjusting `Start-Process`.
