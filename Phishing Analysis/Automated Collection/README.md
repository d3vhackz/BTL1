# Automated Collection with PhishTool

PhishTool is a forensic analysis platform that automates artifact extraction from phishing emails. It provides a web-based console where analysts can upload emails, extract indicators, and generate detailed reports—removing the need for manual collection in many cases.

---

## Uploading an Email

1. Navigate to the PhishTool console.
2. Upload a phishing email by either:
   - Drag-and-dropping the email file
   - Clicking "Browse" and selecting the `.eml` or `.msg` file

Once uploaded, PhishTool automatically parses the message and extracts relevant metadata and artifacts.

---

## Example 1: Email with Malicious Links

After submission, PhishTool will display several sections:

### Basic Header
Retrieve the following artifacts:
- Sending Address
- Subject Line
- Recipients
- Date and Time

### Detailed Header
Provides:
- Sending Server IP (e.g., X-Originating-IP)
- Reverse DNS of the sending server

### URLs Section
- Lists all hyperlinks contained in the email body
- URLs can be copied directly using the clipboard icon

**Note:** File-related artifacts are not applicable in this example.

---

## Example 2: Email with Suspicious Attachment

After uploading an email with an attachment:

### Basic Header → Attachments Section
Artifacts extracted include:
- File Name
- MD5 File Hash
- VirusTotal link for hash reputation checking

PhishTool automatically links to VirusTotal, allowing analysts to view the file's threat reputation in seconds.

---

## Summary

PhishTool can extract the following artifacts automatically:

- Sending Address
- Subject Line
- Recipients
- Date and Time
- Sending Server IP
- Reverse DNS
- URLs (if present)
- File Name (if attached)
- File Hash (if attached)

---

## Why This Matters

Automated collection via PhishTool drastically reduces time-to-analysis and minimizes manual errors. However, analysts should still be proficient in manual methods for cases where automation isn't available (e.g., no internet access, tool outages).

> Knowing how to use tools is powerful. Knowing how to work without them is critical.

