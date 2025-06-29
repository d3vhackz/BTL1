# 4.13: Linux User Account Artifacts

Understanding Linux user account structures is fundamental to forensic investigations on Unix-based systems. The `/etc/passwd` and `/etc/shadow` files provide critical information about user accounts, privileges, and potential unauthorized access.

---

## Core User Account Files

### /etc/passwd File

The `/etc/passwd` file maintains a registry of all user accounts with system access. This file is readable by all users but writable only by superusers.

**Location**: `/etc/passwd`
**Permissions**: World-readable, root-writable only

#### Forensic Significance
- **Complete user inventory** of all accounts on the system
- **Service account identification** for process attribution
- **Hidden account detection** disguised among legitimate service accounts
- **Privilege escalation evidence** through unauthorized account creation

#### File Structure Analysis
Each line represents one user account with seven colon-separated fields:
```
username:password:UID:GID:GECOS:home_directory:shell
```

| Field | Description | Forensic Value |
|-------|-------------|----------------|
| **Username** | Account identifier | Primary account tracking |
| **Password** | Usually 'x' (redirects to shadow) | Legacy password detection |
| **UID** | User ID number | Privilege level identification |
| **GID** | Primary group ID | Group membership analysis |
| **GECOS** | User information/comments | Real name and contact info |
| **Home Directory** | User's home folder path | File location mapping |
| **Shell** | Default command interpreter | Interactive vs. service accounts |

### /etc/shadow File

The `/etc/shadow` file stores encrypted passwords and account security policies, accessible only to root.

**Location**: `/etc/shadow`
**Permissions**: Root-readable only (prevents password hash extraction)

#### Security Architecture
- **Password hash storage** using secure encryption algorithms
- **Account expiration tracking** for temporal access control
- **Password aging policies** enforcement
- **Account lockout status** monitoring

#### File Structure
Each line contains nine colon-separated fields:
```
username:encrypted_password:last_change:min_age:max_age:warn:inactive:expire:reserved
```

---

## Investigative Analysis Techniques

### Account Enumeration Workflow

```bash
# Display all user accounts
cat /etc/passwd

# Focus on interactive users (UID >= 1000)
awk -F: '$3 >= 1000 {print $1 ":" $3 ":" $7}' /etc/passwd

# Identify accounts with shell access
grep -v '/nologin\|/false' /etc/passwd
```

### Suspicious Account Detection

#### Red Flags to Investigate
- **Unusual service accounts** with interactive shells
- **Recently created accounts** with high privileges
- **Accounts with UID 0** (root privileges) other than root
- **Disabled accounts** with recent activity
- **Generic usernames** that blend with system accounts

#### Account Analysis Commands
```bash
# Check for multiple root-level accounts (UID 0)
awk -F: '$3 == 0 {print $1}' /etc/passwd

# List accounts with bash shells (interactive capability)
grep '/bin/bash' /etc/passwd

# Examine shadow file for locked/expired accounts
sudo grep -E '^[^:]+:!' /etc/shadow
```

### Timeline Analysis

#### Last Password Changes
```bash
# View password change dates
sudo chage -l username

# Bulk analysis of password ages
sudo awk -F: '{print $1 ":" $3}' /etc/shadow | while IFS=: read user days; do
    if [ "$days" != "" ] && [ "$days" -gt 0 ]; then
        date -d "1970-01-01 + $days days" "+%Y-%m-%d $user"
    fi
done
```

---

## Advanced Forensic Techniques

### Group Membership Analysis

Understanding user group associations reveals privilege levels and access patterns:

```bash
# Display user's group memberships
groups username

# List all groups and members
getent group

# Find users in administrative groups
grep -E '^(sudo|wheel|admin):' /etc/group
```

### Cross-Reference Validation

#### Consistency Checks
- **Orphaned entries**: Users in passwd but not shadow (or vice versa)
- **Home directory existence**: Verify actual home folders exist
- **Shell validity**: Confirm assigned shells are legitimate executables

```bash
# Check for passwd entries without shadow counterparts
comm -23 <(awk -F: '{print $1}' /etc/passwd | sort) <(awk -F: '{print $1}' /etc/shadow | sort)

# Verify home directories exist
awk -F: '{print $6 ":" $1}' /etc/passwd | while IFS=: read home user; do
    [ ! -d "$home" ] && echo "Missing home: $user -> $home"
done
```

### Artifact Preservation

#### Evidence Collection
```bash
# Create forensic copies with metadata preservation
cp -p /etc/passwd /evidence/passwd_$(date +%Y%m%d_%H%M%S)
sudo cp -p /etc/shadow /evidence/shadow_$(date +%Y%m%d_%H%M%S)

# Generate hash values for integrity verification
sha256sum /etc/passwd /etc/shadow > /evidence/account_files.sha256
```

---

## Attack Pattern Recognition

### Common Threat Scenarios

| Scenario | Indicators | Investigation Focus |
|----------|------------|-------------------|
| **Backdoor Account** | New user with UID 0 or sudo access | Creation time, naming patterns |
| **Service Account Abuse** | Interactive shell on service account | Process attribution, login history |
| **Privilege Escalation** | Modified UID/GID values | File modification times, process logs |
| **Password Attacks** | Unusual shadow file access patterns | Authentication logs, failed attempts |

### Persistence Mechanisms

Attackers often modify account files for persistence:
- **Creating hidden administrative accounts** with innocuous names
- **Modifying existing service accounts** to allow interactive access
- **Manipulating password expiration** to maintain access
- **Adding backup authentication methods** through group modifications

---

## Integration with Other Artifacts

### Correlation Opportunities
- **Authentication logs** (`/var/log/auth.log`) for account usage verification
- **Process lists** for attribution of running processes to accounts
- **File ownership** analysis using account information
- **Network connections** mapped to user sessions

### Timeline Reconstruction
Combine account artifacts with:
- File system timestamps for account creation correlation
- Log entries for first/last usage patterns
- Process execution history for behavioral analysis
- Network activity logs for remote access patterns

[⬆️ Back to Digital Forensics](./README.md)
