# 2.4: Report Writing

A well-written phishing investigation report allows other analysts and decision-makers to quickly understand the threat, the analysis performed, and the response taken. Clarity, completeness, and actionability are key.

---

## Report Structure

A strong report should be structured into clear sections.

### 1. Summary & Artifacts
-   **One-Sentence Summary**: Briefly describe the phishing attempt (e.g., "Credential harvesting attempt impersonating Amazon.").
-   **Header Artifacts**:
    -   Sender Address & IP
    -   Subject Line
    -   Recipients
    -   Date & Time
-   **Web & File Artifacts**:
    -   URLs (sanitized)
    -   Attachment Filename & Hashes (MD5, SHA256)
-   **Email Screenshot**: Include a picture of the email if possible.

### 2. Analysis Process & Results
Document the steps you took to analyze each artifact and what you discovered.
-   **Tools Used**: VirusTotal, URLScan, Hybrid Analysis, WHOIS, etc.
-   **Findings**: State the verdict for each artifact (e.g., "URL hosted a fake login page," "File hash detected as malware by 61/71 vendors.").
-   **Justification**: Explain why the artifact is considered malicious based on the evidence.

#### Example: URL Analysis Section
> **URL:** `hxxps://maliciousdomainexample[.]com/login.aspx`  
> - **WHOIS**: Domain was registered 3 days ago via Namecheap.
> - **URLScan.io**: Screenshot confirms a spoofed Microsoft 365 login page.
> - **VirusTotal**: 0/90 detections; domain is too new for most blacklists.  
> **Verdict**: Malicious. The site is a credential harvester.

### 3. Defensive Measures Taken
Summarize the actions you took or requested to block the threat. Be specific.
-   **Email Gateway**: Blocked sender domain `maliciousdomainexample[.]com`.
-   **Web Proxy**: Blocked URL `hxxps://maliciousdomainexample[.]com/*`.
-   **SIEM**: Created an alert for the sender IP `123.45.67.89`.

---

## Artifact Sanitization (Defanging)

**Defanging** is the practice of modifying URLs and IPs to make them non-hyperlinked and prevent accidental clicks. This is a professional standard for all technical reports.

-   **Rule**: Replace `http` with `hxxp` and `.` with `[.]`.

#### Examples
-   **URL**: `https://google.com` becomes `hxxps://google[.]com`
-   **IP Address**: `8.8.8.8` becomes `8[.]8[.]8[.]8`

> **Pro Tip**: Use a tool like CyberChef with the "Defang URL" and "Defang IP Addresses" operations for quick and accurate sanitization.

[⬆️ Back to Phishing Analysis](./README.md)