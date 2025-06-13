# 4.1: Introduction to Digital Forensics

**Digital Forensics** is the process of collecting, analyzing, and preserving digital evidence in a manner that is legally admissible. While often associated with law enforcement, its principles are vital for corporate incident response (where it's known as **DFIR**).

---

## The Digital Forensics Process

Forensic investigations follow a strict, scientific methodology to ensure findings are repeatable and verifiable.

1.  **Identification**: Locate potential sources of digital evidence (e.g., laptops, servers, mobile devices).
2.  **Preservation**: Secure and isolate evidence to prevent tampering or modification. This includes maintaining a strict **Chain of Custody**.
3.  **Collection**: Extract and create a bit-for-bit forensic image of the relevant data.
4.  **Analysis**: Examine the collected data to identify artifacts, timelines, and draw conclusions.
5.  **Reporting**: Document all findings, tools, and methods in a clear, comprehensive report.

> **Contemporaneous Notetaking**: A crucial companion to all steps, ensuring that every action taken is documented in real-time.

---

## Core Principles

-   **Chain of Custody**: A detailed log documenting the handling of evidence from collection to presentation. It tracks who had the evidence, when, and what they did with it, ensuring its integrity.
-   **Write-Blocking**: Using a hardware or software tool that prevents any writes to the evidence drive during examination, preserving its original state.

---

## Key Tools and Terms

| Term / Tool     | Description                                                                              |
|-----------------|------------------------------------------------------------------------------------------|
| **IOC**         | **Indicator of Compromise**: Forensic data proving malicious activity (e.g., malware hashes, C2 IPs). |
| **TTP**         | **Tactics, Techniques, and Procedures**: Adversary behaviors, often mapped to MITRE ATT&CK. |
| **PCAP**        | **Packet Capture**: A file containing captured network traffic for analysis in tools like Wireshark. |
| **Write-Blocker** | A device that prevents modification of evidence during acquisition.                      |
| **FTK Imager**  | A popular tool for creating forensic images (bit-for-bit copies) of storage media.         |
| **KAPE**        | **Kroll Artifact Parser and Extractor**: A fast and efficient tool for collecting forensic artifacts from a live system or image. |
| **/etc/passwd** | A Linux file listing user accounts.                                                      |
| **/etc/shadow** | A Linux file storing encrypted user passwords.                                           |

[⬆️ Back to Digital Forensics](./README.md)