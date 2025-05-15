# Artifacts to Collect

When investigating a phishing email, collecting the right artifacts is essential for further analysis, intelligence sharing, and implementing defensive controls. Below are the key categories and artifacts to retain.

---

## Email Artifacts

### Sending Email Address
- The address that sent or appeared to send the email
- May be spoofed, but still useful for searches and correlation

### Subject Line
- Can be used to identify related phishing emails or block future attempts

### Recipient Email Addresses
- Identify who else received the email
- Check email gateway logs using sender and subject to find other recipients

### Sending Server IP and Reverse DNS
- Helps verify whether the sender domain matches the sending infrastructure
- Use reverse DNS tools to resolve the hostname

### Reply-To Address
- Often differs from the spoofed sending address
- Reveals the attacker's real destination for replies

### Date and Time
- Useful for correlating with other attack attempts or campaign timelines
- Helps identify patterns of phishing activity

---

## File Artifacts

### Attachment Name
- Record full file name and extension
- May be used for blocking in EDR or mail filtering systems

### SHA256 Hash Value
- Unique identifier for the file
- Use for reputation checks (e.g., VirusTotal)
- SHA256 is the standard—avoid MD5 and SHA1 due to known collisions

---

## Web Artifacts

### Full URLs
- Always copy the full link accurately; never retype manually
- Source can be obtained via "Copy Link" or from the email header/source

### Root Domain
- Sometimes useful when the full URL isn't available
- Can help determine if the domain is newly created, suspicious, or compromised

---

> These artifacts are core to phishing investigations. Proper collection and documentation enables search, detection, response, and intelligence correlation.
