# Tactical Threat Intelligence

This section covers the actionable, hands-on processes for threat detection and response. It includes threat exposure techniques, IOC monitoring, public footprint analysis, and operational deployment of Threat Intelligence Platforms.

---

## Threat Exposure Checks Explained

Threat exposure checks involve searching for IOCs from vendors, gov alerts, or OSINT in internal tools like SIEM and EDR.

### Example Workflow:
- US-CERT releases alert with scanning IPs
- Analyst queries firewall logs in the SIEM
- If scanning is detected: block IPs and set alerts
- Cross-team with Vulnerability Management to prioritize patching

> Even medium CVEs may demand urgent patching if actively exploited.

---

## Watchlists / IOC Monitoring

Watchlists let SOCs continuously monitor for IOCs without human effort, alerting teams when activity is detected.

### Example: Malicious IP Watchlist
- Analyst loads IPs into SIEM alert rules
- Alert fires when a phishing email is clicked
- Analyst investigates via EDR logs or network activity

> Automation lets TI analysts scale and focus on deeper tasks.

---

## Public Exposure Checks

TI analysts assess what public data could endanger the org. This includes social media, leaks, and metadata.

### Risk Areas:
- **Social Media Metadata**: Device names, timestamps, GPS from selfies
- **Leaked Screens/Notes**: Visible whiteboards, sticky notes, OS info
- **Insider Threat Signs**: Hostile tweets, erratic behavior
- **Brand Impersonation**: Lookalike domains and fake accounts
- **Data Breach Dumps**: Reused credentials from external hacks

> Monitor these channels proactively to catch weak points early.

---

## Threat Intelligence Platforms (TIPs)

TIPs aggregate threat data and automate distribution to security controls like firewalls or SIEM.

### Functions of TIPs:
- Ingest threat feeds (STIX/TAXII, JSON, CSV)
- Normalize and enrich IOCs
- Share across internal teams or ISACs
- Integrate with SOC tooling

### Example TIPs:
- **MISP** (Open source)
- **ThreatConnect**
- **ThreatQ**
- **Anomali**

> TIPs bridge intelligence and detection. Choose one that fits your org's size and maturity.

---

## Malware Information Sharing Platform (MISP)

MISP is an open-source platform for storing, analyzing, and sharing malware intel and IOCs.

### Key Features:
- Import/export in many formats (STIX, CSV, JSON)
- NIDS/EDR rule generation
- Correlation engine for related IOCs
- API + GUI interfaces
- Attribute-level access control and sharing groups

> Used by thousands of orgs and ISACs. Ideal for hands-on TI teams.

---

## Deploying MISP (Lab Setup)

### Summary of Deployment Process:
1. Import MISP OVA into VirtualBox
2. Set bridged networking for remote access
3. Configure base URL to match IP
4. Start backend workers (`start.sh`)
5. Add users/orgs, set up dashboard
6. Enable threat feeds and pull events
7. Create a custom event using public IOC report (e.g. APT28 from NCSC)

### Why Practice with MISP?
- Reinforces understanding of TIP workflow
- Provides a real-world, open platform to test IOC correlation
- Builds hands-on skills that stand out on resumes and in interviews

> MISP is a great gateway into operationalizing tactical threat intelligence.

---

