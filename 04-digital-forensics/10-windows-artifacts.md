# 4.10 Windows Artifacts

This section focuses on the common artifacts retrieved from systems running Microsoft Windows. Understanding these artifacts is essential for conducting forensic investigations to trace user activity, identify executed programs, and reconstruct events on a system.

## Program Execution Artifacts

Analyzing artifacts related to application execution can reveal which programs were run, their execution times and frequency, and their location on the file system.

### LNK Files (Shortcuts)

LNK files are shortcuts that link to another file or application. They contain valuable metadata for an investigation.

*   **Artifact Description:** Provides the target file's location, file size, and timestamps for creation, modification, and last access.
*   **Artifact Location:** `C:\Users\$USER$\AppData\Roaming\Microsoft\Windows\Recent`
*   **Analysis Tool:** `Windows File Analyzer`

### Prefetch Files (.pf)

Prefetch files are created by Windows to speed up application startup. They serve as a forensic record of program execution.

*   **Artifact Description:** Provides the application name, executable path, run count, and timestamps for last run and creation/installation.
*   **Artifact Location:** `C:\Windows\Prefetch`
*   **Analysis Tool:** `Prefetch Explorer Command Line (PECmd.exe)`

### Jump Lists

Jump Lists store information about recently accessed applications and files, particularly those pinned to the taskbar.

*   **Artifact Description:** Contains file paths, timestamps, and Application IDs (AppIDs) related to user activity. Files are named `automaticDestination-ms` or `customDestination-ms`.
*   **Artifact Location:**
    > `C:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations`
    > `C:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Recent\CustomDestinations`
*   **Analysis Tool:** `JumpList Explorer`

## Browser Artifacts

Browser data is a primary source of evidence, revealing visited websites, search history, downloads, and cached content.

*   **Key Artifacts:** Cookies, Favorites, Download History, Visited URLs, Searches, Cached Pages & Images.
*   **Analysis Tools:**
    *   **KAPE (Kroll Artifact Parser and Extractor):** For targeted collection of browser files.
    *   **Browser History Capturer (BHC) & Viewer (BHV):** For acquiring and analyzing browser history from a live system.

### Acquisition & Analysis Workflow
1.  **Capture:** Use a tool like KAPE or BHC to collect browser files (e.g., history databases, cache) from the user's profile directory. BHC is particularly useful on live systems as it can copy files locked by active browser processes.
2.  **Analyze:** Load the captured files into an analysis tool like BHV. This allows an investigator to view browsing history, see cached images, and render cached webpages to see what the user saw. The tool also provides powerful filtering by date, keyword, and browser.

## Logon Event Artifacts

Windows Event Logs provide a detailed record of user logon/logoff activity, which is crucial for building a timeline and attributing actions to an account.

*   **Artifact Location:** The Security Event Log (`C:\Windows\System32\winevt\Logs\Security.evtx`).
*   **Analysis Tool:** Windows Event Viewer, SIEM platforms.

### Key Event IDs

| Event ID | Description                                                                                               |
| :------- | :-------------------------------------------------------------------------------------------------------- |
| **4624** | **Successful Logon:** An account successfully logged on. The `Logon Type` specifies how (e.g., Interactive, Network, Unlock). |
| **4672** | **Special Logon:** A logon by a user with administrative privileges.                                       |
| **4625** | **Failed Logon:** An account failed to log on. The `Status Code` gives the reason (e.g., bad password, disabled account). |
| **4634** | **Logoff:** An account logged off.                                                                          |

> By correlating the `Logon ID` between logon (4624/4672) and logoff (4634) events, an investigator can determine the precise duration of a user session.

## Recycle Bin Artifacts

The Recycle Bin (`C:\$Recycle.Bin`) temporarily stores deleted files and is a key source for recovering data and tracing a user's intent to delete files.

*   **Artifact Location:** `C:\$Recycle.Bin` (a hidden system folder).
*   **Analysis Tool:** `RBCmd.exe` by Eric Zimmerman.

### Technical Analysis
1.  The `$Recycle.Bin` directory contains sub-folders named with the Security IDs (SIDs) of system users. Use `wmic useraccount get name,SID` to map an SID to a username.
2.  Inside each SID folder, deleted items are stored in two parts:
    *   **`$R...` file:** The actual contents of the deleted file.
    *   **`$I...` file:** The metadata, including original filename, path, size, and deletion timestamp.
3.  Use `RBCmd.exe` to parse the `$I` metadata files. The `-d` flag can be used to parse an entire directory and `--csv` to export the results to a CSV file for analysis.

> **Note:** If a user empties the Recycle Bin, these artifacts are removed. However, the underlying file data may still be recoverable from unallocated disk space using file carving techniques.

[⬆️ Back to Digital Forensics](./README.md)
