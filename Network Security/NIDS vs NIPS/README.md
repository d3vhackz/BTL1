# NIDS vs NIPS

Network-based Intrusion Detection and Prevention Systems (NIDS/NIPS) are critical for monitoring and defending network traffic against malicious activity.

---

## NIDS (Network Intrusion Detection System)

**Purpose:**  
Monitors network traffic passively and alerts on suspicious or malicious activity.

**Key Features:**
- Sits off a SPAN port, tap, or mirror port
- Does not interfere with traffic flow
- Ideal for forensic analysis and real-time alerts

**Deployment Modes:**
- **Passive** – Observes traffic without interfering
- **Network Tap** – Connected directly to the wire, mirroring traffic
- **SPAN Port** – Gets a copy of traffic from switches

**Limitations:**
- Cannot block traffic
- Can be overwhelmed on high-throughput networks

---

## NIPS (Network Intrusion Prevention System)

**Purpose:**  
Actively monitors and blocks malicious traffic in real time.

**Key Features:**
- Inline deployment (sits in the path of traffic)
- Can drop packets, reset connections, and prevent intrusions
- Prevents attacks like port scans, brute force attempts, and malware delivery

**Use Case Example:**  
If an internal host begins scanning the network, NIPS can detect this and immediately block outbound connections.

**Risks:**
- If misconfigured, it can block legitimate traffic
- Must be properly tuned to avoid false positives

---

## Summary

| Feature | NIDS | NIPS |
|--------|------|------|
| Deployment | Passive | Inline |
| Blocking | ❌ No | ✅ Yes |
| Risk of Downtime | Low | Moderate |
| Use Case | Alerting | Prevention |

---

> Use NIDS for visibility and detection. Use NIPS for prevention and response. Together, they form a powerful layer of network defense.

