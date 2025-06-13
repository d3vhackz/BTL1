# 3.1: Introduction to Threat Intelligence

**Cyber Threat Intelligence (CTI)** is evidence-based knowledge about an existing or emerging threat that can be used to inform decisions. It transforms raw data into actionable insights that help organizations anticipate, prevent, and respond to cyberattacks.

---

## The Intelligence Lifecycle

CTI is not just data; it's the output of a structured, six-phase process.

1.  **Planning & Direction**: Define the goals of the intelligence effort. What questions do we need to answer?
2.  **Collection**: Gather raw data from various sources (e.g., threat feeds, OSINT, dark web).
3.  **Processing**: Convert collected data into a usable format (e.g., translating, structuring).
4.  **Analysis**: Turn processed data into actionable intelligence. This is the human-driven phase of connecting the dots.
5.  **Dissemination**: Share the finished intelligence with stakeholders (e.g., SOC teams, leadership).
6.  **Feedback**: Gather input on the intelligence to refine future efforts.

---

## Types of Intelligence (The "-INTS")

CTI often draws from traditional intelligence disciplines.

| Type    | Name                       | Description                                                     | Cyber Example                               |
|---------|----------------------------|-----------------------------------------------------------------|---------------------------------------------|
| **OSINT** | Open-Source Intelligence   | Information gathered from publicly available sources.           | Social media, WHOIS records, public forums. |
| **HUMINT**| Human Intelligence         | Information gathered from human sources.                        | Information from an insider or informant.   |
| **SIGINT**| Signals Intelligence       | Information gathered by intercepting signals.                   | Intercepted C2 traffic (network telemetry). |
| **GEOINT**| Geospatial Intelligence    | Information derived from satellite or aerial imagery.           | N/A in most corporate CTI.                  |

---

## Levels of Threat Intelligence

Threat intelligence is tailored to different audiences and purposes.

-   **Strategic Intelligence**: High-level, non-technical information for leadership. Focuses on risks, trends, and geopolitical factors. *Answers: "What are the threats to our business?"*
-   **Operational Intelligence**: Information about adversary TTPs (Tactics, Techniques, and Procedures). Used by security managers and response teams. *Answers: "How do our adversaries attack?"*
-   **Tactical Intelligence**: Technical indicators like IPs, domains, and file hashes. Used by SOC analysts for real-time detection and blocking. *Answers: "What are the signs of an active attack?"*

---

## Vulnerability Intelligence

A key aspect of CTI is understanding which vulnerabilities pose a real-world threat.

-   **CVE (Common Vulnerabilities and Exposures)**: A unique identifier for a specific vulnerability (e.g., `CVE-2021-44228`).
-   **CVSS (Common Vulnerability Scoring System)**: A static score (0-10) indicating the theoretical severity of a vulnerability.
-   **Predictive Prioritization (e.g., Tenable's VPR)**: A modern approach that combines CVSS scores with real-time threat intelligence to identify which vulnerabilities are *actually being exploited* in the wild, allowing teams to prioritize patching effectively.

[⬆️ Back to Threat Intelligence](./README.md)