# 4.16: Linux Memory Artifacts

Memory forensics on Linux systems provides access to volatile data that exists only while the system is running. This includes active processes, network connections, loaded modules, and cached data that can be crucial for incident response and forensic investigations.

---

## Memory Forensics Fundamentals

### Why Linux Memory Analysis Matters

Memory artifacts provide unique investigative opportunities that complement traditional disk-based evidence:

| Artifact Type | Information Available | Investigative Value |
|---------------|----------------------|-------------------|
| **Running Processes** | Active malware, hidden processes | Real-time threat detection |
| **Network Connections** | Active C2 communications | Attribution and data flow |
| **Loaded Modules** | Rootkits, kernel modifications | Advanced persistent threats |
| **Process Memory** | Encryption keys, passwords | Data recovery and decryption |
| **File Handles** | Open files, deleted file recovery | Access pattern analysis |
| **System Calls** | Real-time activity monitoring | Behavioral analysis |

### Memory Acquisition Challenges

Linux memory acquisition presents unique technical challenges:
- **Kernel module requirements** for live acquisition
- **System stability** during memory dumping
- **Large memory sizes** in modern systems
- **Anti-forensics techniques** targeting memory analysis

---

## Memory Acquisition Tools

### LiME (Linux Memory Extractor)

**LiME** is the primary tool for Linux memory acquisition, designed specifically for forensic memory dumps.

**Repository**: https://github.com/504ensicsLabs/LiME

#### Key Features
- **Kernel module-based** acquisition for complete memory access
- **Multiple output formats** (raw, padded, compressed)
- **Network transmission** capability for remote acquisition
- **Minimal system impact** during acquisition process

#### Installation and Compilation

```bash
# Clone LiME repository
git clone https://github.com/504ensicsLabs/LiME

# Navigate to source directory
cd LiME/src

# Compile for current kernel
make

# Load the module for acquisition
sudo insmod lime.ko "path=/evidence/memory.lime format=lime"
```

#### Acquisition Options

| Parameter | Purpose | Example Values |
|-----------|---------|----------------|
| **path** | Output location | `/tmp/memory.dump`, `tcp:4444` |
| **format** | Output format | `lime` (default), `padded`, `raw` |
| **dio** | Direct I/O | `1` (enabled), `0` (disabled) |

### memdump Utility

**memdump** provides an alternative approach for memory acquisition on systems where LiME cannot be compiled.

**Installation**:
```bash
sudo apt-get install memdump
```

**Usage**:
```bash
# Dump physical memory
sudo memdump > /evidence/memory_dump.raw

# Dump specific memory regions
sudo memdump -s 0x1000000 -l 0x10000 > /evidence/partial_dump.raw
```

#### Advantages and Limitations

**Advantages**:
- Pre-compiled utility availability
- Simple command-line interface
- No kernel module compilation required

**Limitations**:
- Less comprehensive than LiME
- May miss certain memory regions
- Limited format options

---

## Memory Analysis with Volatility

### Volatility Framework Integration

**Volatility** is the industry standard for memory analysis, supporting Linux memory dumps through specialized plugins.

#### Profile Selection for Linux

```bash
# Identify appropriate profile
volatility -f memory.lime imageinfo

# List available Linux profiles
volatility --info | grep Linux

# Use specific Linux profile
volatility -f memory.lime --profile=LinuxUbuntu1804x64 pslist
```

### Essential Linux Memory Analysis Commands

#### Process Analysis

```bash
# List running processes
volatility -f memory.lime --profile=LinuxProfile linux_pslist

# Process tree visualization
volatility -f memory.lime --profile=LinuxProfile linux_pstree

# Hidden process detection
volatility -f memory.lime --profile=LinuxProfile linux_psxview

# Process memory dumping
volatility -f memory.lime --profile=LinuxProfile linux_procdump -p [PID] -D /output/
```

#### Network Artifacts

```bash
# Active network connections
volatility -f memory.lime --profile=LinuxProfile linux_netstat

# Network interface information
volatility -f memory.lime --profile=LinuxProfile linux_ifconfig

# ARP table contents
volatility -f memory.lime --profile=LinuxProfile linux_arp
```

#### File System Artifacts

```bash
# Open file handles
volatility -f memory.lime --profile=LinuxProfile linux_lsof

# Mount point information
volatility -f memory.lime --profile=LinuxProfile linux_mount

# Recover deleted files from memory
volatility -f memory.lime --profile=LinuxProfile linux_recover_filesystem
```

---

## Advanced Memory Analysis Techniques

### Rootkit Detection

#### Kernel Module Analysis

```bash
# List loaded kernel modules
volatility -f memory.lime --profile=LinuxProfile linux_lsmod

# Check for hidden modules
volatility -f memory.lime --profile=LinuxProfile linux_check_modules

# Analyze module structures
volatility -f memory.lime --profile=LinuxProfile linux_moddump -D /output/
```

#### System Call Table Inspection

```bash
# Examine system call table for hooks
volatility -f memory.lime --profile=LinuxProfile linux_check_syscall

# Analyze interrupt descriptor table
volatility -f memory.lime --profile=LinuxProfile linux_check_idt
```

### Malware Analysis

#### Process Memory Examination

```bash
# Extract process memory maps
volatility -f memory.lime --profile=LinuxProfile linux_proc_maps -p [PID]

# Dump process memory segments
volatility -f memory.lime --profile=LinuxProfile linux_dump_map -p [PID] -s [START_ADDR]

# Search for specific patterns in memory
volatility -f memory.lime --profile=LinuxProfile linux_yarascan -Y malware_rules.yar
```

#### Environmental Analysis

```bash
# Extract environment variables
volatility -f memory.lime --profile=LinuxProfile linux_psenv

# Analyze command-line arguments
volatility -f memory.lime --profile=LinuxProfile linux_psaux

# Examine process working directories
volatility -f memory.lime --profile=LinuxProfile linux_pwd
```

---

## Live Memory Analysis

### Real-time Investigation Techniques

#### /proc Filesystem Analysis

For live systems, the `/proc` filesystem provides memory-related information:

```bash
# Process information
cat /proc/[PID]/status
cat /proc/[PID]/maps
cat /proc/[PID]/environ

# System memory information
cat /proc/meminfo
cat /proc/slabinfo

# Kernel information
cat /proc/modules
cat /proc/kallsyms
```

#### Memory Inspection Tools

```bash
# Process memory examination
sudo gdb -p [PID]
(gdb) info proc mappings
