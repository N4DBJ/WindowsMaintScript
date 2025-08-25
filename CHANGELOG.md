# Changelog:
## Version 2.8.5 - August 24, 2025
#   2.8.6 (2025-08-24)
#     - Fixed parsing error in Write-Log statement for temp file cleanup by using ${folder} to properly delimit the variable name before the colon.
#   2.8.5 (2025-08-24)
#     - Fixed Contig command by escaping the dollar sign in the MFT path to prevent PowerShell from interpreting `$mft` as a variable. This ensures the correct path (e.g., `C:\$mft`) is passed to Contig.exe.
#     - Updated logging and console messages to reflect the escaped path for clarity and accuracy.
#   2.8.4 (2025-08-20):
#     - Fixed Optimization Section to target specific drives (SSD or non-SSD) instead of using /C parameter, ensuring user choice is respected.
#     - Added verbose comments throughout for clarity.
#     - Retained all fixes and features from 2.8.3, including completed Log Analysis and server-specific patterns.
#   2.8.3 (2025-08-19):
#     - Fixed truncation in Log Analysis section: completed pattern matching for cleanup and defender errors.
#     - Added counts for CHKDSK errors and repairs in Log Analysis.
#     - Enhanced Log Analysis with Windows Server-specific patterns (e.g., WSUS-related errors).
#     - Completed Final Summary section with all summary variables, recommendations, and script version.
#     - Added OS compatibility checks for Windows 10/11 and Server 2019+ (skips non-applicable features like memory diagnostic on servers).
#     - Improved error handling in process starts (added -Wait where needed).
#     - Updated README.md (assumed external) with server usage notes.
#     - Retained all fixes from 2.7.6, including syntax corrections in Log Analysis and Final Summary.
#   2.7.6 (2025-08-17):
#     - Fixed syntax error in Log Analysis section: missing string terminator in Write-Log summary statement.
#     - Fixed missing closing brace for Log Analysis 'if' block.
#     - Updated CHANGELOG.md to document version 2.7.6 fixes.
#     - Updated README.md to enhance syntax validation instructions with specific PSScriptAnalyzer rules.
#     - Retained all fixes from 2.7.5, including completed Log Analysis and Final Summary sections.
#   2.7.5 (2025-08-17):
#     - Fixed syntax errors in Log Analysis section: missing string terminators in Write-Log and Write-Host statements.
#     - Fixed missing closing brace for Log Analysis 'if' block.
#     - Updated CHANGELOG.md to document version 2.7.5 fixes.
#     - Updated README.md to enhance syntax validation instructions with PSScriptAnalyzer.
#     - Retained all fixes from 2.7.4, including completed Log Analysis and Final Summary sections.
#   2.7.4 (2025-08-17):
#     - Fixed syntax errors in Log Analysis section (missing closing parenthesis, string terminator, and braces).
#     - Completed Log Analysis section to properly display Contig errors and fragments.
#     - Completed Final Summary section with all summary variables and recommendations.
#     - Added comment to confirm closure of all script blocks.
#     - Updated README.md with syntax validation instructions.
#     - Retained all fixes from 2.7.3, including completed Final Summary section.
#   2.7.3 (2025-08-17):
#     - Fixed syntax error in Final Summary section (missing closing quote in Write-Log statement).
#     - Completed Final Summary section to include all summary variables and recommendations.
#     - Retained all fixes from 2.7.2, including Log Analysis section corrections.
#   2.7.2 (2025-08-17):
#     - Fixed syntax error in Log Analysis section (missing closing brace for 'if' block at Confirm-Action).
#     - Completed Final Summary section to include all summary variables and script version logging.
#     - Retained all fixes from 2.7.1, including Malware Scan section corrections.
#   2.7.1 (2025-08-17):
#     - Fixed syntax error in Malware Scan section (incomplete hash literal in Get-WinEvent, missing catch block, and unclosed braces).
#     - Restored Log Analysis and Final Summary sections that were truncated.
#     - Retained all features from 2.7.0, including $scriptVersion variable and Final Summary display/logging.
#   2.7.0 (2025-08-17):
#     - Added $scriptVersion variable to manage version display in one place.
#     - Added script version display in new Final Summary section at end of execution.
#     - Added script version to Final Summary in log file.
#     - Retained UTF-8 encoding for all log writes from 2.5.9.
#     - Retained Write-Log function placement fix from 2.6.0.
#   2.6.0 (2025-08-16):
#     - Fixed 'Write-Log' not recognized error by moving function definition before first use.
#     - Retained UTF-8 encoding for all log writes from 2.5.9 to resolve "Unsupported text encoding" upload issues.
#     - Retained streamlined logging from 2.5.8 (task start, command, filtered output, result, summary only).
#     - Retained script version logging at start ("Script Version: 2.6.0").
#     - Retained Windows 11-specific error patterns and recommendations in Log Analysis section.
#   2.5.9 (2025-08-16):
#     - Changed log file encoding to UTF-8 for all writes to resolve "Unsupported text encoding" upload issues.
#     - Retained streamlined logging from 2.5.8 (task start, command, filtered output, result, summary only).
#     - Retained script version logging at start ("Script Version: 2.5.9").
#     - Retained Windows 11-specific error patterns and recommendations in Log Analysis section.
#   2.5.8 (2025-08-16):
#     - Added script version to log file at start.
#     - Enhanced Log Analysis section with Windows 11-specific error/repair patterns and recommendations.
#     - Streamlined logging to remove redundant status lines (e.g., repetitive progress, "End of" unless critical).
#     - Enhanced console output in OS Maintenance to clearly display DISM results (e.g., "Result: Success: No corruption detected").
#     - Added full command to description line for each step (e.g., "dism /online /cleanup-image /checkhealth").
#   2.5.7 (2025-08-16):
#     - Added timeout (1 hour) and stalled progress detection (10 min) for DISM /restorehealth to prevent hangs.
#     - Added pre-check for internet connectivity if no repair source is specified.
#     - Added prompt to run SFC /scannow before /restorehealth if not already run.
#     - Added script version display in console.
#     - Fixed DISM command execution error by updating $osCommands to store only arguments and adjusting Start-Process.