# 4.4: File Systems

A **file system** controls how data is stored, organized, and retrieved on a storage device. Different operating systems use different file systems, each with its own structure, logic, and features.

---

## Windows File Systems

### FAT (File Allocation Table)
-   **FAT16**: An older file system used in DOS and early Windows. Limited to small partition sizes.
-   **FAT32**: An update to FAT16 with larger partition support (up to 8 TB) and compatibility across a vast range of devices (Windows, macOS, Linux, cameras, game consoles).
    -   **Major Limitation**: Cannot store individual files larger than 4 GB. Lacks modern security features like encryption and permissions.

### NTFS (New Technology File System)
The default file system for modern Windows versions. It offers significant improvements over FAT.
-   **Key Features**:
    -   Support for very large files and partitions.
    -   **File System Journaling**: Improves reliability by keeping a log of changes, allowing for faster recovery from system crashes.
    -   **Security**: Supports robust file and folder permissions through Access Control Lists (ACLs).
    -   Includes built-in compression and encryption (EFS).

---

## Linux File Systems

### Ext (Extended File System)
-   **Ext3**: A widely used journaled file system in many Linux distributions. The journaling feature significantly reduces the time needed for data recovery after a crash.
-   **Ext4**: The default for most modern Linux distributions. It improves on Ext3 with support for larger volumes (up to 1 exbibyte), larger file sizes, and better performance through the use of **extents**, which reduces file fragmentation.

---

## Identifying File Systems with FTK Imager

You can use a tool like **FTK Imager** to identify the file system of a disk image.

### Steps:
1.  In FTK Imager, go to `File` > `Add Evidence Item...`.
2.  Select **"Image File"** as the source type and click `Next`.
3.  Browse to and select your disk image file (e.g., `.img`, `.e01`).
4.  Click `Finish`.
5.  In the **Evidence Tree** pane, FTK Imager will display the partition information, including the detected file system type (e.g., NTFS, FAT32, Ext4).

[⬆️ Back to Digital Forensics](./README.md)
