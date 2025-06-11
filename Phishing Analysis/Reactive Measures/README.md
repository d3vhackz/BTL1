# Reactive Measures

This section outlines the immediate steps analysts should take after identifying a phishing email. These actions help contain the threat, inform affected users, and begin defensive remediation.

---

## Immediate Response Steps

### 1. Retrieve an Original Copy of the Email

- Methods:
  - Email gateway logs (e.g., Proofpoint, Mimecast)
  - Mail server access (e.g., Exchange)
  - Forwarded copy to a security-monitored inbox

### 2. Gather Artifacts

Collect email, web, and file-based artifacts as previously covered. These include:
- Sender address and domain
- Sending server IP
- Subject line
- URLs, domains, and attachments

### 3. Notify Recipients

Inform employees who received the phishing email:
- Send a templated message with:
  - **Date and time** of the email
  - **Subject line**
  - **Instructions** (delete or forward)
  - **Contact info** for security team
- Use BCC to avoid revealing recipients to each other

### 4. Analyze Artifacts

Investigate all artifacts:
- **Email headers, attachments, and links**
- Use tools like:
  - VirusTotal
  - URLScan
  - Hybrid Analysis
  - WHOIS lookup
  - Internal sandboxing tools
  - Virtual machines for detonation

### 5. Take Defensive Measures

Implement blocks based on artifacts to prevent future delivery or interaction:
- **Email Gateway Blocks**
  - Sender address or domain
  - Subject line
  - Sending IP (only when justified)
- **Web Proxy & DNS Blocks**
  - Block malicious URLs or domains
  - Use DNS blackholing for educational feedback
- **Attachment Handling**
  - Quarantine or strip suspicious files
  - Enable attachment sandboxing where possible

### 6. Complete Investigation Report

Document:
- How the email was identified
- What was discovered during analysis
- Actions taken to mitigate risk
- Lessons learned

This serves as an audit trail and a resource for improving future response.

---

## Defensive Blocking Tactics

### Email Gateway

**Sender Address:**  
Block messages from malicious senders directly in the email gateway.

**Sender Domain:**  
Block entire domains if they’re exclusively used for phishing.

**Sending IP Address:**  
Used only in high-confidence cases (e.g., known malware-sending IP).

**Subject Line:**  
Block recurring phishing emails using common subject strings.

---

## Web Proxy and Firewall Blocks

### URL Blocks

- Very specific; blocks only the exact path.
- Good for static phishing links or personalized URLs.
- Can target key directories (e.g., `/index/2024/malware/...`) to widen scope.

### Domain Blocks

- Blocks all URLs from a domain.
- Best for domains created solely for malicious purposes.
- Use with caution—verify domain legitimacy with WHOIS, Google, or screenshot tools.

### DNS Blackholing

- Redirects users attempting to access a blocked domain to a safe page.
- Can be used for user education and SIEM alerting.

### Firewall Blocks

- Used to block malicious IPs hosting multiple phishing sites.
- Rare for phishing use—more common with scanning or infrastructure attacks.
- IPs are easily rotated by attackers, so not reliable long-term.

---

## Making the Decision to Block

Ask:

- Was the domain created for malicious purposes?
- Is there any legitimate business need to access this domain?
- Would a URL block be more effective than a domain block?

**Use:**
- **WHOIS**: to verify domain age and registrar
- **URL2PNG or Hybrid Analysis**: to assess visual content
- **VirusTotal / threat feeds**: to determine history or prior detections

---

> The faster and more thoroughly you respond, the lower the risk to the organization. Reactive measures are where analysis meets action.

