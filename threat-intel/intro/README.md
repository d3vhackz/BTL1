# Threat Intelligence

## Introduction to Threat Intelligence

Threat intelligence is information that helps organizations understand current or potential threats. This knowledge supports proactive defense, informs risk mitigation strategies, and enables swift response to signs of compromise. Intelligence is typically shared in the form of Indicators of Compromise (IOCs) like malicious email addresses, IPs, or file hashes.

### Threat Intelligence Lifecycle

1. **Planning & Direction** – Set goals and scope.
2. **Collection** – Gather data from forums, OSINT, threat feeds, etc.
3. **Processing** – Translate and structure raw data for analysis.
4. **Analysis** – Turn data into actionable insights tailored to technical or non-technical audiences.
5. **Dissemination** – Share intelligence with SOC teams, TI analysts, or leadership.
6. **Feedback** – Refine efforts based on ongoing team input and evolving threats.

### Threat Intelligence Analysts

TI analysts are critical thinkers and researchers who analyze actors and TTPs (tools, techniques, procedures). Their work supports detection, defense, and strategic planning, often mapping to frameworks like MITRE ATT&CK and the Cyber Kill Chain.

---

## Types of Intelligence

- **SIGINT** – Signals Intelligence, includes COMINT (communication) and ELINT (electronic).
- **OSINT** – Open-source information from public platforms and databases.
- **HUMINT** – Human Intelligence gathered through interviews or espionage.
- **GEOINT** – Geospatial Intelligence from satellite or aerial imaging.

---

## Types of Threat Intelligence

### Strategic
- High-level, non-technical reports for leadership
- Focused on geopolitical risk and industry-specific trends

### Operational
- Research into threat actor TTPs
- Supports red teaming, detection engineering, and incident response

### Tactical
- Technical indicators like hashes, domains, or emails
- Used for detection and automated defenses

---

## Why Threat Intelligence is Useful

- **Cyber Threat Context** – Helps assess relevant adversaries and exposures
- **Incident Prioritization** – Guides decisions when triaging multiple alerts
- **Investigation Enrichment** – Adds critical background to observed behaviors
- **Information Sharing** – Builds collective defense through intelligence communities and threat feeds

---

## The Future of Threat Intelligence

### CVEs and CVSS
- **CVE** – Unique identifier for vulnerabilities (e.g., CVE-2019-0708)
- **CVSS** – Scoring system (e.g., CVSS:3.0/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H)

### Predictive Prioritization
Tenable’s **VPR** (Vulnerability Priority Rating) combines CVEs with real-time threat intelligence to dynamically prioritize vulnerabilities most likely to be exploited. This lets teams focus remediation on real-world threats instead of theoretical risk.

---

## Glossary

### TIP — Threat Intelligence Platform  
A system for storing IOCs and reports to feed into firewalls, IDS, SIEM, and EDR tools for enhanced security posture.

### TEC — Threat Exposure Check  
Manual or automated checks for known malicious indicators across an environment.

### EDR — Endpoint Detection and Response  
Endpoint agents that detect anomalies, send telemetry, and can take automated defensive actions.

### IDS/IPS/IDPS — Intrusion Detection/Prevention System  
Systems that detect (IDS) or prevent (IPS) malicious activity on a network.

### CTI — Cyber Threat Intelligence  
Intelligence activities focused on cyber threats, actors, and defense readiness.

### IOC — Indicator of Compromise  
Data from known malicious activities, e.g., file hashes, email addresses, or domains used in attacks.

### TTP — Tactics, Techniques, and Procedures  
Methods adversaries use, as categorized by MITRE ATT&CK.

### MD5 — Message Digest 5  
A 128-bit hash used for file validation, not secure for cryptographic purposes.

### SHA1 — Secure Hash Algorithm 1  
A deprecated 160-bit hash function formerly used in cryptography.

### SHA256 — Secure Hash Algorithm 256  
A secure hashing algorithm producing a 256-bit hash, useful for verifying file integrity.

### APT — Advanced Persistent Threat  
Nation-state level actors engaged in long-term, covert cyber operations.

### OSINT — Open-Source Intelligence  
Data collected from publicly available sources like websites, social media, and forums.

### MISP — Malware Information Sharing Platform  
An open-source tool to store and share threat intel across trusted organizations.

### DDoS — Distributed Denial-of-Service  
Attacks that overwhelm a service with traffic from many sources to render it unusable.

### CVE — Common Vulnerabilities and Exposures  
A unique identifier system for publicly known security vulnerabilities.

### CVSS — Common Vulnerability Severity Scoring  
Standard for rating the severity of software vulnerabilities.

### RDP — Remote Desktop Protocol  
A Windows service allowing remote GUI access to systems, often abused by attackers for lateral movement.

### VPR — Vulnerability Priority Rating  
Tenable's score combining vulnerability data with threat intel to prioritize patching efforts.

### SIGINT — Signals Intelligence  
Intercepted communication signals for gathering military or cyber intelligence.

### COMINT — Communications Intelligence  
A subset of SIGINT focused on messages between people (text, voice).

### ELINT — Electronic Intelligence  
Intelligence from non-communication sources like radar and guidance systems.

### UAV — Unmanned Aerial Vehicle  
Drones or aircraft operated remotely or autonomously for reconnaissance or other purposes.

### HUMINT — Human Intelligence  
Intelligence gathered through interpersonal contact or observation.

### GEOINT / GEOSINT — Geospatial Intelligence  
Intelligence derived from satellite imagery, used in defense and disaster response.

### FIN — Financially Motivated Threat Actor  
Threat groups driven by financial gain, often involved in cybercrime.

### UNC — Unclassified Threat Actor  
A designation used by FireEye/Mandiant for groups under analysis.

### ISAC — Information Sharing and Analysis Center  
Industry-specific collectives that share cyber threat intelligence for mutual defense.

---

