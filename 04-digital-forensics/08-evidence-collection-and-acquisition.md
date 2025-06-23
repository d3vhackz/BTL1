# 4.8: Evidence Collection and Acquisition

This section covers the practical aspects of collecting digital evidence, including the necessary equipment and the techniques for acquiring data from both powered-off (dead) and running (live) systems.

---

## Forensic Equipment Toolkit

Proper evidence collection requires a specialized toolkit to prevent tampering and ensure data integrity.

| Equipment                      | Purpose                                                                                             |
|--------------------------------|-----------------------------------------------------------------------------------------------------|
| **Forensic Workstation/Laptop**| A dedicated machine with forensic software (e.g., CAINE, FTK) for on-site analysis and acquisition.    |
| **Hardware Write-Blockers**    | Physical devices that permit read-only access to storage media, preventing any accidental writes.   |
| **Blank Hard Drives**          | High-capacity, forensically wiped drives used to store forensic images of suspect media.          |
| **Electro-Static Evidence Bags**| Protects sensitive electronic components from static discharge during transport and storage.        |
| **Tamper-Evident Seals/Stickers** | Provides visual proof if a bag or container has been opened, ensuring evidence integrity.         |
| **Labels & Documentation**     | For meticulously documenting every piece of evidence and maintaining the Chain of Custody.         |
| **Digital Camera**             | To photograph the scene, showing how devices were connected and their physical state.             |
| **Faraday Bags/Boxes**         | Blocks all wireless signals (Wi-Fi, Cellular, Bluetooth) to prevent remote wiping or tampering.     |

---

## Live Forensics

**Live forensics** is the acquisition of volatile data from a system that is still powered on. This is crucial because shutting a system down can destroy critical evidence that exists only in memory.

### Why Live Forensics is Important
-   **Volatile Data**: Captures RAM, running processes, active network connections, and cache data that would be lost on shutdown.
-   **Bypassing Encryption**: Full-disk encryption can make a powered-off drive unreadable. Live forensics allows an analyst to potentially extract encryption keys from memory or acquire data before the system is shut down.
-   **Remote Acquisition**: Enables a centralized security team to remotely investigate a system in a different geographical location without needing physical access.

---

## Disk Imaging with FTK Imager

**FTK Imager** is a powerful, free tool for creating forensically sound copies (images) of storage devices and memory.

### Key Features
-   Create bit-by-bit forensic images of hard drives (e.g., in `.img` or `.E01` format).
-   Capture the live memory (RAM) of a running system.
-   Mount and preview the contents of a disk image in a read-only view.
-   Generate hash values to verify data integrity.

### Workflow for Imaging a Hard Drive
1.  Connect the suspect drive to the forensic workstation via a hardware write-blocker.
2.  In FTK Imager, select `File > Create Disk Image`.
3.  Choose the source evidence type (e.g., `Physical Drive`).
4.  Select the suspect drive from the list.
5.  Add evidence information (case number, examiner, etc.) for the Chain of Custody.
6.  Choose the destination, image type (`raw (dd)` is common), and filename.
7.  Start the imaging process. FTK Imager will perform a hash verification at the end to ensure the copy is identical to the original.

---

## Live Acquisition with KAPE

The **Kroll Artifact Parser and Extractor (KAPE)** is a highly efficient triage tool designed for rapid live acquisition. It targets forensically useful artifacts and parses them within minutes.

### How KAPE Works
1.  **Target Selection**: The analyst chooses what to collect from a list of predefined "Targets" (e.g., web browser history, Windows Event Logs, Registry hives).
2.  **Module Application**: The analyst can optionally apply "Modules" to process the collected data automatically (e.g., parse event logs into a readable format).
3.  **Execution**: KAPE runs on the live system (or against a mounted image), copies the targeted artifacts to a designated location, and executes any modules.

> **Use Case:** While a full disk image is being acquired (which can take hours), an analyst can run KAPE to immediately collect and analyze high-value artifacts like browser history, prefetch files, and memory, potentially generating investigative leads in minutes.

[⬆️ Back to Digital Forensics](./README.md)
