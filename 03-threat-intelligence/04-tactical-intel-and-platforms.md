# 3.4: Tactical Intelligence and Platforms

Tactical intelligence consists of technical indicators used for real-time detection and response. This section covers how to use these IOCs and the platforms that manage them.

---

## Threat Exposure Checks

A **threat exposure check** is the process of proactively searching internal systems for known IOCs. This is a core task for a tactical CTI analyst.

### Example Workflow
1.  **Receive Intelligence**: A government agency (e.g., CISA) releases an alert with IP addresses and file hashes associated with a new ransomware campaign.
2.  **Query Internal Systems**: The analyst queries the SIEM, firewall logs, and EDR platform for any sign of the reported IOCs.
3.  **Analyze Findings**:
    -   If a match is found, an incident is declared.
    -   If no match is found, the check is documented, and the IOCs are added to a watchlist.
4.  **Action**: Block the identified IOCs in firewalls, proxies, and EDR to prevent future attacks.

---

## IOC Watchlists

Instead of performing manual checks, SOCs use **watchlists** to automate the continuous monitoring of IOCs.

-   **How it works**: A list of malicious IPs, domains, or hashes is loaded into a SIEM or EDR. The system then generates an alert automatically if any activity involving a listed IOC is detected.
-   **Benefit**: Frees up analysts from repetitive searching and enables immediate detection.

---

## Threat Intelligence Platforms (TIPs)

A **TIP** is a centralized system for aggregating, managing, and distributing threat intelligence. It acts as the brain of an organization's CTI program.

### Key Functions
-   **Ingestion**: Collects threat feeds from various sources (OSINT, commercial, government) in formats like STIX/TAXII.
-   **Normalization & Enrichment**: Standardizes data and adds context (e.g., WHOIS info, geolocation).
-   **Distribution**: Pushes actionable IOCs out to security controls (firewalls, SIEM, EDR) for automated blocking and alerting.

### Featured Platform: MISP
The **Malware Information Sharing Platform (MISP)** is a powerful, open-source TIP used by thousands of organizations.

-   **Collaboration**: Enables secure sharing of intelligence within trusted communities.
-   **Correlation**: Automatically links related IOCs and events.
-   **Flexibility**: Supports numerous data formats and has a robust API for integration.

> **Hands-on Practice**: Setting up a personal MISP lab is an excellent way to learn the fundamentals of TIP management and operationalizing threat intelligence.

[⬆️ Back to Threat Intelligence](./README.md)