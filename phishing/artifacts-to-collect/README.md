# Artifacts to Collect

When investigating a phishing email, collecting the right artifacts is essential for threat hunting, intelligence sharing, and taking defensive actions. Below are the key categories and artifacts to retain.

---

## 📧 Email Artifacts

### Sending Email Address
- The address that sent or appears to have sent the email
- May be spoofed; still useful for searching mail logs or security tools

### Subject Line
- Useful for identifying other emails in the same campaign
- Can be used to search, detect, or block related phishing attempts

### Recipient Email Addresses
- Identify who else received the phishing email
- BCC is often used to hide recipients
- Cross-reference sender + subject in email gateway to find affected mailboxes

### Sending Server IP & Reverse DNS
- Helps validate legitimacy of sending server
- Use reverse DNS tools (e.g., MXToolbox) to identify hostname

### Reply-To Address
- May differ from sending address
- Often reveals attacker-controlled email for replies

### Date & Time
- Record timestamp for campaign correlation and pattern analysis
- Helps identify other emails sent around the same time

---

## 📎 File Artifacts

### Attachment Name
- Include full filename and extension
- Useful for blocking by name in EDR or email security tools

### SHA256 Hash Value
- Cryptographic fingerprint of the attachment
- Use for reputation checks (e.g., VirusTotal, Talos)
- SHA256 preferred over MD5 or SHA1 due to collision risks

---

## 🌐 Web Artifacts

### Full URLs
- Copy exactly using "Copy Link" or from raw source
- Used to analyze phishing pages, redirect chains, and payload delivery

### Root Domain
- Helps identify malicious infrastructure
- Useful when full URL is unavailable or for domain-based threat intelligence

---

> These artifacts are foundational for phishing investigations and will often be required on the BTL1 exam. Learn to extract and apply them with precision.

