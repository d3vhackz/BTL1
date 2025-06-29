# 4.14: Linux System Logs and Package Information

The `/var/lib` and `/var/log` directories contain crucial forensic artifacts for understanding system configuration, installed software, authentication events, and web server activity on Linux systems.

---

## /var/lib Directory Analysis

### Package Management Artifacts

#### Debian-based Systems: /var/lib/dpkg/status

**Location**: `/var/lib/dpkg/status`
**Purpose**: Complete inventory of installed software packages

This file serves as a comprehensive database of all installed packages, providing valuable intelligence about system capabilities and potential tools available to attackers or users.

#### Forensic Analysis Techniques

**Manual Analysis Workflow**:
```bash
# Copy status file for analysis
cp /var/lib/dpkg/status ~/analysis/dpkg_status_$(date +%Y%m%d)

# Extract package names only
cat /var/lib/dpkg/status | grep "^Package:" > installed_packages.txt

# Search for specific security tools
grep -i "Package: \(nmap\|metasploit\|nikto\|steghide\|exiftool\)" /var/lib/dpkg/status
```

#### Investigative Focus Areas

| Category | Example Packages | Investigative Significance |
|----------|------------------|---------------------------|
| **Network Security Tools** | nmap, ncat, masscan | Network reconnaissance capability |
| **Steganography Tools** | steghide, outguess, stegdetect | Data hiding and covert communication |
| **Forensic Tools** | sleuthkit, autopsy, volatility | Counter-forensic awareness |
| **Penetration Testing** | metasploit, burpsuite, sqlmap | Attack tool availability |
| **Cryptography** | gpg, openssl, cryptsetup | Encryption and data protection |

#### Practical Analysis Example

**Scenario**: Investigating potential insider threat
```bash
# Search for reconnaissance tools
grep -E "Package: (nmap|masscan|zmap)" /var/lib/dpkg/status

# Look for data exfiltration tools  
grep -E "Package: (curl|wget|rsync|scp)" /var/lib/dpkg/status

# Check for anonymization tools
grep -E "Package: (tor|proxychains|torsocks)" /var/lib/dpkg/status
```

---

## /var/log Directory Analysis

### Authentication and Security Logs

#### Critical Log Files

| Log File | Purpose | Key Information |
|----------|---------|-----------------|
| **auth.log** | System authentication events | User logins, sudo usage, authentication failures |
| **secure** | Authentication and authorization | SSH connections, privilege escalation attempts |
| **btmp** | Failed login attempts | Brute force detection, account enumeration |
| **faillog** | User login failures | Account-specific failure patterns |

#### System Management Logs

| Log File | Purpose | Forensic Value |
|----------|---------|----------------|
| **dpkg.log** | Package installation/removal | Software installation timeline |
| **cron** | Scheduled task execution | Persistence mechanism detection |

### Authentication Log Analysis

#### /var/log/auth.log Investigation

**Key Event Types**:
```bash
# SSH login attempts
grep "sshd" /var/log/auth.log | grep "Failed password"

# Successful logins
grep "sshd" /var/log/auth.log | grep "Accepted"

# Sudo command execution
grep "sudo" /var/log/auth.log

# User account changes
grep -E "(useradd|userdel|usermod)" /var/log/auth.log
```

#### Failed Login Pattern Analysis

**Brute Force Detection**:
```bash
# Count failed attempts by IP
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr

# Timeline of failed attempts
grep "Failed password" /var/log/auth.log | awk '{print $1, $2, $3, $11}' | head -20

# Successful logins after failures
grep -A5 -B5 "Accepted password" /var/log/auth.log
```

### Package Installation Timeline

#### /var/log/dpkg.log Analysis

**Installation Tracking**:
```bash
# Recent package installations
grep "install" /var/log/dpkg.log | tail -20

# Package removal events
grep "remove" /var/log/dpkg.log

# Security-related package changes
grep -E "(nmap|metasploit|wireshark)" /var/log/dpkg.log
```

---

## Web Server Log Analysis

### Apache Access Log Forensics

**Location**: `/var/log/apache2/access.log`

#### Log Format Structure
```
IP - User [Timestamp] "Method Resource Protocol" Status Size
```

**Example Entry Analysis**:
```
52.50.100.106 - SBTUser [27/Jul/2020:15:30:00 -0600] "GET /logo.png HTTP/1.1" 200 379
```

| Field | Value | Forensic Significance |
|-------|-------|----------------------|
| **IP Address** | 52.50.100.106 | Source of request (geolocation, reputation) |
| **User** | SBTUser | Authenticated user identification |
| **Timestamp** | 27/Jul/2020:15:30:00 | Precise timing for timeline correlation |
| **Method** | GET | HTTP method (GET, POST, PUT, DELETE) |
| **Resource** | /logo.png | Requested file/endpoint |
| **Status Code** | 200 | Success/failure indication |
| **Size** | 379 | Response size (data exfiltration detection) |

### Attack Pattern Detection

#### Common Attack Signatures

**SQL Injection Attempts**:
```bash
grep -i "union\|select\|insert\|update\|delete" /var/log/apache2/access.log
```

**Directory Traversal**:
```bash
grep -E "\.\./\.\./\.\." /var/log/apache2/access.log
```

**Vulnerability Scanning**:
```bash
# Nikto signatures
grep -i "nikto" /var/log/apache2/access.log

# Rapid sequential requests (scanning behavior)
awk '{print $1}' /var/log/apache2/access.log | sort | uniq -c | sort -nr | head -10
```

#### User-Agent Analysis

**Suspicious User-Agents**:
```bash
# Extract unique user-agents
awk -F'"' '{print $6}' /var/log/apache2/access.log | sort | uniq -c | sort -nr

# Look for automated tools
grep -E "(sqlmap|nikto|nmap|curl|wget|python)" /var/log/apache2/access.log
```

---

## Advanced Analysis Techniques

### Timeline Correlation

#### Multi-Log Timeline Creation
```bash
# Combine authentication and web access events
(grep "sshd.*Accepted" /var/log/auth.log | awk '{print $1, $2, $3, "SSH_LOGIN", $9}';
 grep "GET\|POST" /var/log/apache2/access.log | awk '{print $4, "WEB_ACCESS", $1, $7}') | sort
```

### Automated Analysis Scripts

#### Suspicious Activity Detection
```bash
#!/bin/bash
# Basic threat hunting script

echo "=== Failed SSH Attempts ==="
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr | head -5

echo "=== Top Web Requesters ==="
awk '{print $1}' /var/log/apache2/access.log | sort | uniq -c | sort -nr | head -5

echo "=== Recent Package Installations ==="
grep "install" /var/log/dpkg.log | tail -5

echo "=== Security Tools Installed ==="
grep -E "Package: (nmap|nikto|metasploit|wireshark)" /var/lib/dpkg/status
```

### Evidence Preservation

#### Log Collection Best Practices
```bash
# Create forensic copies with timestamps
mkdir /evidence/logs_$(date +%Y%m%d_%H%M%S)
cp -p /var/log/auth.log /evidence/logs_*/
cp -p /var/log/apache2/access.log /evidence/logs_*/
cp -p /var/lib/dpkg/status /evidence/logs_*/

# Generate integrity hashes
find /evidence/logs_* -type f -exec sha256sum {} \; > /evidence/log_hashes.txt
```

[⬆️ Back to Digital Forensics](./README.md)
