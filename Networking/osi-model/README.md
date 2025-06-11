# OSI Model

The OSI (Open Systems Interconnection) Model is a conceptual framework used to understand and design how different networking protocols interact in a layered architecture.

---

## 7 Layers of the OSI Model

### Layer 7 – Application
- Closest to the end user
- Interfaces with software applications like web browsers and email clients
- **Examples:** HTTP, SMTP, FTP, DNS

---

### Layer 6 – Presentation
- Translates data into a format readable by the application layer
- Handles encryption, compression, and encoding
- **Examples:** SSL/TLS, JPEG, MPEG, ASCII

---

### Layer 5 – Session
- Manages sessions (connections) between devices
- Controls dialog (opening, maintaining, and closing sessions)
- **Examples:** NetBIOS, RPC, APIs, Sockets

---

### Layer 4 – Transport
- Provides end-to-end communication and flow control
- Responsible for error checking and retransmissions
- **Examples:** TCP, UDP

---

### Layer 3 – Network
- Handles routing and logical addressing
- Determines best path to destination
- **Examples:** IP, ICMP, IGMP

---

### Layer 2 – Data Link
- Handles physical addressing and error detection on the local network
- Divided into two sublayers: MAC and LLC
- **Examples:** Ethernet, ARP, PPP, Switches

---

### Layer 1 – Physical
- Deals with physical transmission of raw bits
- Includes cables, switches, voltages, and other hardware
- **Examples:** Ethernet cables, Hubs, Fiber optics, Wireless signals

---

## Mnemonic to Remember the Layers

**From top (Layer 7) to bottom (Layer 1):**  
> All People Seem To Need Data Processing

**Reverse (Layer 1 to Layer 7):**  
> Please Do Not Throw Sausage Pizza Away

---

## Why OSI Matters

- Helps isolate issues (e.g., Layer 1 = cable unplugged, Layer 4 = port blocked)
- Used in network design, security architecture, and protocol analysis
- A foundational model for understanding how network traffic flows

---

> Mastering the OSI Model allows you to troubleshoot like a pro, design networks intelligently, and speak the language of cybersecurity and IT fluently.

