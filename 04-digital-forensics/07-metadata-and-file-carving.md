# 4.7: Metadata and File Carving

This section covers practical techniques for extracting hidden data from files (metadata) and recovering deleted files from a disk image (file carving).

---

## Metadata Analysis

**Metadata** is "data about data." It is information embedded within a file that describes it, such as its author, creation date, and more. Metadata can provide crucial context during an investigation.

### Extracting Metadata
-   **Windows**: Right-click a file > `Properties` > `Details` tab.
-   **Linux**: Right-click a file > `Properties`, or use command-line tools.
-   **`exiftool`**: A powerful command-line tool for extracting comprehensive metadata from a wide variety of file types (images, documents, etc.).
    -   **Installation**: `sudo apt-get install exiftool`
    -   **Usage**: `exiftool <filename>`

> **Example**: The metadata of a photograph can reveal the exact camera model used, GPS coordinates of where the photo was taken, and the date and time.

---

## File Carving

**File carving** is the process of recovering files from a data stream or disk image based on their file signatures (headers and footers), rather than relying on file system metadata. This technique is essential for recovering deleted files.

### Tool: `scalpel`
`scalpel` is a command-line file carving tool for Linux. It reads a configuration file to know which file types to search for.

#### File Carving Workflow with `scalpel`
1.  **Configure `scalpel`**:
    *   Open the configuration file: `sudo nano /etc/scalpel/scalpel.conf`
    *   Find the line corresponding to the file type you want to recover (e.g., `jpg`).
    *   Uncomment the line by removing the `#` at the beginning. Save and exit.

2.  **Run `scalpel`**:
    *   Use the following command syntax:
        ```bash
        sudo scalpel -o <output_directory> <disk_image_file>
        ```
    *   `-o`: Specifies an output directory for recovered files. This directory must be empty or non-existent.
    *   **Example**: `sudo scalpel -o recovered_files disk_image.img`

3.  **Review the Output**:
    *   `scalpel` will create the specified output directory and populate it with subdirectories for each file type it recovered (e.g., `jpg-0-0`).
    *   It also creates an `audit.txt` file detailing the carving process.

---

## Managing Permissions with `chown`

During forensic analysis, you may encounter files or directories that your user account does not have permission to access. The `chown` (change owner) command is used to take ownership.

-   **Syntax**: `chown [new_owner] [file_or_directory]`
-   **Forensic Use Case**: After using `scalpel`, the output directory might be owned by the `root` user. To access the recovered files, you may need to take ownership.
-   **Example**:
    ```bash
    # Take ownership of the 'recovered_files' directory
    sudo chown your_username recovered_files
    ```

[⬆️ Back to Digital Forensics](./README.md)
