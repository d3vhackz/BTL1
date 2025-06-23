# 4.5: Evidence Handling and Integrity

Proper evidence handling is the cornerstone of a forensically sound investigation. Mistakes can taint evidence, rendering it inadmissible in legal proceedings and unreliable for internal analysis.

---

## Digital Evidence

**Digital evidence** is any probative information stored or transmitted in digital form. This can include emails, logs, files, browser history, photos, and messages.

> **Key Principle**: Digital evidence is easily modified. Any single piece of evidence should be treated with caution and verified with corroborating data before being trusted.

---

## Core Principles of Evidence Handling

1.  **Do No Harm**: Actions taken should not alter the original evidence. Whenever possible, analysis should be performed on a forensic copy, not the original media.
2.  **Use Write-Blockers**: A **write-blocker** is a device or software that prevents an operating system from making any changes to a storage device.
    -   **Hardware Write-Blockers**: Intercept signals at the hardware level, work with any OS, and are the preferred standard.
    -   **Software Write-Blockers**: Work at the OS level and are specific to that OS.
3.  **Document Everything**: Every action taken during an investigation must be meticulously documented. "If you didn't write it down, it didn't happen." This documentation forms the basis of the **Chain of Custody**, a log that tracks the control, transfer, and analysis of evidence.

---

## Hashing and Evidence Integrity

A **hash** is a unique digital fingerprint of a file or piece of data. The most common algorithms are MD5, SHA1, and **SHA256 (the modern standard)**.

### How Hashing Ensures Integrity
1.  **Acquisition**: When a storage device is collected, a hash value of the entire drive is calculated.
2.  **Imaging**: A bit-for-bit forensic image (an exact copy) of the device is created.
3.  **Verification**: A hash of the new forensic image is calculated.
4.  **Comparison**: The two hash values must match exactly. If they do, it proves the copy is identical to the original and has not been tampered with. This verification process is fundamental to all forensic work.

---

## The Order of Volatility

Volatile evidence is data that can be lost when a system is powered down. Evidence should be collected in order from most volatile to least volatile to minimize data loss, as outlined in **IETF RFC 3227**.

1.  **Registers & Cache**: CPU data, extremely volatile and constantly changing.
2.  **Memory (RAM)**: Running processes, network connections, encryption keys. Lost on power down.
3.  **Network State**: Active connections, ARP cache.
4.  **Running Processes**: Information about what programs are currently executing.
5.  **Disk (HDD/SSD)**: Data on storage drives. Considered non-volatile but can be altered by running processes.
6.  **Remote Logging/Monitoring Data**: Logs stored on a separate system.
7.  **Physical Configuration & Archived Media**: Network topology diagrams and backups. Least volatile.

[⬆️ Back to Digital Forensics](./README.md)
