# TCP vs UDP vs ICMP

Understanding core transport and network protocols is critical for analyzing traffic, building detections, and troubleshooting issues.

---

## TCP (Transmission Control Protocol)

**Type:** Connection-oriented

**Key Features:**
- Ensures reliable, ordered delivery of packets
- Performs error checking and retransmission
- Used for applications where accuracy matters

**Three-Way Handshake:**
1. Client sends SYN (synchronize) to server
2. Server replies with SYN-ACK
3. Client sends ACK and begins data transfer

**Common Use Cases:**
- Web browsing (HTTP/HTTPS)
- Email (SMTP, IMAP, POP3)
- File transfers (FTP)

---

## UDP (User Datagram Protocol)

**Type:** Connectionless

**Key Features:**
- No guarantee of delivery or order
- Lightweight and fast
- Minimal overhead, no handshake

**Common Use Cases:**
- Video streaming and gaming
- VoIP and DNS queries
- SNMP monitoring

---

## ICMP (Internet Control Message Protocol)

**Type:** Network-level protocol

**Purpose:**
- Used for diagnostic and error-reporting functions
- Does not transfer application data

**Common ICMP Messages:**
- **Echo Request/Reply**: Used by `ping` to check host availability
- **Destination Unreachable**: Indicates routing or access problems
- **Time Exceeded**: Used in `traceroute` to map hops

**Security Note:**  
ICMP can be abused for network reconnaissance (e.g., ping sweeps), so it’s often restricted or monitored.

---

## Comparison Summary

| Protocol | Type             | Reliable | Ordered | Use Case Examples         |
|----------|------------------|----------|---------|---------------------------|
| TCP      | Connection-based | ✅ Yes   | ✅ Yes  | HTTPS, SMTP, FTP          |
| UDP      | Connectionless    | ❌ No    | ❌ No   | DNS, VoIP, Video Streams  |
| ICMP     | Diagnostic Layer  | N/A      | N/A     | Ping, Traceroute          |

---

> TCP guarantees delivery, UDP guarantees speed, and ICMP helps you figure out what’s working—or not—on the network.

