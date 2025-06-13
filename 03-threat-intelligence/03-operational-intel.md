# 3.3: Operational Intelligence

Operational intelligence focuses on adversary TTPs to help defenders understand *how* an attack unfolds. It bridges high-level strategy and low-level technical indicators.

---

## Precursors vs. Indicators of Compromise (IOCs)

-   **Precursors**: Early signs that an attack may be imminent. They are often subtle and hard to detect.
    -   *Examples*: A spike in port scanning against a specific server, social engineering attempts against employees, dark web chatter mentioning your organization.
-   **Indicators of Compromise (IOCs)**: Forensic artifacts that provide evidence that a compromise has already occurred. They are concrete and actionable.
    -   *Examples*: Malicious IP addresses, domains, file hashes, or suspicious registry keys.

---

## The Pyramid of Pain

The Pyramid of Pain is a conceptual model that illustrates how difficult it is for an adversary to change different types of indicators when they are detected. The higher up the pyramid a defender focuses their efforts, the more "pain" they cause the adversary.

| Level                  | Pain to Adversary | Description                                                |
|------------------------|-------------------|------------------------------------------------------------|
| **TTPs**               | 😭 Maximum        | Forcing an adversary to change their core behavior is extremely difficult and costly. |
| **Tools**              | 😫 Very High     | Blocking an entire tool (e.g., Cobalt Strike) requires the adversary to retool. |
| **Network/Host Artifacts**| 😖 High          | Detecting unique strings or patterns from malware traffic/behavior. |
| **Domain Names**       | 😒 Moderate       | Adversaries must acquire and set up new domains.           |
| **IP Addresses**       | 😑 Low           | Easily changed; often sourced from compromised infrastructure or cloud providers. |
| **Hash Values**        | 😄 Trivial        | A single-bit change to a file creates a new hash.          |

> **Defensive Strategy**: Focus on detecting and blocking at the top of the pyramid (Tools and TTPs) to have the greatest impact on adversary operations.

---

## Attribution and Its Challenges

**Attribution** is the process of identifying the source of an attack—the specific machine, individual, or sponsoring organization.

### Levels of Attribution
-   **Machine Attribution**: Identifying the device used (e.g., IP, hostname).
-   **Human Attribution**: Linking the activity to a specific person.
-   **Ultimate Attribution**: Identifying the nation-state or organization responsible.

### Challenges
-   **Anonymization**: Attackers use proxies, VPNs, and TOR to hide their true origin.
-   **Spoofing**: IPs and other data can be forged.
-   **False Flags**: Sophisticated actors may intentionally plant evidence to misdirect attribution and blame another group.

> **Conclusion**: While tempting, definitive attribution is often difficult and time-consuming. For most corporate defenders, the priority should be on detection, response, and remediation rather than attribution.

[⬆️ Back to Threat Intelligence](./README.md)