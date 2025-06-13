# 1.2: Networking Essentials

A strong understanding of networking protocols, devices, and tools is essential for any blue team professional.

---

## OSI Model

The **OSI (Open Systems Interconnection) Model** is a conceptual framework that standardizes the functions of a telecommunication or computing system in seven abstract layers.

| Layer | Name          | Purpose & Protocols                                | Mnemonic     |
|-------|---------------|----------------------------------------------------|--------------|
| **7** | Application   | User-facing protocols (HTTP, SMTP, DNS).           | **A**ll      |
| **6** | Presentation  | Data translation, encryption, compression (TLS, JPEG). | **P**eople   |
| **5** | Session       | Manages connections between applications (APIs, Sockets). | **S**eem     |
| **4** | Transport     | End-to-end communication and reliability (TCP, UDP). | **T**o       |
| **3** | Network       | Routing and logical addressing (IP, ICMP).         | **N**eed     |
| **2** | Data Link     | Physical addressing and local delivery (Ethernet, MAC). | **D**ata     |
| **1** | Physical      | Raw bit transmission (Cables, Hubs).               | **P**rocessing|

---

## TCP vs. UDP vs. ICMP

| Protocol | Type             | Reliability | Key Feature                                  | Use Cases                      |
|----------|------------------|-------------|----------------------------------------------|--------------------------------|
| **TCP**  | Connection-based | ✅ Yes      | Three-way handshake ensures ordered delivery. | Web browsing (HTTPS), Email, FTP. |
| **UDP**  | Connectionless   | ❌ No       | Fast, lightweight, "fire and forget."        | Video streaming, DNS, VoIP.    |
| **ICMP** | Network-level    | N/A         | Used for error reporting and diagnostics.    | `ping`, `traceroute`.          |

---

## Common Ports & Protocols

| Port | Protocol | Purpose                             |
|------|----------|-------------------------------------|
| 21   | FTP      | File Transfer Protocol (insecure)   |
| 22   | SSH      | Secure Shell                        |
| 25   | SMTP     | Simple Mail Transfer Protocol       |
| 53   | DNS      | Domain Name System                  |
| 80   | HTTP     | HyperText Transfer Protocol         |
| 443  | HTTPS    | HTTP Secure                         |
| 143  | IMAP     | Internet Message Access Protocol    |
| 445  | SMB      | Server Message Block (File Sharing) |
| 3389 | RDP      | Remote Desktop Protocol             |

---

## Network Devices

| Device         | OSI Layer | Function                                                                    |
|----------------|-----------|-----------------------------------------------------------------------------|
| **Hub**        | Layer 1   | Broadcasts packets to all connected devices. Outdated.                      |
| **Switch**     | Layer 2   | Forwards packets to specific devices on a LAN using MAC addresses.          |
| **Router**     | Layer 3   | Forwards packets between different networks using IP addresses.             |
| **Firewall**   | Layer 3-7 | Filters traffic between networks based on a set of security rules.          |
| **Access Point**| Layer 2   | Allows wireless devices to connect to a wired network.                      |
| **Modem**      | Layer 1   | Modulates/demodulates signals for transmission over phone/cable lines.      |

---

## Network CLI Tools

| Tool                  | Purpose                                        | Common Command Examples                               |
|-----------------------|------------------------------------------------|-------------------------------------------------------|
| `ip`/`ifconfig`         | View and configure network interfaces.         | `ip a` (Linux), `ipconfig /all` (Windows)             |
| `ping`                | Test host reachability using ICMP.             | `ping 8.8.8.8`                                        |
| `traceroute`/`tracert`  | Map the network path to a destination host.    | `traceroute google.com`, `tracert 8.8.8.8`              |
| `netstat`             | Display active network connections and ports.  | `netstat -an`                                         |
| `nslookup`/`dig`      | Query DNS servers for name resolution.         | `nslookup google.com`, `dig google.com MX`              |
| `nmap`                | Discover hosts and services on a network.      | `nmap -sV -p- 192.168.1.1`                            |

[⬆️ Back to Security Fundamentals](./README.md)