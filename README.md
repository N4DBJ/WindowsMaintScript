# System Maintenance Script

## Overview

This PowerShell script automates comprehensive system maintenance tasks on Windows by running DISM, SFC, CHKDSK, Contig, defrag, temp file cleanup, hardware checks, driver checks, memory diagnostics, malware scans, and Event Log analysis, with user confirmation prompts. It logs all command outputs to a file named with the hostname and date (e.g., `CombinedSystemMaintenanceLog_DESKTOP123_20250816.txt`) and analyzes logs (`DISM.log`, `CBS.log`, Event Logs) to provide a detailed summary of errors, repairs, and recommendations. It supports multiple NTFS drives, SSD/HDD detection, optional repair sources for DISM, and Sysinternals Suite integration. Results are displayed on-screen after each command and section to guide user decisions (e.g., skipping `DISM /restorehealth` if no corruption is found). DISM commands show real-time progress on the console, and logs exclude repetitive DISM progress lines for clarity.

## Features

* Executes the following commands with Yes/No prompts (default: Yes unless specified):
  - **OS Maintenance**:
    - `dism /online /cleanup-image /checkhealth`: Checks component store for corruption. If no corruption, prompts to skip `scanhealth` and `restorehealth` (default "No"). Shows real-time progress on console.
    - `dism /online /cleanup-image /scanhealth`: Performs a deeper corruption scan. If no corruption, prompts to skip `restorehealth` (default "No"). Shows real-time progress on console.
    - `dism /online /cleanup-image /restorehealth`: Repairs component store (optional `/source` for repair files). Shows real-time progress on console.
    - `dism /online /cleanup-image /AnalyzeComponentStore`: Analyzes component store size. If cleanup not recommended, prompts to skip `startcomponentcleanup` (default "No"). Shows real-time progress on console.
    - `dism /online /cleanup-image /StartComponentCleanup`: Cleans up superseded components. Shows real-time progress