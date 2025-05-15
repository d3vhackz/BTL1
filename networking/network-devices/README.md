# Network Devices

Network devices are physical or virtual components that connect systems, route traffic, and enforce boundaries within networks. Understanding how each device operates is foundational for networking and security.

---

## Router

- **Purpose:** Forwards data packets between different networks using IP addresses
- **Layer:** Operates at OSI Layer 3 (Network)
- **Example Use Case:** Directing internet traffic from your home or office to the ISP

---

## Switch

- **Purpose:** Connects devices within a local network and forwards traffic based on MAC addresses
- **Layer:** OSI Layer 2 (Data Link)
- **Smarter than hubs** — only sends data to the intended recipient
- **Can support VLANs** and port security features

---

## Hub

- **Purpose:** Broadcasts incoming packets to all connected devices
- **Layer:** OSI Layer 1 (Physical)
- **Very outdated** — replaced by switches in modern networks

---

## Bridge

- **Purpose:** Connects two separate networks and makes them act as one
- **Layer:** OSI Layer 2
- **Use Case:** Joining two network segments while filtering traffic based on MAC address

---

## Firewall

- **Purpose:** Filters traffic based on rules; allows or blocks traffic between zones
- **Layer:** Can operate across multiple layers (typically Layers 3–7)
- **Types:**
  - **Network Firewalls:** Control traffic between networks (e.g., perimeter firewall)
  - **Host-based Firewalls:** Control traffic on individual devices

---

## Access Point (AP)

- **Purpose:** Allows wireless devices to connect to a wired network
- **Layer:** OSI Layer 2
- **Security Features:** WPA3, MAC filtering, captive portals

---

## Modem

- **Purpose:** Converts digital data to analog signals and vice versa for transmission over telephone or cable lines
- **Layer:** OSI Layer 1

---

## Load Balancer

- **Purpose:** Distributes incoming traffic across multiple servers
- **Layer:** Layer 4 (Transport) or Layer 7 (Application) depending on configuration
- **Benefits:** Improves performance, redundancy, and fault tolerance

---

> Every device plays a distinct role in controlling how data moves and who can access what. Knowing how they work is essential for securing, segmenting, and scaling networks.

