# 1.5: Management & Compliance

This section covers the administrative and governance frameworks that underpin a mature security program.

---

## AAA Framework

The **AAA (Authentication, Authorization, Accountability)** framework is a foundational security model used to control access and enforce policies.

-   **Authentication**: Verifies the identity of a user. *Who are you?*
    -   **Factors**: Something you know (password), something you have (token), something you are (biometrics).
-   **Authorization**: Determines what an authenticated user is allowed to do. *What are you allowed to do?*
    -   **Concepts**: Principle of Least Privilege (PoLP), Role-Based Access Control (RBAC).
-   **Accountability**: Tracks user actions and links them to their identity. *What did you do?*
    -   **Tools**: Audit trails, logs, SIEM integration.

---

## Risk Management

**Risk** is the potential for loss or damage when a threat exploits a vulnerability. Risk management is the process of identifying, assessing, and treating this potential.

> **Risk = Threat × Vulnerability × Impact**

### Risk Treatment Options
-   **Mitigation**: Reduce risk by applying controls (e.g., patching, firewalls).
-   **Transfer**: Shift responsibility to a third party (e.g., cyber insurance).
-   **Acceptance**: Acknowledge and tolerate the risk (when cost of mitigation is too high).
-   **Avoidance**: Eliminate the activity that causes the risk.

---

## Policies and Procedures

Policies and procedures form the backbone of a secure organization, defining expectations and guiding employee behavior.

-   **Policy**: A high-level statement of intent. (*What* we do).
-   **Standard**: A mandatory rule that supports a policy. (*How* we do it consistently).
-   **Procedure**: A step-by-step instruction for a specific task. (*How* to perform the task).
-   **Guideline**: A recommended, non-mandatory practice. (*What* we suggest).

### Common Policies
-   **Acceptable Use Policy (AUP)**: Defines rules for using company resources.
-   **Service Level Agreement (SLA)**: Defines service expectations between a provider and client.
-   **Bring Your Own Device (BYOD) Policy**: Governs the use of personal devices on the corporate network.

---

## Compliance and Frameworks

**Compliance** ensures that an organization meets legal, regulatory, and industry-specific requirements for security and data protection. **Frameworks** provide the structure for achieving compliance.

| Framework/Regulation | Focus                                             | Key Requirement Examples                                      |
|----------------------|---------------------------------------------------|---------------------------------------------------------------|
| **GDPR**             | Data protection for EU citizens.                  | Lawful basis for processing, 72-hour breach notification.     |
| **ISO/IEC 27001**      | Information Security Management Systems (ISMS).   | Risk management, access control, continual improvement.       |
| **PCI DSS**          | Security for credit/debit card transactions.      | Protect cardholder data, maintain secure networks, regular testing. |
| **HIPAA**            | Protection of electronic health information (ePHI) in the US. | Technical safeguards (encryption), administrative safeguards (training). |

[⬆️ Back to Security Fundamentals](./README.md)