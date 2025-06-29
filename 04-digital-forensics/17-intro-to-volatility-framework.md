# 4.17: Introduction to Volatility Framework

Volatility is the industry-standard open-source memory forensics framework used for incident response and malware analysis. Originally developed by computer scientist Aaron Walters, it provides comprehensive capabilities for analyzing memory dumps across multiple operating systems.

---

## Framework Overview

### Core Capabilities

Volatility enables investigators to extract critical forensic artifacts from memory dumps:

| Category | Capabilities | Investigative Value |
|----------|--------------|-------------------|
| **Process Analysis** | Running processes, hidden processes, process trees | Malware detection, user activity tracking |
| **Network Forensics** | Active/closed connections, listening ports | C2 communication, data exfiltration analysis |
| **Browser Artifacts** | Internet history, cached pages, downloads | User behavior reconstruction |
| **File System** | File handles, cached files, file recovery | Data access patterns, evidence recovery |
| **User Activity** | Command history, clipboard contents, screenshots | Timeline reconstruction, user attribution |
| **Malware Detection** | YARA scanning, code injection, rootkit detection | Threat identification and analysis |
| **Cryptographic Artifacts** | SSL keys, certificates, password hashes | Decryption capabilities, credential analysis |

### Supported Operating Systems

- **Microsoft Windows** - XP through Windows 11
- **Linux** - Various distributions and kernel versions  
- **macOS** - Multiple versions with profile support

---

## Memory Forensics Fundamentals

### Why Memory Analysis Matters

Memory forensics provides unique investigative opportunities:

#### Volatile Evidence Recovery
- **Active processes** and their command-line arguments
- **Network connections** established at time of capture
- **Encryption keys** loaded in memory for data recovery
- **Cached data** not yet written to disk

#### Anti-forensics Detection
- **Hidden processes** used by advanced malware
- **Code injection** techniques and DLL manipulation
- **Rootkit presence** through system call hooks
- **Process hollowing** and other evasion methods

#### Timeline Reconstruction
- **Process creation times** for activity sequencing
- **Network connection timestamps** for communication analysis
- **File access patterns** from cached metadata
- **User interaction evidence** from GUI artifacts

---

## Volatility Architecture

### Plugin-based Framework

Volatility uses a modular plugin architecture for different analysis tasks:

#### Core Plugin Categories

| Plugin Type | Function | Common Plugins |
|-------------|----------|----------------|
| **Process Plugins** | Process enumeration and analysis | `pslist`, `pstree`, `psscan` |
| **Network Plugins** | Network connection analysis | `netscan`, `netstat`, `sockets` |
| **Registry Plugins** | Windows registry analysis | `hivelist`, `printkey`, `hashdump` |
| **File Plugins** | File system and handle analysis | `filescan`, `dumpfiles`, `handles` |
| **Malware Plugins** | Threat detection and analysis | `yarascan`, `malfind`, `apihooks` |

### Memory Dump Sources

Volatility can analyze memory dumps from various acquisition tools:

- **FTK Imager** - Forensic imaging with live memory capture
- **DumpIt** - Simple Windows memory acquisition
- **LiME** - Linux memory extraction tool
- **VMware** - Virtual machine memory snapshots
- **Hibernation files** - Windows hiberfil.sys analysis

---

## Profile-based Analysis (Volatility 2)

### Understanding Profiles

**Profiles** in Volatility 2 contain operating system-specific information needed to interpret memory structures correctly.

#### Profile Identification Workflow

```bash
# Step 1: Identify appropriate profile
volatility -f memory.dump imageinfo

# Example output analysis:
# Suggested Profile(s): Win7SP1x64, Win7SP0x64, Win2008R2SP0x64
# KDBG: 0xf80002c0c0a0 (64-bit)
# Number of Processors: 2
```

#### Profile Usage in Commands

All Volatility 2 commands require profile specification:

```bash
# Generic command structure
volatility -f [MEMORY_FILE] --profile=[PROFILE] [PLUGIN] [OPTIONS]

# Practical example
volatility -f memory.dump --profile=Win7SP1x64 pslist
```

### Essential Volatility 2 Commands

#### Process Analysis Commands

```bash
# List running processes
volatility -f memory.dump --profile=Win7SP1x64 pslist

# Display process tree structure
volatility -f memory.dump --profile=Win7SP1x64 pstree

# Scan for hidden processes (malware detection)
volatility -f memory.dump --profile=Win7SP1x64 psscan

# Combined process view with detection methods
volatility -f memory.dump --profile=Win7SP1x64 psxview

# Extract process executable
volatility -f memory.dump --profile=Win7SP1x64 procdump -p [PID]
```

#### Network Analysis Commands

```bash
# Active and closed network connections
volatility -f memory.dump --profile=Win7SP1x64 netscan

# Display listening sockets
volatility -f memory.dump --profile=Win7SP1x64 sockets
```

#### File System Commands

```bash
# List files referenced in memory
volatility -f memory.dump --profile=Win7SP1x64 filescan

# Extract files from memory
volatility -f memory.dump --profile=Win7SP1x64 dumpfiles -n --dump-dir=./extracted/

# Show command-line arguments for process
volatility -f memory.dump --profile=Win7SP1x64 cmdline -p [PID]
```

#### Browser and User Activity

```bash
# Internet Explorer browsing history
volatility -f memory.dump --profile=Win7SP1x64 iehistory

# Timeline of system events
volatility -f memory.dump --profile=Win7SP1x64 timeliner
```

---

## Advanced Analysis Techniques

### Malware Detection Workflow

#### Multi-stage Process Analysis

```bash
# 1. Compare process enumeration methods
volatility -f memory.dump --profile=Win7SP1x64 pslist > visible_processes.txt
volatility -f memory.dump --profile=Win7SP1x64 psscan > all_processes.txt

# 2. Identify discrepancies (hidden processes)
diff visible_processes.txt all_processes.txt

# 3. Analyze suspicious processes
volatility -f memory.dump --profile=Win7SP1x64 cmdline -p [SUSPICIOUS_PID]
volatility -f memory.dump --profile=Win7SP1x64 procdump -p [SUSPICIOUS_PID]
```

#### Network Forensics Investigation

```bash
# 1. Identify active connections
volatility -f memory.dump --profile=Win7SP1x64 netscan | grep ESTABLISHED

# 2. Correlate with processes
volatility -f memory.dump --profile=Win7SP1x64 netscan | grep [SUSPICIOUS_IP]

# 3. Extract process making connections
volatility -f memory.dump --profile=Win7SP1x64 procdump -p [NETWORK_PID]
```

### Timeline Analysis Methodology

#### Comprehensive Timeline Creation

```bash
# Generate timeline with multiple artifacts
volatility -f memory.dump --profile=Win7SP1x64 timeliner --output-file=timeline.txt

# Combine with other forensic timelines
# - File system timeline (from disk analysis)
# - Log file timeline (from system logs)
# - Network timeline (from packet captures)
```

---

## Investigation Best Practices

### Systematic Analysis Approach

#### Phase 1: Initial Assessment
1. **Profile identification** using `imageinfo`
2. **Process enumeration** with `pslist` and `psscan`
3. **Network connection analysis** using `netscan`
4. **File system overview** with `filescan`

#### Phase 2: Threat Detection
1. **Hidden process identification** by comparing enumeration methods
2. **Network anomaly detection** through connection analysis
3. **Malware artifact search** using specialized plugins
4. **Code injection detection** with `malfind` plugin

#### Phase 3: Evidence Extraction
1. **Process memory dumps** for malware analysis
2. **File extraction** from memory caches
3. **Registry artifact collection** for persistence analysis
4. **Timeline generation** for incident reconstruction

### Documentation Standards

#### Analysis Documentation Template

```bash
# Memory Analysis Report Header
echo "=== Volatility Analysis Report ===" > analysis_report.txt
echo "Memory Dump: $(basename memory.dump)" >> analysis_report.txt
echo "Analysis Date: $(date)" >> analysis_report.txt
echo "Analyst: [Name]" >> analysis_report.txt

# Profile Information
echo -e "\n=== Profile Information ===" >> analysis_report.txt
volatility -f memory.dump imageinfo >> analysis_report.txt

# Process Analysis
echo -e "\n=== Running Processes ===" >> analysis_report.txt
volatility -f memory.dump --profile=Win7SP1x64 pslist >> analysis_report.txt

# Network Connections  
echo -e "\n=== Network Connections ===" >> analysis_report.txt
volatility -f memory.dump --profile=Win7SP1x64 netscan >> analysis_report.txt
```

[⬆️ Back to Digital Forensics](./README.md)
