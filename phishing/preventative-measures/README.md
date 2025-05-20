# Preventative Measures Against Phishing

This section covers key technical and human-focused defenses that help reduce the likelihood of phishing emails reaching users or causing harm. Topics include:

- Anti-spoofing records (SPF, DKIM, DMARC)
- Spam filtering
- Attachment filtering and sandboxing
- User awareness training and phishing simulations

---

## 1. Anti-Spoofing Records

Email spoofing is a tactic where attackers forge the sender address to impersonate a trusted source. To mitigate this, organizations can implement the following DNS-based records:

### SPF (Sender Policy Framework)

- A DNS TXT record that defines which IPs/domains can send email on behalf of your domain.
- Syntax:
  - v=spf1 include:mailgun.org protection.outlook.com -all
  - The `-all` means emails not matching the rule will fail the SPF check (hard fail).

### DKIM (DomainKeys Identified Mail)

- Adds a cryptographic signature to each email, using a private key.
- The receiving mail server verifies the email by using the public key in your DNS records.
- Ensures the message wasn’t altered in transit.
- Syntax:
  - v=DKIM1; k=rsa; p=<public-key>

### DMARC (Domain-based Message Authentication, Reporting & Conformance)

- Builds on SPF and DKIM, specifying what to do if checks fail.
- Syntax:
  - v=DMARC1; p=quarantine; rua=mailto:contact@yourdomain.com
  - Options for `p=`: `none`, `quarantine`, or `reject`
  - Helps detect spoofing and enables reporting on suspicious senders.

---

## 2. Spam Filtering

Spam filters help prevent unwanted or dangerous emails from reaching user inboxes. They use algorithms, rule sets, blacklists, heuristics, and machine learning to classify messages.

### Types of Spam Filters

- **Gateway Filters:** On-premise appliances (e.g., Barracuda)
- **Hosted Filters:** Cloud-based (e.g., SpamTitan)
- **Desktop Filters:** Installed locally (not ideal for enterprise use)

### Detection Methods

- **Content Filters:** Analyze headers and body content
- **Rule-Based Filters:** Enforce custom rules (e.g., "FREE OFFER" from outside senders)
- **Bayesian Filters:** Use machine learning to adapt to user-defined spam

**Important:** Misconfigured filters can flag legitimate emails. Filters must be tuned, and users should be trained to properly report spam.

---

## 3. Attachment Filtering

Malicious attachments are a common delivery method for malware. Attachment filtering helps block or isolate suspicious file types.

### Common Blocked File Types

- `.exe`, `.vbs`, `.js`, `.iso`, `.bat`, `.ps1`, `.htm`, `.html`

### Common Allowed (But Risky) File Types

- `.zip`, `.docx`, `.xlsm`, `.pdf`

### Actions Filters Can Take

- Block or quarantine the email
- Strip the attachment
- Scan the file before delivery
- Alert administrators
- Generate logs for SIEM ingestion

**Tip:** Block rarely-used file types and monitor macros and embedded scripts in Office documents.

---

## 4. Attachment Sandboxing

Attachment sandboxing detonates files in a controlled virtual environment to observe behavior. If the file behaves maliciously (e.g., connects to a C2 server, modifies registry keys), it can be flagged or blocked.

### Capabilities of Advanced Sandboxing Tools

- Behavioral analysis reports
- Scalable virtual environments
- Machine learning that improves over time
- Integration with threat intelligence platforms

**Use Case:** A macro-enabled Word doc bypasses filters. Sandboxing catches its C2 communication during detonation and blocks future delivery.

---

## 5. User Awareness Training

Humans are the final layer of defense. Social engineering targets human error, so training users to identify phishing is critical.

### Awareness Training Topics

- Unknown senders
- Spelling/grammar errors
- Urgency or emotional manipulation
- Suspicious links or attachments

**Best Practice:** Conduct training during onboarding and refresh regularly.

---

## 6. Simulated Phishing Attacks

Simulations test your users and identify training gaps. These mock phishing emails assess how employees respond under real-world conditions.

### Benefits

- Measure who clicks vs. who reports
- Reinforce training with real examples
- Identify repeat offenders for retraining

### Common Platforms

- Sophos Phish Threat
- GoPhish (open source)
- Trend Micro Phish Insight
- PhishingBox

---

## Conclusion

Phishing protection is multi-layered:

- Technical: DNS records, spam filters, sandboxing
- Organizational: Training, simulations, policy enforcement

No single tool will stop all attacks. Prevention is strongest when people, processes, and technology work together.



