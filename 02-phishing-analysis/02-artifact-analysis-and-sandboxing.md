# 2.2: Artifact Analysis and Sandboxing

Once artifacts are collected, the next step is to analyze them using specialized tools to determine if they are malicious.

---

## Artifact Analysis Tools & Techniques

Analysts must use a variety of tools to gather context about URLs and files without executing them on a production machine.

### URL Analysis
-   **Safe Visualization**: Tools like **URLScan.io** and **URL2PNG** generate a screenshot of a website without requiring you to visit it. This safely reveals spoofed login pages or malicious content.
-   **Reputation Checks**:
    -   **VirusTotal**: Checks a URL against dozens of security vendor blacklists.
    -   **URLhaus / PhishTank**: Community-powered feeds of known malicious URLs.
-   **WHOIS Lookups**: Provides registration details for a domain, such as creation date, registrar, and contact info. Newly registered domains are often a red flag.

### File Analysis
-   **Reputation Checks**:
    -   **VirusTotal (File Tab)**: Upload a file or submit its SHA256 hash to see if it's detected as malware by AV engines.
    -   **Cisco Talos File Reputation**: Checks a file hash against Cisco's threat intelligence ecosystem.
-   **Static vs. Dynamic Analysis**:
    -   **Static Analysis**: Examining a file without running it (e.g., checking strings, headers).
    -   **Dynamic Analysis**: Running the file in a sandbox to observe its behavior.

---

## Malware Sandboxing

**Malware sandboxing** is the technique of executing a potentially malicious file in a secure, isolated environment to observe its behavior. It is the core of dynamic analysis.

### Why Sandboxing is Critical
When malware is detonated in a sandbox, an analyst can safely observe and document:
-   **Process Activity**: What processes the malware spawned (e.g., `powershell.exe`).
-   **Network Connections**: Communication with C2 servers, DNS lookups, or download attempts.
-   **File System & Registry Changes**: Files dropped or registry keys created for persistence.

### Featured Tool: Hybrid Analysis
**Hybrid Analysis** is a free online sandbox that provides detailed reports on file behavior.
-   **Threat Score & Verdict**: An overall risk assessment.
-   **MITRE ATT&CK Mapping**: Maps observed behaviors to specific attacker techniques.
-   **Extracted IOCs**: A clean list of IPs, domains, hashes, and other indicators.

> **Workflow**: If a file has no reputation on VirusTotal (a "zero-day"), a sandbox is the best way to determine if it is malicious. The behavioral report provides the evidence needed for blocking and remediation.

[⬆️ Back to Phishing Analysis](./README.md)