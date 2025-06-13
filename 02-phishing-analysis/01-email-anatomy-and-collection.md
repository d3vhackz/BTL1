# 2.1: Email Anatomy and Artifact Collection

A successful phishing investigation begins with understanding email structure and knowing how to collect key artifacts accurately and safely.

---

## Email Anatomy

Every email consists of headers, a body, and sometimes attachments. Attackers manipulate these components to deceive users and bypass security controls.

### Key Email Header Fields
-   **`From:`**: The sender’s display address (easily spoofed).
-   **`Reply-To:`**: An alternate address for replies, often used to redirect responses to an attacker-controlled inbox.
-   **`Received:`**: A trace of all mail servers the message passed through, read from **bottom to top**. This is crucial for identifying the true origin IP address.
-   **`Message-ID:`**: A globally unique identifier for the email message.

---

## Artifact Collection

Collecting the right artifacts is essential for analysis, intelligence sharing, and implementing defensive controls. Always perform collection in a safe environment like a VM.

### Key Artifacts to Collect

| Category | Artifacts                                                    | Purpose                                                              |
|----------|--------------------------------------------------------------|----------------------------------------------------------------------|
| **Email**| Sender address, subject, recipient, sending IP, `Reply-To`.    | Search logs, identify campaign scope, and track infrastructure.      |
| **Web**  | Full URL, root domain.                                       | Check reputation, analyze landing pages, and block malicious sites.  |
| **File** | Attachment name, **SHA256 hash**.                            | Check reputation on VirusTotal and block malware via EDR.            |

### Collection Methods: Manual vs. Automated

#### Manual Collection
Manually extracting artifacts requires inspecting the email's source code or using the client interface.

-   **IPs and `Reply-To`**: Open the `.eml` file in a text editor and search for `X-Sender-IP` and `Reply-To`.
-   **URLs**: Right-click a hyperlink and "Copy Link Address." Never click it directly.
-   **Hashes**: Use built-in OS tools to generate file hashes.
    -   **PowerShell**: `Get-FileHash C:\path\to\file -Algorithm SHA256`
    -   **Linux**: `sha256sum /path/to/file`

#### Automated Collection with PhishTool
Tools like **PhishTool** automate artifact extraction from `.eml` or `.msg` files.

1.  **Upload Email**: Drag and drop the email file into the console.
2.  **Automated Parsing**: PhishTool automatically extracts headers, URLs, attachments, and file hashes.
3.  **One-Click Analysis**: Provides direct links to perform VirusTotal lookups, WHOIS queries, and safe URL visualization.

> **Key Takeaway**: While automation drastically speeds up collection, proficiency in manual methods is essential for situations where tools are unavailable.

[⬆️ Back to Phishing Analysis](./README.md)