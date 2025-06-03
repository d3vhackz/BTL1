# Operational Threat Intelligence

This section explores tactical and behavioral insights that help defenders detect and respond to cyber threats proactively. Topics include threat precursors, indicators of compromise, attribution challenges, threat modeling frameworks, and attacker pain points.

---

## Precursors Explained

**Precursors** are early signs or behaviors that may indicate a system vulnerability or the groundwork for an attack. These signals, while hard to detect, provide critical insight to preempt malicious activity.

### Types of Precursors

- **Port Scanning & Fingerprinting**: Watch for spikes in port probes or vuln scans.
- **Social Engineering & Recon**: E.g., dumpster diving, eavesdropping, suspicious visitor behavior.
- **OSINT & Dark Web Chatter**: Monitor threat group chatter, CVEs, or declared targeting.

> Most attacks don’t show detectable precursors—making them difficult to act on without mature logging and awareness.

---

## Indicators of Compromise (IOCs) Explained

IOCs are forensic artifacts used to detect and identify compromises in networks and systems.

### Common IOCs

- Email addresses sending phishing
- IP addresses running scans or malware C2
- Domains/URLs hosting phishing or payloads
- File hashes/names of malware

### IOC Sharing Formats

- **STIX**: Rich structure for threat info (incl. capabilities and motivation)
- **TAXII**: Protocol to share STIX data with trusted peers

---

## MITRE ATT&CK Framework

The MITRE ATT&CK framework documents known adversary tactics and techniques based on real-world observations.

### Use Cases

- Map APT behavior
- Build detections
- Simulate threats
- Assess SOC coverage

> MITRE ATT&CK gives shared language to describe attacker behaviors across organizations.

---

## Lockheed Martin Cyber Kill Chain

A 7-step model outlining phases of an APT attack:

1. Reconnaissance  
2. Weaponization  
3. Delivery  
4. Exploitation  
5. Installation  
6. Command & Control  
7. Actions on Objectives  

### Benefits

- Simple overview of attack lifecycle

### Limitations

- External-heavy, doesn’t model internal threats well
- Often combined with ATT&CK for deeper insight (Unified Kill Chain)

---

## Attribution and Its Limitations

Attribution is the process of identifying the origin of an attack: machine, individual, or ultimate sponsor.

### Levels of Attribution

- **Machine Attribution**: IPs, hostnames, logs
- **Human Attribution**: Behavioral ties to specific people (often unreliable alone)
- **Ultimate Attribution**: Nation-states or organizations responsible

### Indicators for Attribution

- Tradecraft and TTPs  
- Attack infrastructure  
- Unique malware signatures  
- Intent and targeting  
- External sources and intelligence reports

### Attribution Challenges

- IPs and data may be spoofed or relayed  
- TOR and proxies enhance attacker anonymity  
- Threat actors may impersonate others to confuse defenders

---

## Pyramid of Pain

A model showing which indicators cause more disruption to attackers when detected or blocked:

| Indicator Type         | Difficulty to Change | Pain to Adversary |
|------------------------|----------------------|--------------------|
| **Hash Values**        | Very Easy            | Minimal            |
| **IP Addresses**       | Easy                 | Low                |
| **Domain Names**       | Moderate             | Moderate           |
| **Network Artifacts**  | Hard                 | High               |
| **Tools**              | Very Hard            | Very High          |
| **TTPs**               | Extremely Hard       | Maximum            |

> Focus your defense on high-impact areas: artifacts, tools, and TTPs to force attacker retooling.

---


