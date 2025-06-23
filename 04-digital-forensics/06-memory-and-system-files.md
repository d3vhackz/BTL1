# 4.6: Memory and System Files

Some of the most critical digital evidence resides not in user files, but in system memory and special files used by the operating system to manage it.

---

## Memory Analysis (Memory Forensics)

**Memory forensics** is the analysis of volatile data from a computer's memory (RAM) dump. It is crucial for investigating advanced threats that may not leave traces on the hard drive.

### Why Memory Forensics is Important
-   **Fileless Malware**: Detects malware that resides entirely in memory to evade AV detection.
-   **Runtime Activity**: Captures data that exists only when the system is running, such as:
    -   Active network connections.
    -   Running processes and injected code.
    -   Loaded command-line arguments.
    -   User credentials, chat messages, and encryption keys.

> **Key Takeaway**: Any program, malicious or not, must be loaded into memory to execute. This makes memory analysis a vital part of modern DFIR.

---

## Windows System Files

### Pagefile (`Pagefile.sys`)
The pagefile is a hidden system file on the hard drive that Windows uses as "virtual memory."
-   **Function**: When physical RAM becomes full, Windows moves the least-used "pages" of memory from RAM to `Pagefile.sys` to free up space.
-   **Forensic Value**: It can contain fragments of data that were once in active memory, including passwords, encryption keys, and remnants of program activity, even after a reboot.

### Hibernation File (`hiberfil.sys`)
The hibernation file is created when a Windows system hibernates.
-   **Function**: To enable hibernation, the OS saves the entire contents of RAM to `hiberfil.sys` before powering down. When the computer resumes, it loads this file back into RAM to restore the exact previous state.
-   **Forensic Value**: It is a complete snapshot of the system's memory at a specific point in time. Analyzing this file can be as valuable as analyzing a live memory dump, revealing running processes and other volatile data.

---

## Linux System Files

### Swap Space
Swap space in Linux serves the same purpose as the Windows pagefile.
-   **Implementation**: It can be configured as either a **swap partition** (a dedicated section of the drive) or a **swap file**.
-   **Forensic Value**: Like the pagefile, swap space can contain valuable data that has been moved out of active RAM.

### Key Linux Commands
-   `free -h`: Checks the amount of available memory and swap space.
-   `swapon --show`: Identifies whether swap space is a file or a partition.

[⬆️ Back to Digital Forensics](./README.md)
