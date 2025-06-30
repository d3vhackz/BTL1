# 4.20: Autopsy Practical Investigation Workflow

This section provides a comprehensive walkthrough of conducting digital forensic investigations using Autopsy, from case initialization through evidence analysis and extraction.

---

## Case Initialization Process

### Creating a New Investigation

#### Step 1: Case Setup
```
1. Launch Autopsy application
2. Click "New Case" from main interface
3. Provide case metadata:
   - Case Name: [Descriptive identifier]
   - Base Directory: [Evidence storage location]
   - Case Number: [Reference identifier]
```

#### Step 2: Investigation Metadata
Optional but recommended for professional investigations:

| Field | Purpose | Example |
|-------|---------|---------|
| **Case Number** | Official case tracking | 2024-CYBER-001 |
| **Examiner** | Primary investigator name | Jane Smith, GCFA |
| **Case Type** | Investigation category | Employee Misconduct |
| **Description** | Case summary | Suspected data theft investigation |

### Data Source Integration

#### Supported Image Formats

| Format | Extension | Use Case |
|--------|-----------|----------|
| **Expert Witness** | .E01, .Ex01 | Professional forensic imaging |
| **Raw Images** | .img, .dd | Basic disk duplication |
| **Virtual Machines** | .vmdk, .vdi | Virtualized environment analysis |
| **Live Systems** | N/A | Real-time investigation |

#### Import Workflow
```
1. Select "Disk Image or VM File" as data source type
2. Browse to disk image location (.E01 files)
3. Verify image integrity (automatic hash validation)
4. Configure ingest module selection
5. Initiate automated analysis process
```

---

## Ingest Module Configuration

### Essential Module Selection

For comprehensive analysis, select these critical ingest modules:

#### Core Analysis Modules

| Module | Function | Analysis Output |
|--------|----------|-----------------|
| **Recent Activity** | User behavior reconstruction | Recently accessed files, programs |
| **File Type Identification** | Signature-based file analysis | File classification, extension mismatches |
| **Embedded File Extractor** | Archive and container analysis | Hidden files, compressed content |
| **EXIF Parser** | Image metadata extraction | GPS coordinates, camera details |
| **Email Parser** | Communication evidence | Email messages, attachments |
| **Encryption Detection** | Security measure identification | Encrypted files, hidden volumes |

#### Advanced Modules (Optional)
- **Hash Database Lookup** - Known file identification
- **Keyword Search** - Content-based evidence discovery
- **Web Artifacts** - Browser activity reconstruction
- **Android Analyzer** - Mobile device investigation

### Module Execution Monitoring

#### Progress Tracking
- **Real-time progress bar** in bottom-right interface
- **Module completion notifications** for each analysis type
- **Artifact tree population** showing discovered evidence
- **Error logging** for failed analysis attempts

---

## Evidence Analysis Methodology

### Partition and Volume Analysis

#### Understanding Disk Structure

Navigate to **Data Sources** → **[Image Name]** to examine disk layout:

```
Disk Image Structure:
├── vol1 (Boot Partition)
├── vol2 (Primary NTFS - Main OS)
└── vol3 (Recovery Partition)
```

#### Partition Table Analysis

**Key Information Available**:
- **File System Type** (NTFS, FAT32, ext4)
- **Sector Start Location** (physical disk positioning)
- **Partition Length** (storage capacity)
- **Allocated vs. Unallocated** space distribution

**Forensic Significance**:
- **vol2** typically contains user data and system files
- **Unallocated space** may contain deleted file remnants
- **Boot sectors** provide system configuration evidence

### File System Navigation

#### Manual File Exploration

**Navigation Method**:
1. Expand **Data Sources** → **[Image Name]** → **vol2**
2. Browse directory structure similar to Windows Explorer
3. Examine user profiles: `C:\Users\[Username]\`
4. Investigate common evidence locations:
   - Desktop files and shortcuts
   - Downloads folder contents
   - Documents and personal files
   - Application data directories

#### Read-Only Access Benefits
- **Evidence preservation** through non-destructive viewing
- **Original timestamp** maintenance
- **File structure integrity** protection
- **Chain of custody** compliance

---

## Artifact Analysis Categories

### Web Activity Investigation

#### Browser History Analysis

**Location**: Results → Web History

**Available Information**:
- **Visited URLs** with complete web addresses
- **Access timestamps** (precise visit times)
- **Page titles** when available
- **Browser identification** (Chrome, Firefox, Edge)
- **Visit frequency** and duration patterns

**Investigative Applications**:
```
Example Analysis:
URL: 4chan.org/rules
Timestamp: 2013-12-18 02:35 AM GMT
Browser: Chrome
Significance: Unusual hours internet activity
```

#### Web Artifact Correlation
- **Download history** matched with file system evidence
- **Search queries** revealing user intent
- **Social media activity** patterns
- **Suspicious website** access patterns

### Deleted File Recovery

#### Recycle Bin Analysis

**Location**: Results → Recycle Bin

**Evidence Available**:
- **Original file paths** before deletion
- **Deletion timestamps** (precise timing)
- **User attribution** (who deleted the file)
- **File size** and type information

#### File Recovery Process
```
Recovery Workflow:
1. Right-click target file in Recycle Bin view
2. Select "Extract File(s)" from context menu
3. Choose extraction destination directory
4. Verify file integrity after extraction
5. Document chain of custody for recovered evidence
```

#### Practical Example
```
Recovered Evidence:
File: "Underage_lolita_r@ygold_001.jpg"
Original Path: C:\Users\Craig\Desktop\
Deleted: 2013-12-15 14:23:17 GMT
Examiner Note: Suspicious filename requiring analysis
```

### Installed Software Analysis

#### Program Inventory Assessment

**Location**: Results → Installed Programs

**Analysis Focus**:
- **Installation timestamps** for timeline correlation
- **Suspicious software** identification (hacking tools, P2P clients)
- **Security software** presence or absence
- **Version information** for vulnerability assessment

**Common Findings**:
```
Relevant Software Examples:
- WinRAR/7-Zip: File compression/extraction capability
- GIMP: Image editing for potential evidence manipulation
- BitTorrent clients: File sharing activity
- VPN software: Network anonymization tools
- Forensic tools: Counter-forensic awareness
```

### Communication Evidence

#### Email Investigation

**Location**: Results → Accounts → Email

**Evidence Types**:
- **Downloaded email files** from client applications
- **Message metadata** (sender, recipient, timestamps)
- **Attachment files** potentially containing malware
- **Email routing information** for attribution

#### Email Analysis Workflow
```
1. Identify email files in Results tree
2. Right-click and extract individual messages
3. Open with email client (Thunderbird, Outlook)
4. Alternative: Text editor for raw header analysis
5. Document communication patterns and suspicious content
```

**Forensic Intelligence Extraction**:
- **Sender/recipient** identification
- **Communication timeline** establishment
- **Attachment analysis** for malware indicators
- **Email server** routing and authentication data

---

## Advanced Analysis Techniques

### Cross-Artifact Correlation

#### Timeline Integration
- **File system** timestamps with web activity
- **Email communication** timing with file downloads
- **Software installation** dates with suspicious activity
- **User logon** events with evidence creation

#### Pattern Recognition
- **Activity clustering** around specific timeframes
- **Behavioral anomalies** indicating suspicious activity
- **Data exfiltration** patterns through multiple evidence types
- **Anti-forensic** tool usage detection

### Evidence Tagging and Organization

#### Systematic Evidence Management

**Tagging Strategy**:
```
Tag Categories:
- "Suspicious" - Requires further investigation
- "Contraband" - Illegal or policy-violating content
- "Bookmark" - Important reference material
- "Malware" - Security threat indicators
- "Timeline" - Chronologically significant events
```

#### Documentation Standards
- **Descriptive comments** for each tagged item
- **Chain of custody** tracking for extracted files
- **Analysis notes** documenting investigative reasoning
- **Evidence correlation** references between artifacts

---

## Professional Investigation Practices

### Evidence Preservation

#### Export and Documentation
```
Professional Workflow:
1. Tag all relevant evidence with descriptive labels
2. Add detailed comments explaining significance
3. Extract files using right-click → "Extract File(s)"
4. Maintain extraction logs with timestamps
5. Calculate hash values for extracted evidence
6. Document analysis methodology and findings
```

### Case Reporting Integration

#### Comprehensive Documentation
- **Executive summary** of key findings
- **Timeline reconstruction** of critical events
- **Evidence catalog** with tagged items
- **Technical appendix** with detailed analysis
- **Chain of custody** documentation

#### Legal Preparation
- **Evidence authentication** through hash verification
- **Methodology documentation** for court testimony
- **Expert opinion** formation based on findings
- **Cross-examination preparation** materials

[⬆️ Back to Digital Forensics](./README.md)
