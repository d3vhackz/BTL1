# Endpoint Security

Endpoint security protects individual devices (laptops, desktops, servers) from threats that can compromise systems and data. It plays a critical role in preventing breaches and maintaining organizational integrity.

## Key Components

### HIDS (Host Intrusion Detection System)

- Installed on individual endpoints
- Monitors for suspicious or malicious activity using predefined rules
- Generates alerts for investigation
- Can forward logs to a SIEM for correlation and analysis

### HIPS (Host Intrusion Prevention System)

- Builds on HIDS functionality with proactive defense
- Can block traffic, terminate connections, or delete malicious files
- Uses signature and behavior-based rules

### Anti-Virus (AV)

- **Signature-Based**: Detects known malware by matching to a database
- **Behavior-Based**: Flags abnormal activity by comparing against baseline behavior
- Essential to have AV on all endpoints

### Log Monitoring

- Collects logs from endpoints and sends them to a central SIEM
- Enables rule-based correlation and threat detection
- Tools like Syslog help normalize log data for analysis

### EDR (Endpoint Detection and Response)

- Advanced toolset combining HIDS/HIPS, logging, and analytics
- Offers real-time monitoring and historical search capabilities
- Supports threat hunting and incident response
- Useful for detecting insider threats and lateral movement

### Vulnerability Scanning

- **External Scans**: Show what attackers see from outside the network
- **Internal Scans**: Reveal vulnerabilities inside the organization
- **Credentialed**: Provides deep visibility into system configs and patch levels
- **Non-Credentialed**: Simulates attacker’s access with no privileges

---

> Endpoint security is the frontline of cyber defense. It ensures that each device is monitored, protected, and capable of detecting threats independently and as part of a larger detection ecosystem.

