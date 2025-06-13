# 1.1: Core Security Concepts

This section covers the foundational ideas of physical security, endpoint protection, and the classification of security controls.

---

## Physical Security

**Physical security** comprises the measures designed to protect physical assets, personnel, and infrastructure from unauthorized access, damage, or disruption. Without it, even the most advanced digital defenses can be bypassed.

### Key Control Types

| Control Type          | Purpose                                                     | Examples                                     |
|-----------------------|-------------------------------------------------------------|----------------------------------------------|
| **Deterrent**         | Discourage threats through visibility or consequence.       | Security guards, warning signs, fences.      |
| **Preventive**        | Block or restrict physical access.                          | Locked doors, man-traps, biometric scanners. |
| **Detective**         | Identify and log unauthorized access attempts.              | CCTV, motion detectors, intrusion alarms.    |
| **Corrective/Recovery**| Respond to and restore operations after an incident.      | Emergency exits, backup power, fire suppression. |

---

## Endpoint Security

**Endpoint security** is the practice of securing endpoint devices—such as desktops, laptops, and servers—from malicious threats. Endpoints are the frontline of cyber defense and primary targets for initial access.

### Key Components

| Component | Description                                                                                             |
|-----------|---------------------------------------------------------------------------------------------------------|
| **HIDS/HIPS** | **Host Intrusion Detection/Prevention Systems** monitor and block malicious activity on an individual device using predefined rules and signatures. |
| **Antivirus (AV)** | Detects known malware using **signature-based** matching and flags suspicious behavior using **heuristic** analysis. |
| **EDR/XDR** | **Endpoint/Extended Detection and Response** platforms provide real-time monitoring, historical search, threat hunting, and automated response capabilities. |
| **Vulnerability Scanning** | Identifies system weaknesses. **Credentialed** scans offer deep visibility into configurations and patch levels, while **non-credentialed** scans simulate an external attacker's view. |

---

## Security Controls

**Security controls** are the safeguards and countermeasures implemented to reduce risk and protect assets. They are the practical application of security policy.

### Control Categories by Function

| Category         | Function                                             | Examples                                               |
|------------------|------------------------------------------------------|--------------------------------------------------------|
| **Preventive**   | Stop an incident before it occurs.                   | Firewalls, Access Control Lists (ACLs), Encryption.    |
| **Detective**    | Identify and alert when an incident is in progress.  | Intrusion Detection Systems (IDS), SIEM alerts, Logs.  |
| **Corrective**   | Mitigate damage and restore systems after an incident. | Backups and restore, Patch management, Incident Response Plan. |
| **Deterrent**    | Discourage potential attackers.                      | Warning banners, visible cameras, Acceptable Use Policy (AUP). |
| **Compensating** | Fulfill the intent of a missing or inadequate control. | Increased monitoring to offset a lack of MFA.          |

[⬆️ Back to Security Fundamentals](./README.md)