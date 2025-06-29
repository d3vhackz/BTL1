# 4.12: Windows Recycle Bin Artifacts

The Windows Recycle Bin serves as a temporary storage location for deleted files before permanent removal. For forensic investigators, it represents a critical source of evidence about user deletion behavior and potential data destruction attempts.

---

## Forensic Significance

The Recycle Bin provides multiple investigative opportunities:

### Evidence Recovery
- **Recently deleted files** can be easily restored and analyzed
- **File metadata preservation** maintains original timestamps and locations
- **User intent analysis** through examination of what was deliberately deleted

### Timeline Reconstruction
- **Deletion timestamps** help establish sequence of events
- **File path information** reveals original storage locations
- **User attribution** through SID-based folder organization

### Data Carving Opportunities
- Even after emptying, **underlying data may persist** on storage media
- **File remnants** can be recovered through specialized techniques
- **Metadata artifacts** survive even when file content is overwritten

---

## Artifact Structure and Location

### Directory Location
```
C:\$Recycle.Bin\
├── [SID-1001]\      # User 1's deleted files
├── [SID-1002]\      # User 2's deleted files
└── [SID-XXXX]\      # Additional user folders
```

### File Naming Convention (Windows Vista+)

Each deleted file generates **two companion files**:

| File Prefix | Contents | Purpose |
|-------------|----------|---------|
| **$R** + random string | Original file content | Complete data preservation |
| **$I** + same string | File metadata | Original path, size, deletion timestamp |

**Example Pair**:
- `$R1UOZ51.xlsx` ← Actual Excel file content
- `$I1UOZ51.xlsx` ← Metadata about the Excel file

---

## Analysis Methodology

### Tool Requirements
- **Command Prompt (CMD)** - Basic directory navigation
- **RBCmd** - Recycle Bin artifact parser by Eric Zimmerman
- **CSV Analysis Tool** - For bulk data examination

### Step 1: Identify User Accounts

**Challenge**: Recycle Bin folders use Security Identifiers (SIDs) instead of usernames.

**Solution**: Map SIDs to usernames using WMIC:
```cmd
wmic useraccount get name,SID
```

**Output Example**:
```
Name          SID
Administrator S-1-5-21-XXXXXXXX-XXXXXXXX-XXXXXXXX-500
Guest         S-1-5-21-XXXXXXXX-XXXXXXXX-XXXXXXXX-501
Simon.Leeves  S-1-5-21-XXXXXXXX-XXXXXXXX-XXXXXXXX-1010
```

### Step 2: Navigate to Target User's Folder

```cmd
# Show hidden folders
dir /a C:\$Recycle.Bin

# Navigate to specific user (use Tab completion)
cd C:\$Recycle.Bin\S-1-5-21-XXXXXXXX-XXXXXXXX-XXXXXXXX-1010

# List hidden contents
dir /a
```

### Step 3: Analyze Individual Files with RBCmd

**Single File Analysis**:
```cmd
RBCmd.exe -f $I1UOZ51.xlsx
```

**Sample Output**:
```
Version: 2
File size: 1,874,635 bytes
Deleted on: 2023-04-12 13:18:45
Original path: C:\Users\Simon.Leeves\Downloads\DU Financials 2022.xlsx
```

### Step 4: Bulk Analysis and Export

**Comprehensive Directory Analysis**:
```cmd
RBCmd.exe -d . --csv "C:\Analysis\RBCmdOutput"
```

**Benefits of CSV Export**:
- Sortable by deletion date
- Filterable by file type
- Easy bulk analysis of deletion patterns
- Timeline creation for investigative reports

---

## Advanced Analysis Techniques

### Multi-User Investigation

For system-wide deleted file analysis:
```cmd
# From C:\$Recycle.Bin directory
RBCmd.exe -d . --csv "C:\Analysis\SystemWide_DeletedFiles"
```

**Capabilities**:
- **Recursive processing** through all user SID folders
- **Consolidated timeline** of all deletion activity
- **Cross-user pattern analysis** for coordinated data destruction

### Metadata Analysis Focus

**Critical Metadata Fields**:
- **Original File Path** - Reveals file organization and purpose
- **File Size** - Indicates importance (large files often significant)
- **Deletion Timestamp** - Correlates with incident timelines
- **File Extension** - Identifies file types of interest

### Investigative Scenarios

| Scenario | Focus Areas | Key Indicators |
|----------|-------------|----------------|
| **Data Theft** | Large files, external drives, compression | High-value document deletion after access |
| **Evidence Destruction** | System files, logs, browser data | Systematic deletion of forensic artifacts |
| **Malware Cleanup** | Temporary files, downloads | Deletion of suspicious executables |
| **Policy Violation** | Personal files, unauthorized software | Non-business file deletion patterns |

---

## Limitations and Considerations

### When Recycle Bin Analysis Fails
- **Emptied Recycle Bin** - Metadata files are removed
- **Direct deletion** - Shift+Delete bypasses Recycle Bin
- **Third-party deletion tools** - May not use standard Windows deletion
- **Storage space limits** - Very large files may be permanently deleted

### Recovery Alternatives
If Recycle Bin is empty, consider:
- **File carving** from unallocated disk space
- **Shadow copy analysis** for previous versions
- **File system journal examination** for deletion records
- **Memory analysis** for recently deleted file traces

---

## Practical Investigation Example

**Case**: Employee suspected of deleting financial documents before termination

1. **Map user account**: `wmic useraccount get name,SID`
2. **Navigate to SID folder**: `cd C:\$Recycle.Bin\[USER-SID]`
3. **Generate comprehensive report**: `RBCmd.exe -d . --csv report.csv`
4. **Analyze patterns**: Look for systematic deletion of financial files
5. **Build timeline**: Correlate deletion times with employee access logs
6. **Document findings**: Include original paths and file sizes in report

**Expected Evidence**:
- Multiple Excel/PDF financial documents
- Deletion timestamps clustered around termination period
- High-value files (indicated by large file sizes)
- Evidence of intentional data destruction

[⬆️ Back to Digital Forensics](./README.md)
