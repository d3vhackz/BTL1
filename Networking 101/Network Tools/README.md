# Network Tools

Network tools are essential for diagnostics, discovery, and security monitoring. Mastering these CLI tools enables faster incident response, troubleshooting, and visibility into what's happening on your network.

---

## IP / ipconfig / ifconfig

- **Purpose:** View and configure network interface settings
- **Commands:**
  - `ip a` – Show IP addresses
  - `ip r` or `route` – Show routing table
  - `ip link set dev [interface] up/down` – Enable/disable a network interface
  - `ipconfig` (Windows) – Show IP, gateway, DNS
  - `ifconfig` (Linux) – Legacy tool to view interface info

---

## Traceroute / tracert

- **Purpose:** Shows the path packets take to reach a destination
- **Use Case:** Diagnose where traffic is slowing or being blocked
- **Commands:**
  - `traceroute domain.com` (Linux/macOS)
  - `tracert domain.com` (Windows)
  - `traceroute -p [port] domain.com` – Specify port for trace

---

## dig / nslookup

- **Purpose:** Query DNS servers for domain name records
- **Use Case:** Identify IPs, MX records, DNS misconfigurations
- **Commands:**
  - `dig example.com` – Get A record
  - `dig example.com MX` – Get mail records
  - `dig example.com ANY +noall +answer` – Get all records cleanly
  - `nslookup example.com` – Basic DNS query tool (Windows)

---

## netstat

- **Purpose:** View active network connections and listening ports
- **Use Case:** Detect C2 traffic, unauthorized connections
- **Commands:**
  - `netstat -a` – All connections and ports
  - `netstat -a -b` – Show executables using each connection (Windows)
  - `netstat -s -p tcp` – TCP-specific statistics

---

## nmap

- **Purpose:** Perform network discovery and security scanning
- **Capabilities:**
  - Discover live hosts
  - Identify open ports and services
  - Detect OS types and versions
  - Use NSE scripts for deeper vulnerability detection

- **Example Command:**  
  `nmap -sV -T4 -O -F --version-light 192.168.1.0/24`

- **Result Columns:**
  - **PORT** – Port number (e.g., 22)
  - **STATE** – Open, closed, or filtered
  - **SERVICE** – The service running on that port (e.g., SSH)

- **Cheat Sheet:**  
  [Nmap Cheat Sheet (PDF)](https://s3-us-west-2.amazonaws.com/stationx-public-download/nmap_cheet_sheet_0.6.pdf)

---

> These tools are the digital equivalent of a stethoscope and X-ray. Learn them well, and your visibility into networks and threats will level up dramatically.

