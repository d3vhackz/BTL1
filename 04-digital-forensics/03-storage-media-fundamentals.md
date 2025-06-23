# 4.3: Storage Media Fundamentals

Digital evidence is primarily stored on Hard Disk Drives (HDDs) and Solid-State Drives (SSDs). Understanding their mechanics is crucial for data recovery and analysis.

---

## Hard Disk Drives (HDD)

HDDs are mechanical drives that store data magnetically on rotating platters.

-   **Platters**: Circular disks where magnetic data is stored.
-   **Sectors**: The smallest physical storage unit on a platter, traditionally 512 bytes.
-   **Clusters**: A group of sectors, representing the smallest logical amount of disk space that an operating system can allocate to a file.

### Slack Space: A Forensic Goldmine
**Slack space** is the unused space in a cluster between the end of a file and the end of the cluster.
-   **Why it's important**: This space is not wiped when a file is saved and can contain remnants of previously deleted files that once occupied that cluster. Examining slack space can reveal hidden or deleted evidence.

---

## Solid-State Drives (SSD)

SSDs are flash-based storage devices with no moving parts, making them significantly faster than HDDs. Data is written to "pages," which are grouped into "blocks."

### Forensic Challenges with SSDs
Modern SSDs use internal processes that can actively destroy data, posing a significant challenge to forensic investigators.

-   **Garbage Collection**: An automated process where the SSD controller reorganizes data to free up empty blocks for writing. It moves valid data from a block and erases the rest, potentially destroying fragments of deleted files.
-   **TRIM**: A command that tells the SSD which data blocks are no longer in use (e.g., from a deleted file). Unlike on an HDD where data remains until overwritten, TRIM allows the SSD to immediately erase this data, making recovery nearly impossible.
-   **Wear Leveling**: A technique that distributes writes evenly across all memory cells to extend the SSD's lifespan. This constantly moves data around, making it difficult to determine the original physical location of a file.

> **Critical Forensic Procedure for SSDs:**
> If a system with an SSD contains potential evidence, it must be powered off **immediately** by a hard shutdown (holding the power button or pulling the plug). A normal OS shutdown could trigger TRIM or garbage collection commands, permanently destroying volatile evidence.

[⬆️ Back to Digital Forensics](./README.md)
