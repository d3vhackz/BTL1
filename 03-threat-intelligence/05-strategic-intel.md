# 3.5: Strategic Intelligence

Strategic intelligence provides high-level context about the threat landscape to inform long-term planning, investment, and risk management. It is consumed by leadership and decision-makers.

---

## Intelligence Sharing and Partnerships

No organization can see the entire threat landscape alone. Sharing intelligence builds collective defense.

-   **ISACs (Information Sharing and Analysis Centers)**: These are industry-specific communities (e.g., Financial Services ISAC, Health ISAC) where member organizations can securely share threat data, best practices, and strategic insights.
-   **Benefits**: An attack on one member provides early warning and actionable intelligence for all other members.

---

## Intelligence Sharing Protocols

To ensure that sensitive intelligence is shared responsibly, standardized protocols are used to define who can see the information and what they can do with it.

### Traffic Light Protocol (TLP)
TLP is a system for classifying information based on how widely it can be shared.

| Level               | Color   | Sharing Guideline                                               |
|---------------------|---------|-----------------------------------------------------------------|
| **TLP:CLEAR**       | White   | Publicly shareable. No restrictions.                            |
| **TLP:GREEN**       | Green   | Shareable within a specific community (e.g., an ISAC).          |
| **TLP:AMBER**       | Amber   | Limited sharing; recipients can only share on a need-to-know basis within their own organization. |
| **TLP:AMBER+STRICT**| Amber+  | Stricter than AMBER; limited to the organization only.          |
| **TLP:RED**         | Red     | No sharing. Information is for the recipient's eyes only.       |

### Permissible Action Protocol (PAP)
PAP defines what actions are permitted when handling threat data, based on the risk of alerting the adversary.

| Level           | Color   | Permissible Action                                              |
|-----------------|---------|-----------------------------------------------------------------|
| **PAP:CLEAR**   | White   | No restrictions on action.                                      |
| **PAP:GREEN**   | Green   | Defensive actions are permitted (e.g., blocking an IP).         |
| **PAP:AMBER**   | Amber   | Passive observation only. No actions that could tip off the adversary. |
| **PAP:RED**     | Red     | Extreme caution; passive observation in highly controlled, non-attributable environments. |

> **Relationship**: TLP defines **who** can know. PAP defines **what** they can do with that knowledge.

[⬆️ Back to Threat Intelligence](./README.md)