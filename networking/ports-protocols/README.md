# Ports and Protocols

Ports and protocols are foundational to how devices communicate over networks. Understanding them is essential for network monitoring, firewall configuration, and threat detection.

---

## Port Ranges

- **Well-Known Ports:** 0 – 1023  
  Used by core protocols (e.g., HTTP, FTP, SSH)

- **Registered Ports:** 1024 – 49151  
  Used by user applications and services

- **Dynamic/Ephemeral Ports:** 49152 – 65535  
  Used temporarily for client-side connections

---

## Common Ports and Protocols

| Port | Protocol | Purpose |
|------|----------|---------|
| 20, 21 | FTP | Transfers files between systems (insecure) |
| 22 | SSH | Secure remote login and command execution |
| 23 | Telnet | Legacy remote login (insecure) |
| 25 | SMTP | Sends email between mail servers |
| 53 | DNS | Resolves domain names to IP addresses (UDP/TCP) |
| 67, 68 | DHCP | Assigns IP addresses to devices |
| 80 | HTTP | Unencrypted web traffic |
| 443 | HTTPS | Encrypted web traffic using TLS |
| 110 | POP3 | Retrieves email (downloads and deletes from server) |
| 143 | IMAP | Accesses email stored on a server |
| 389 | LDAP | Directory access protocol |
| 445 | SMB | Windows file sharing and network resource access |
| 514 | Syslog | Forwards log data to a log server (UDP) |
| 3389 | RDP | Remote desktop access (Windows) |

---

## Ephemeral Port Example

When you connect to a web server:
- **Destination Port:** 443 (HTTPS)
- **Source Port:** Random ephemeral port (e.g., 50932)

---

## Best Practices

- Only open necessary ports on firewalls and servers
- Monitor for unusual port activity (e.g., SSH on unexpected ports)
- Use encrypted protocols (e.g., HTTPS, SFTP) whenever possible
- Document all service-port mappings for auditing

---

> Every protocol is a language. Every port is a door. Know which ones are open, which ones are guarded, and which ones should never exist in your environment.

