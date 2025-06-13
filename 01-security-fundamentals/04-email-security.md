# 1.4: Email Security

Email is a primary vector for cyberattacks. These controls help protect against spam, phishing, and data exfiltration.

---

## Spam Filtering

**Spam filtering** is a security control that scans incoming emails to detect and block unsolicited, malicious, or irrelevant messages. It is a first line of defense against phishing and malware.

### How It Works
-   **Pattern Matching**: Looks for known spam formats and phrases.
-   **Blacklists (RBLs)**: Blocks emails from known spam-sending domains or IPs.
-   **Heuristic Rules**: Uses a scoring system based on suspicious traits (e.g., excessive links, strange formatting).
-   **Machine Learning**: Adapts to new spam trends and user-defined classifications.

---

## Email Scanning

**Email scanning** analyzes all components of an email—headers, body, and attachments—to detect and block malicious content before it reaches the user.

### What It Scans
-   **Headers**: Verifies sender domain (SPF, DKIM, DMARC), IP reputation, and timestamps.
-   **Body Content**: Looks for phishing language, suspicious formatting, and malicious URLs.
-   **Attachments**: Analyzes file types and checks for embedded malware, macros, or scripts.
-   **Links**: Scans URLs against malicious destination databases.

### Detection Methods
-   **Signature-Based**: Matches known malware fingerprints.
-   **Heuristic-Based**: Uses rules to detect suspicious behavior.
-   **Sandboxing**: Executes suspicious attachments in an isolated environment to observe behavior.

---

## Data Loss Prevention (DLP)

**Data Loss Prevention (DLP)** is a security control designed to prevent sensitive or confidential information from leaving an organization in an unauthorized manner. It turns data handling policies into enforcement.

### What DLP Protects
-   **PII**: Personally Identifiable Information (Social Security numbers, addresses).
-   **Financial Data**: Credit card numbers, bank details.
-   **Intellectual Property**: Source code, strategic plans, internal documents.

### How It Works
DLP systems scan outbound communications (like email) for content that matches predefined policies.
-   **Pattern Matching**: Uses regex to find patterns like credit card or Social Security numbers.
-   **Keyword Detection**: Flags sensitive words like "confidential" or "internal use only."
-   **Policy Enforcement**: If a violation is detected, the system can block, quarantine, or log the communication and alert the security team.

[⬆️ Back to Security Fundamentals](./README.md)