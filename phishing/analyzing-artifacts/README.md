# Analyzing Artifacts

This section covers how to safely analyze phishing email artifacts using free online tools for visualization, reputation checks, and threat intelligence enrichment. These methods allow analysts to gather context about URLs and attachments without executing them.

---

## Visualization Tools

### URL2PNG

- Generates a screenshot of a website without visiting it.
- Input the URL into the form and the tool will return an image of the webpage.
- Useful for viewing malicious login portals (e.g., spoofed Outlook login pages) safely.

### URLScan.io

- Provides screenshots and metadata about a scanned URL.
- Includes:
  - Destination page preview
  - Hosting server IPs
  - Technologies used on the page
  - Redirect chains and embedded domains
- Screenshot is shown alongside threat intelligence data.

**Why it matters:**  
Visualization tools help analysts preview suspicious links without the risk of exposure or infection.

---

## URL Reputation Tools

### VirusTotal

- Allows you to submit URLs and check if any security vendors have flagged them as malicious.
- Community-based scoring provides additional context.
- A red result from multiple vendors indicates strong evidence of malicious behavior.

**Note:** A clean result does not guarantee safety. Assume a suspicious URL is malicious unless proven otherwise.

### URLScan.io

- Besides screenshots, URLScan includes:
  - Threat reputation
  - Network infrastructure
  - Related scans
- Valuable for correlating URL data across scans and identifying infrastructure reuse.

### Threat Feeds

#### URLhaus
- Community-powered feed of known malicious URLs.
- Displays:
  - Date added
  - URL
  - Malware type (e.g., Quakbot)
  - Source who reported it
- Feeds can be used to power blacklists in email security gateways.

#### PhishTank
- Similar to URLhaus but focused on phishing.
- Submissions are validated by the community.
- Provides transparency and history of phishing URLs.

---

## File Reputation Tools

### VirusTotal (File Tab)

- Upload suspicious files directly to see how many vendors detect them.
- Supports .exe, .doc, .pdf, and other file types.
- Displays:
  - Detection ratio (e.g., 63/72 flagged malicious)
  - File size and type
  - Vendor-specific detection names

**Reminder:** A file with no detections can still be malicious. Further analysis is always required.

### Talos File Reputation (Cisco)

- Enter a SHA256 file hash to check if Cisco’s products flag the file as malicious.
- Supported by:
  - AMP
  - FirePower
  - ClamAV
  - Snort
- Results include:
  - Detection score (e.g., 100)
  - File metadata
  - Detection aliases

### Getting SHA256 Hashes

**PowerShell (Windows):**

```powershell
Get-FileHash C:\path\to\file -Algorithm SHA256
```

**Linux:**

```bash
sha256sum file
```

### Conclusion

- Visualization, URL reputation, and file reputation tools are foundational to phishing artifact analysis. These tools allow analysts to verify and report on indicators without executing potentially harmful payloads.
- Use these tools as part of a broader investigation process, and always document:
  - The tools used
  - The artifacts analyzed
  - The results (screenshots, scores, verdicts)
