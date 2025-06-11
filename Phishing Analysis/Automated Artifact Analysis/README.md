# Automated Artifact Analysis with PhishTool

PhishTool isn’t just for collecting artifacts—it can also be used to perform **direct analysis** on the artifacts extracted from phishing emails. This streamlines the investigation process and consolidates key tools in one console.

This lesson covers how to:
- Analyze file hashes with VirusTotal
- Visualize URLs without visiting them
- Perform WHOIS lookups on suspicious domains

---

## File Artifact Analysis

When a phishing email contains an attachment, PhishTool automatically extracts:
- **Filename**
- **MD5 hash**

Next to the hash, there is a button labeled **"VirusTotal"**. Clicking this will:
- Generate a VirusTotal search for that MD5 hash
- Open it in a new browser tab
- Show you which vendors (if any) flagged the file as malicious

**Why this matters:**
- MD5 hashes can be used to quickly determine if a file is known malware
- VirusTotal provides visibility across dozens of AV engines
- Additional details like detection names, first submission date, and behavior analysis are available in the VirusTotal report

**Note:** MD5 is sufficient for most reputation checks, but SHA256 is the preferred standard for modern systems.

---

## Web Artifact Analysis

If an email contains URLs, PhishTool allows analysts to safely analyze them using:

### Web Capture

- Clicking on any extracted URL shows a **live screenshot** of the destination
- Works like URL2PNG or URLScan, but directly in the platform
- Includes:
  - Screenshot of the webpage
  - HTTP request/response headers
  - Redirect history (if applicable)

**Use Case:**  
This allows you to inspect potentially malicious login pages or phishing portals without the risk of navigating to the site yourself.

---

## WHOIS Domain Analysis

PhishTool includes built-in **WHOIS lookups** for any domain found in a phishing email. This lookup provides:

- **Registrar** (e.g., GoDaddy, Namecheap, Domain.com LLC)
- **Domain age** (e.g., 2643 days old)
- **Registrant contact email**
- **Creation, update, and expiration dates**
- **Name servers and DNS records**

**Why this matters:**
- Legitimate domains are often older, while malicious infrastructure tends to be newly registered
- WHOIS emails can be used for pivoting or abuse reporting
- Domain registrars give insight into the attacker’s ecosystem

---

## Additional Benefits of PhishTool Analysis

- Centralized investigation: no need to switch between tools
- Faster reporting: artifact metadata and reputation scores in one pane
- Reduced risk: no direct interaction with malicious sites or files
- Supports team-based workflows: artifacts and tags can be annotated and shared

---

## Summary

PhishTool enables automated analysis of phishing artifacts, including:

- **MD5 hash lookups on VirusTotal** for attachment reputation
- **Safe web visualization** of suspicious URLs via web capture
- **WHOIS lookups** for attacker-owned domains

> While manual analysis is essential, automation speeds up triage and makes your workflow scalable. Use tools like PhishTool to handle the heavy lifting—so you can focus on decision-making and response.

