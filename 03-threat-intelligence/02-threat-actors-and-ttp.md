# 3.2: Threat Actors and TTPs

Understanding who the adversaries are, what motivates them, and how they operate is central to building an effective defense.

---

## Threat Actor Categories

| Category          | Motivation                 | Common Characteristics                                     |
|-------------------|----------------------------|------------------------------------------------------------|
| **Cyber Criminals** | Financial Gain             | From sophisticated syndicates (e.g., ransomware) to script kiddies. |
| **Nation-States (APTs)**| Espionage, Sabotage, Politics| Extremely resourced, persistent, and stealthy.             |
| **Hacktivists**   | Social or Political Causes | Known for DDoS attacks and website defacement.             |
| **Insider Threats** | Varies (malice, accident)  | Employees or contractors who misuse their authorized access. |

---

## Advanced Persistent Threats (APTs)

**APTs** are highly sophisticated, typically state-sponsored groups that conduct long-term, covert cyber operations to achieve a specific objective.

-   **Advanced**: Use custom, sophisticated tools and zero-day exploits.
-   **Persistent**: Remain in a network for extended periods, adapting to defenses.
-   **Threat**: Have a clear intent, motive, and capability.

### Threat Actor Naming Conventions
Security vendors use different naming schemes to track threat actors, often based on perceived origin or characteristics.

| Vendor      | Naming Convention                                | Example                       |
|-------------|--------------------------------------------------|-------------------------------|
| **CrowdStrike**| `[Animal] [Adjective]` based on origin country. | Fancy Bear (Russia), Goblin Panda (China) |
| **Mandiant**  | `APTxx` for nation-states, `FINxx` for financial. | APT28 (Russia), FIN7 (Financial)  |

---

## TTPs & The MITRE ATT&CK Framework

**TTPs** describe **how** adversaries operate: their **T**actics, **T**echniques, and **P**rocedures.

The **MITRE ATT&CK® Framework** is a globally accessible knowledge base of adversary tactics and techniques based on real-world observations. It provides a common vocabulary for discussing and defending against TTPs.

### ATT&CK Tactics (The "Why")
These are the adversary's high-level tactical goals.
1.  Reconnaissance
2.  Resource Development
3.  Initial Access
4.  Execution
5.  Persistence
6.  Privilege Escalation
7.  Defense Evasion
8.  Credential Access
9.  Discovery
10. Lateral Movement
11. Collection
12. Command and Control
13. Exfiltration
14. Impact

### Using ATT&CK
-   **Threat Modeling**: Map an APT's known techniques against your defenses.
-   **Detection Engineering**: Build SIEM rules that detect specific techniques (e.g., T1059.001 - PowerShell).
-   **Red Teaming**: Emulate adversary TTPs to test security controls.

[⬆️ Back to Threat Intelligence](./README.md)