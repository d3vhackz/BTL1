# Manual Collection of Phishing Artifacts

This guide outlines how to manually collect email, web, and file-based artifacts during phishing investigations. These artifacts support deeper analysis, threat intelligence, and defensive response.

> Always perform phishing analysis in a virtual machine or isolated system. Avoid opening suspicious emails or files on your corporate or personal machine.

---

## Email Artifact Collection

### Collected Artifacts:
- Sending Address
- Subject Line
- Recipients (unless BCC)
- Date and Time
- Sending Server IP
- Reverse DNS of the sending server
- Reply-To address (if present)

### Method 1: Email Client (e.g., Outlook, Thunderbird)
Most common email artifacts are directly viewable:
- Subject, sender, timestamp, and recipients are in the header
- Hover over or right-click hyperlinks to copy full URLs

**Caution:** Avoid accidental clicks when copying links.

### Method 2: Text Editor (.eml or .msg Files)
For deeper metadata like IPs and reply-to addresses:
1. Open the email file in a text editor (e.g., Sublime Text)
2. Use `Ctrl+F` to search:
   - `"IP"` → Find `X-Sender-IP`
   - `"reply"` → Find `Reply-To` address

### Reverse DNS Lookup
Use services like [DomainTools WHOIS](https://whois.domaintools.com/) to convert the sender IP into a hostname. A mismatch between the sender domain and hostname often indicates spoofing.

---

## Web Artifact Collection

### Collected Artifacts:
- Full URL (copied from the message)
- Root domain (for infrastructure analysis)

### Method 1: Email Client
- Hover over or right-click a hyperlink → Copy the link
- Never retype; copying ensures accuracy

### Method 2: Text Editor
Search the raw email for:
- `"http"` → Identifies hyperlinks
- `"<a>"` → HTML anchor tags
- Text near the link (e.g., “click here”) to locate the tag

Extract the URL between `href="..."` for clean, safe copying.

---

## File Artifact Collection

### Collected Artifacts:
- Filename (with extension)
- SHA256 hash (preferred)
- MD5 and SHA1 hashes (legacy, still used by some tools)

### PowerShell (Windows)
```powershell
Get-FileHash C:\path\to\file -Algorithm SHA256
Get-FileHash C:\path\to\file -Algorithm SHA1
Get-FileHash C:\path\to\file -Algorithm MD5
```


