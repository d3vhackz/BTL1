# 2.3: Defensive Measures

Defensive measures are categorized as either **preventative** (proactive controls to stop attacks) or **reactive** (actions taken after an attack is identified).

---

## Preventative Measures

These technical controls and user-focused initiatives help reduce the likelihood of a successful phishing attack.

### Anti-Spoofing DNS Records
These records are configured in your domain's DNS to help receiving mail servers verify that an email is legitimate.
-   **SPF (Sender Policy Framework)**: A TXT record listing all IP addresses authorized to send email on behalf of your domain.
-   **DKIM (DomainKeys Identified Mail)**: Adds a cryptographic signature to emails, ensuring the message was not altered in transit.
-   **DMARC (Domain-based Message Authentication, Reporting & Conformance)**: Tells receiving servers what to do if SPF or DKIM checks fail (`p=reject`, `p=quarantine`, or `p=none`) and provides reporting on spoofing attempts.

### Filtering and Sandboxing
-   **Spam Filtering**: Uses rules, heuristics, and blacklists to block unsolicited mail.
-   **Attachment Filtering**: Blocks emails based on risky file types (`.exe`, `.js`, `.vbs`).
-   **Attachment Sandboxing**: Automatically detonates attachments in a safe environment to analyze their behavior before delivery.

### Human Layer
-   **User Awareness Training**: Educates employees on how to identify phishing red flags (e.g., urgency, poor grammar, suspicious links).
-   **Simulated Phishing Attacks**: Tests employee awareness and identifies training gaps by sending safe, mock phishing emails.

---

## Reactive Measures

These are the immediate steps taken to contain a threat and remediate the environment after a phishing email has been identified.

### Immediate Response Steps
1.  **Retrieve** an original copy of the email for analysis.
2.  **Gather** all relevant artifacts (sender, subject, URLs, hashes).
3.  **Notify** recipients who received the email with clear instructions (e.g., "Delete immediately").
4.  **Analyze** all artifacts to confirm malicious intent.
5.  **Take Defensive Action** by implementing blocks.
6.  **Complete** an investigation report.

### Defensive Blocking Tactics

| Control Point         | Blocking Method                                          | When to Use                                                          |
|-----------------------|----------------------------------------------------------|----------------------------------------------------------------------|
| **Email Gateway**     | Block sender address, domain, subject, or IP.            | To stop delivery of similar phishing emails.                         |
| **Web Proxy / Firewall** | Block malicious URLs or entire domains.                  | To prevent users from visiting malicious sites.                      |
| **DNS Blackholing**   | Redirect malicious domain requests to a safe page.       | To block access and provide educational feedback to the user.        |
| **EDR / AV**          | Block file hashes.                                       | To prevent a malicious attachment from executing on any endpoint.    |

> **Decision Point**: When blocking, always ask: "Is this infrastructure exclusively malicious?" Use WHOIS and visualization tools to verify. Blocking a legitimate, compromised domain requires a more targeted URL block.

[⬆️ Back to Phishing Analysis](./README.md)