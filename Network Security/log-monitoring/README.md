# Log Monitoring

Log monitoring is the practice of collecting, analyzing, and responding to system, network, and security event logs to detect suspicious or malicious activity.

---

## Why Log Monitoring Matters

- Detects unauthorized access attempts
- Identifies misconfigurations or failed updates
- Provides audit trails for investigations
- Enables compliance with regulatory standards (e.g., PCI-DSS, HIPAA)

---

## Common Log Sources

### Web Proxy Logs
- Track internet access and websites visited by users
- Useful for detecting access to risky or non-work-related content

### Firewall Logs
- Show inbound and outbound traffic rules
- Help detect:
  - Port scans
  - DDoS attacks
  - Unauthorized services

### Endpoint Logs
- Include system events, application behavior, and security alerts
- Can be forwarded to a SIEM for correlation

### Authentication Logs
- Record logins, failures, and privilege escalations
- Critical for spotting brute-force or lateral movement

---

## Tools and Techniques

- **Syslog**: Common protocol for log forwarding
- **SIEM (Security Information and Event Management)**: Central platform to aggregate, normalize, and analyze logs
- **Alerting Rules**: Custom conditions that trigger alerts (e.g., multiple failed logins, outbound traffic on odd ports)

---

## Best Practices

- Centralize log storage for retention and analysis
- Set clear log retention policies
- Continuously tune detection rules to reduce false positives
- Monitor both successful and failed activities

---

> Log monitoring is the digital surveillance of your infrastructure—quiet, continuous, and crucial for detecting the first signs of compromise.

