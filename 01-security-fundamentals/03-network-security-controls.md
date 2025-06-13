# 1.3: Network Security Controls

These controls are designed to monitor and protect network traffic, enforcing access policies and detecting malicious activity.

---

## NIDS vs. NIPS

**Network Intrusion Detection Systems (NIDS)** and **Network Intrusion Prevention Systems (NIPS)** are critical for monitoring and defending network traffic.

| Feature            | NIDS (Detection)                                    | NIPS (Prevention)                                     |
|--------------------|-----------------------------------------------------|-------------------------------------------------------|
| **Deployment**     | **Passive**: Sits off a SPAN port or network tap.   | **Inline**: Sits directly in the path of traffic.     |
| **Function**       | Monitors traffic and generates alerts on discovery. | Actively blocks malicious traffic in real-time.       |
| **Action**         | Alerts security teams for investigation.            | Can drop packets, reset connections, or block IPs.    |
| **Risk of Downtime**| Low. Does not interfere with traffic flow.          | Moderate. A failure or misconfiguration can block legitimate traffic. |
| **Use Case**       | Gaining visibility and detecting threats.           | Proactively stopping known attacks.                   |

> **Summary:** Use NIDS for visibility and NIPS for active prevention. Together, they form a powerful layer of network defense.

---

## Log Monitoring

**Log monitoring** is the practice of collecting, analyzing, and responding to system, network, and security event logs to detect suspicious or malicious activity. It is the digital surveillance of your infrastructure.

### Why It Matters
-   Detects unauthorized access and misconfigurations.
-   Provides audit trails for forensic investigations.
-   Enables compliance with regulatory standards (e.g., PCI-DSS, HIPAA).

### Common Log Sources
-   **Web Proxy Logs**: Track user internet access.
-   **Firewall Logs**: Show allowed/denied traffic and potential scans.
-   **Endpoint Logs**: Record system events, application behavior, and security alerts.
-   **Authentication Logs**: Track logins, failures, and privilege escalations—critical for spotting brute-force attacks.

### Tools and Techniques
-   **Syslog**: A standard protocol for forwarding log messages.
-   **SIEM (Security Information and Event Management)**: A central platform to aggregate, normalize, and correlate logs.
-   **Alerting Rules**: Custom conditions that trigger alerts on suspicious patterns.

---

## Network Access Control (NAC)

**Network Access Control (NAC)** is a security solution that enforces policies on devices attempting to connect to a network, ensuring only trusted and compliant devices gain access. Think of it as a digital bouncer for your network.

### How It Works
1.  **Authentication**: Verifies device or user identity (e.g., via 802.1X, certificates).
2.  **Posture Assessment**: Checks the device for compliance with security policies (e.g., up-to-date AV, OS patches).
3.  **Access Enforcement**: Based on the assessment, NAC can:
    *   **Grant** full network access.
    *   **Deny** access completely.
    *   **Quarantine** the device in a restricted VLAN for remediation.

### Key Use Cases
-   Preventing unauthorized or rogue devices from connecting.
-   Enforcing security posture on corporate and BYOD devices.
-   Segmenting network access based on device type or user role.

[⬆️ Back to Security Fundamentals](./README.md)