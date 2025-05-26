# Report Writing

This section provides a structured approach for writing phishing investigation reports, focusing on clarity, completeness, and actionability. A well-written report allows other analysts and decision-makers to quickly understand the threat and the response taken.

---

## Section 1: Email Header, Artifacts, and Body Content

### Artifacts to Include

**Header Artifacts:**
- Sending Email Address (e.g., J0hnSm1th@gmail.com)
- Reply-to Address (if present)
- Date Sent (e.g., 20th October 2019, 9:34 AM)
- Sending Server IP (e.g., 40.92.10.10)
- Reverse DNS (e.g., mail-oln040092010100.outbound.protection.outlook.com)
- Recipients
- Subject Line

**URLs:**
- Include sanitized versions only (e.g., hxxps://maliciousdomain[.]com)

**Attachments:**
- Filename + Extension
- MD5 / SHA256 Hashes

### Email Description

Attach the original `.eml` or `.msg` file when possible. Include:
- 1–2 sentence summary of email content
- Screenshot of the email (if your system supports it)
- What the email attempts to make the recipient do (click, reply, download, etc.)

---

### Example One

**Artifacts Retrieved:**
- Sender: bobtom112233@gmail.com  
- Reply-to: None  
- Date: Monday 16th September 2019 17:33  
- IP: 209.85.167.42  
- Reverse DNS: mail-lf1-f42.google.com  
- Recipients: contact@dicksonunited.co.uk  
- Subject: Hello  
- URL: None  
- Attachments: None  

**Email Description:**  
This email contains no malicious artifacts but attempts to engage the user in a conversation. Likely reconnaissance or testing mailbox availability.

---

### Example Two

**Artifacts Retrieved:**
- Sender: amazonsupp0rt@outlook.com  
- Reply-to: no-reply@amazon.co.uk  
- Date: Monday 16th September 2019 19:25  
- IP: 209.85.167.91  
- Reverse DNS: mail-lf1-f91.google.com  
- Recipients: claire.shelley@dicksonunited.co.uk  
- Subject: Suspicious Amazon Order Alert  
- URL: hxxp://maliciousdomainexample[.]com/  
- Attachments: None  

**Email Description:**  
This phishing email impersonates Amazon, using urgency to trick the user into clicking a malicious login link. Clearly crafted to harvest credentials.

---

## Section 2: Analysis Process, Tools, and Results

Use this section to show how you analyzed each artifact. Include:
- Tools used (e.g., VirusTotal, Hybrid Analysis)
- What you discovered
- Justification for further action

### Example One – URL Analysis

**URL:** hxxps://maliciousdomainexample[.]com/index/2019/amazon/login.aspx

- **WHOIS** → Registered 3 days ago with no owner info.
- **VirusTotal** → No detections yet; domain likely too new.
- **URL2PNG** → Hosted a fake Amazon login page. No legitimate homepage found.

### Example Two – Attachment Analysis

**Filename:** wallpaperHD.exe  
**MD5:** 0c4374d72e166f15acdfe44e9398d026  
**SHA256:** 240387329dee4f03f98a89a2feff9bf30dcba61fcf614cdac24129da54442762  

- **VirusTotal** → 61/71 vendors flag it as malicious.  
- **Talos File Reputation** → Confirms VT result.

**Conclusion:** The file is confirmed malware. Detonation unnecessary; file is widely known.

---

## Section 3: Defensive Measures Taken

Summarize what you’ve done—or requested—to block this threat.

### Types of Defensive Actions:
- **Email:** Block sender, domain, subject, or server IP
- **Web:** Block URLs/domains via proxy
- **File:** Block hashes via EDR or AV solutions

### Example One – Analyst Takes Action

- Subject Line Block → “Failed Delivery DHL RESPOND NOW – URGENT!!”
- Domain Block → hxxps://shanepppalkkbc[.]com  
- Rationale: High-confidence spoofing and no business justification

### Example Two – Analyst Escalates

- Sending Address Block → HMRC-0fficial@govpayments.net  
- Domain Block → hmrc.announcementsgov[.]com  
- Rationale: Emotet malware detected; URL and attachment serve the same payload

---

## Section 4: Artifact Sanitization

**Why Defang Artifacts?**  
Avoid accidental execution of malware by colleagues or systems that auto-parse links.

**Defang Rules:**
- Replace `.` with `[.]` in URLs/IPs
- Change `http` to `hxxp`

**Examples:**
- `8.8.8.8` → `8[.]8[.]8[.]8`  
- `https://malware.site` → `hxxps://malware[.]site`

**Tip:** Use [CyberChef](https://gchq.github.io/CyberChef) → "Defang URL" and "Defang IP" operations for bulk sanitization.

---

> A strong report is actionable, traceable, and clear. It helps others understand what happened, what was done, and what needs to be done next.

