# 4.15: Linux User Activity and Hidden Data

User file analysis on Linux systems reveals command history, hidden files, and sophisticated data concealment techniques including steganography. This analysis is crucial for understanding user behavior and uncovering covert activities.

---

## Command History Analysis

### Bash History Artifacts

**Location**: `~/.bash_history` (hidden file in user's home directory)

The bash history file maintains a record of commands executed by the user, providing a detailed timeline of system interaction and potential malicious activity.

#### Discovery and Access

```bash
# Navigate to user's home directory
cd /home/username

# List hidden files (files starting with .)
ls -a

# View command history
cat .bash_history
```

#### Forensic Significance

| Information Type | Forensic Value | Investigation Focus |
|------------------|----------------|-------------------|
| **Command Sequence** | User behavior patterns | Attack methodology reconstruction |
| **Tool Usage** | Installed software utilization | Capability assessment |
| **File Access** | Sensitive data interaction | Data theft or manipulation |
| **Network Activity** | Remote connections and transfers | Lateral movement detection |

### History Persistence vs. Terminal History

#### Critical Understanding

**Terminal History** (`history` command):
- Stored in memory during session
- Can be cleared with `history -c`
- Lost when terminal closes

**Bash History File** (`.bash_history`):
- Persistent storage on disk
- Survives history clearing attempts
- Written when terminal session ends

#### Analysis Workflow

```bash
# Compare current terminal history with file
history > current_session.txt
cat .bash_history > historical_commands.txt

# Identify recently executed commands not yet written to file
diff current_session.txt historical_commands.txt
```

### Command Analysis Techniques

#### Suspicious Activity Patterns

**Network Reconnaissance**:
```bash
grep -E "(nmap|masscan|ping|traceroute)" .bash_history
```

**File System Exploration**:
```bash
grep -E "(find|locate|ls.*-.*R)" .bash_history
```

**Privilege Escalation Attempts**:
```bash
grep -E "(sudo|su|chmod|chown)" .bash_history
```

**Data Exfiltration**:
```bash
grep -E "(scp|rsync|curl|wget.*-O)" .bash_history
```

---

## Hidden File and Directory Analysis

### Linux Hidden File Convention

Files and directories beginning with "." are hidden from standard directory listings, providing a simple concealment mechanism.

#### Detection Methods

```bash
# Standard listing (misses hidden files)
ls

# Complete listing including hidden items
ls -a

# Detailed listing with permissions and timestamps
ls -la
```

### Systematic Hidden File Discovery

#### Comprehensive Search Strategy

```bash
# Find all hidden files in user directories
find /home -name ".*" -type f 2>/dev/null

# Search for recently modified hidden files
find /home -name ".*" -type f -mtime -7 2>/dev/null

# Identify hidden directories
find /home -name ".*" -type d 2>/dev/null
```

#### Investigative Focus Areas

| Location | Hidden Item Types | Potential Evidence |
|----------|------------------|-------------------|
| **Home Directory** | Config files, caches, histories | User preferences, activity logs |
| **Desktop** | Temporary files, hidden folders | Work artifacts, downloaded content |
| **Root Directory** | System caches, application data | System-wide configurations |

---

## Standard User Directory Analysis

### Primary Investigation Targets

#### User Data Locations

```bash
# Desktop files (immediate user access)
ls -la ~/Desktop/

# Download history and files
ls -la ~/Downloads/

# Document storage
ls -la ~/Documents/

# Trash bin analysis
ls -la ~/.local/share/Trash/files/
```

#### Temporal Analysis

```bash
# Recently modified files across user directories
find ~/ -type f -mtime -7 -ls 2>/dev/null | sort -k8,10

# Recently accessed files
find ~/ -type f -atime -1 -ls 2>/dev/null
```

---

## Steganography Detection and Analysis

### Concealment Techniques

Steganography allows hiding data within innocent-appearing files, making detection challenging without specific tools and techniques.

#### Common Steganographic Methods

| Method | Cover Medium | Detection Approach |
|--------|--------------|-------------------|
| **LSB Insertion** | Images (PNG, BMP) | Statistical analysis, visual inspection |
| **Metadata Embedding** | Any file type | Metadata extraction and analysis |
| **Archive Concatenation** | Images + Archives | File signature analysis |
| **Tool-specific Hiding** | Various formats | Tool signature detection |

### Archive Concatenation Analysis

#### Technique: Hiding ZIP in Images

**Detection Method**:
```bash
# Check for embedded archives in images
file suspicious_image.jpg

# Attempt archive extraction
unzip suspicious_image.jpg

# Binary analysis for embedded signatures
strings suspicious_image.jpg | grep -E "(PK|7z|RAR)"
```

**Manual Recreation Example**:
```bash
# Create steganographic image (demonstration)
cat legitimate_image.jpg secret_archive.zip > combined_image.jpg

# File appears normal but contains hidden archive
file combined_image.jpg  # Shows: JPEG image
unzip combined_image.jpg  # Extracts hidden files
```

### Steghide Analysis

#### Tool-based Steganography

**Steghide** is a popular tool for embedding data in image and audio files with password protection.

#### Detection and Extraction

```bash
# Test for steghide-embedded data
steghide info suspicious_image.jpg

# Extract embedded data (no password)
steghide extract -sf suspicious_image.jpg

# Extract with password
steghide extract -sf suspicious_image.jpg -p "password"
```

#### Automated Password Attacks

```bash
# Install StegCracker for brute force
git clone https://github.com/Paradoxis/StegCracker
pip install stegcracker

# Attempt password cracking
stegcracker suspicious_image.jpg wordlist.txt
```

### Metadata Steganography

#### ExifTool Analysis

**Embedding Detection**:
```bash
# Extract all metadata
exiftool suspicious_image.jpg

# Search for suspicious comments
exiftool suspicious_image.jpg | grep -i comment

# Look for non-standard metadata fields
exiftool -s suspicious_image.jpg | grep -v "^(File|Image|EXIF)"
```

**Data Embedding Example**:
```bash
# Embed data in metadata
exiftool -Comment="Hidden message here" image.jpg

# Embed encoded data
echo "secret data" | base64 | xargs -I {} exiftool -Comment="{}" image.jpg
```

### Advanced Steganographic Analysis

#### Statistical Detection Methods

```bash
# File size analysis (compare similar images)
ls -la *.jpg | awk '{print $5, $9}' | sort -n

# Entropy analysis using binwalk
binwalk -E suspicious_image.jpg

# String analysis for embedded text
strings -n 8 suspicious_image.jpg | grep -v "^JFIF\|^Exif"
```

#### Multi-layered Concealment

**Complex Hiding Scenarios**:
1. **Base64-encoded data** in metadata comments
2. **Encrypted archives** embedded in images
3. **Multiple steganographic layers** (steghide within concatenated archives)
4. **Polyglot files** (valid in multiple formats)

---

## Comprehensive Analysis Workflow

### Investigation Methodology

#### Phase 1: Discovery
```bash
# Complete hidden file enumeration
find /home/suspect -name ".*" -type f > hidden_files.txt

# Command history analysis
cat /home/suspect/.bash_history > command_timeline.txt

# Recent file activity
find /home/suspect -type f -newermt "2023-01-01" -ls > recent_activity.txt
```

#### Phase 2: Steganographic Analysis
```bash
# Test all images for steganographic content
find /home/suspect -name "*.jpg" -o -name "*.png" | while read file; do
    echo "Testing: $file"
    steghide info "$file" 2>/dev/null
    exiftool "$file" | grep -i comment
done
```

#### Phase 3: Data Recovery
```bash
# Extract embedded archives
find /home/suspect -name "*.jpg" | while read file; do
    unzip "$file" -d extracted/ 2>/dev/null && echo "Archive found in: $file"
done

# Recover deleted files from trash
cp -r /home/suspect/.local/share/Trash/files/* recovered_trash/
```

### Evidence Documentation

#### Forensic Reporting Template

```bash
# Generate comprehensive report
echo "=== Hidden Files Report ===" > investigation_report.txt
find /home/suspect -name ".*" -type f -ls >> investigation_report.txt

echo "=== Command History Analysis ===" >> investigation_report.txt
wc -l /home/suspect/.bash_history >> investigation_report.txt
grep -E "(wget|curl|scp|ssh)" /home/suspect/.bash_history >> investigation_report.txt

echo "=== Steganographic Analysis ===" >> investigation_report.txt
find /home/suspect -name "*.jpg" -exec exiftool {} \; | grep -i comment >> investigation_report.txt
```

[⬆️ Back to Digital Forensics](./README.md)
