# Strategic Threat Intelligence

This section outlines high-level approaches to acquiring, distributing, and managing intelligence that informs long-term cybersecurity planning and response.

---

## Intelligence Sharing and Partnerships

Organizations benefit from joining **Information Sharing and Analysis Centers (ISACs)** to exchange IOCs, precursors, and strategic threat info.

### Example:
- Org A and Org B (same industry) form an ISAC
- Org B shares threat data post-attack
- Org A applies defensive measures based on that intel

> Strong partnerships increase resilience across industry verticals.

---

## IOC/TTP Gathering and Distribution

Strategic TI analysts gather and forward relevant IOCs and TTPs to tactical analysts for exposure checks and detection tuning.

### Workflow Example:
- Org B shares APT-related IPs post-incident
- Org A’s strategic analyst receives the IOCs
- Passes to tactical team for SIEM searches and alerts
- Alerts broader SOC for situational awareness

> Focus on relevance to reduce noise and avoid false positives.

---

## OSINT vs Paid-for Sources

A comparison between free and premium intelligence sources.

### Open-Source Intelligence (OSINT)
- Spamhaus, URLhaus, AlienVault OTX, Anomali Weekly
- SANS ISC, Talos, VirusShare
- CISA AIS (Automated Indicator Sharing)

Pros: Free, abundant, wide coverage  
Cons: Requires validation, often delayed

### Paid Intelligence Vendors
- FireEye, Recorded Future, CrowdStrike, Flashpoint, Intel471

Pros: Curated, threat-specific, rich context  
Cons: Expensive, may be redundant without proper integration

> Ideal strategy: Combine validated OSINT with relevant paid feeds.

---

## Traffic Light Protocol (TLP)

A classification system for defining how threat intel can be shared.

### TLP Levels

- **TLP:CLEAR** – Publicly shareable
- **TLP:GREEN** – Shareable within community groups (e.g., ISACs)
- **TLP:AMBER** – Internal or client-only (need-to-know)
- **TLP:AMBER+STRICT** – Internal org-only
- **TLP:RED** – No sharing beyond recipients

### Example:
Org A hit by APT33 shares TLP:GREEN IOCs within its ISAC  
Red team reports, or APT detection may be classified TLP:RED

> TLP helps ensure sensitive data stays in trusted hands.

---

## Permissible Action Protocol (PAP)

PAP defines **what actions** can be taken when handling threat data, based on risk of exposure to adversaries.

### PAP Levels

- **PAP:CLEAR** – No restrictions (follow legal/licensing norms)
- **PAP:GREEN** – Permits safe mitigation (e.g., firewall blocks)
- **PAP:AMBER** – Passive review only; no adversary engagement
- **PAP:RED** – Threat hunting in isolated, invisible environments

### Example:
- PAP:GREEN → Block known malicious domain
- PAP:AMBER → Lookup threat info without triggering detection
- PAP:RED → Silent monitoring for APTs in air-gapped networks

> PAP complements TLP by defining safe operational behavior.

---

