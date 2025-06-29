# 4.10 Windows Logon Events

Windows logon events provide crucial forensic evidence for user attribution and timeline reconstruction. These artifacts help investigators determine who accessed a system, when they logged in, and how long they remained active.

---

## Forensic Significance

Logon event analysis serves multiple investigative purposes:

- **User Attribution**: Link system activity to specific user accounts
- **Timeline Construction**: Establish precise windows of user presence
- **Attack Detection**: Identify brute force attempts and unauthorized access
- **Session Tracking**: Monitor user behavior patterns and anomalies

---

## Artifact Location

Windows Event Logs are stored at: `C:\Windows\System32\winevt\Logs\Security.evtx`

**Analysis Tools**: Windows Event Viewer, SIEM platforms, Microsoft Excel (via CSV export)

---

## Key Event IDs

| Event ID | Description                                                                                               |
| :------- | :-------------------------------------------------------------------------------------------------------- |
| **4624** | **Successful Logon:** An account successfully logged on. The `Logon Type` specifies how (e.g., Interactive, Network, Unlock). |
| **4672** | **Special Logon:** A logon by a user with administrative privileges.                                       |
| **4625** | **Failed Logon:** An account failed to log on. The `Status Code` gives the reason (e.g., bad password, disabled account). |
| **4634** | **Logoff:** An account logged off.                                                                          |

### Understanding Logon Types

Event ID 4624 includes a critical **Logon Type** field that indicates how the authentication occurred:

| Logon Type | Description | Use Case |
|------------|-------------|----------|
| **2** | Interactive | Physical logon to the device |
| **3** | Network | System accessed via network |
| **4** | Batch | Automated batch job execution |
| **5** | Service | Windows service started by service controller |
| **7** | Unlock | Workstation unlock (resuming previous session) |
| **8** | NetworkCleartext | Network logon with cleartext credentials |
| **9** | NewCredentials | RunAs with /netonly option |

### Failed Logon Error Codes (Event ID 4625)

Failed logon events contain status codes that reveal the specific reason for authentication failure:

| Error Code | Description | Forensic Significance |
|------------|-------------|----------------------|
| **0xC0000064** | User does not exist | Possible username enumeration |
| **0xC000006A** | Incorrect password | Password guessing/brute force |
| **0xC000006C** | Password policy not met | Policy violation attempt |
| **0xC000006D** | Invalid username | Malformed login attempt |
| **0xC000006E** | Account restrictions | Logon time/location restrictions |
| **0xC000006F** | Time restrictions | Outside allowed hours |
| **0xC0000070** | Workstation restrictions | Unauthorized source machine |
| **0xC0000071** | Password expired | Account maintenance needed |
| **0xC0000072** | Account disabled | Disabled account access attempt |
| **0xC0000234** | Account locked | Automated lockout triggered |

> **Investigation Tip**: Multiple 0xC0000072 (disabled account) or 0xC0000064 (non-existent user) attempts indicate suspicious activity.

### Session Correlation with Logon IDs

By correlating the `Logon ID` between logon (4624/4672) and logoff (4634) events, an investigator can determine the precise duration of a user session and track specific activities to authenticated users.

---

## Event Analysis Workflow

### Event ID 4624: Successful Logon Analysis

**Key Fields to Examine**:
- **Subject Information**: Account name and Security ID performing the logon
- **Logon Type**: Method of authentication (see table above)
- **Logon ID**: Unique session identifier for correlation
- **Timestamp**: Precise time of successful authentication

### Event ID 4672: Special Logon (Administrative)

**Significance**: Indicates privileged account access
- **Account Details**: Shows which administrative account logged in
- **Session Tracking**: Uses same Logon ID as corresponding 4624 event
- **Security Context**: Reveals elevated privilege usage

**Example Analysis**:
```
Subject:
    Account Name: admin@company.com
    Security ID: WORKSTATION\admin
    Logon ID: 0x25A036D

Logged: 2023-04-12 09:15:23
```

### Event ID 4625: Failed Logon Investigation

**Critical Analysis Points**:
- **Failure Reason**: Status code indicating specific failure type
- **Source Information**: Originating IP address and workstation
- **Account Target**: Which account was targeted
- **Pattern Recognition**: Frequency and timing of attempts

**Suspicious Patterns**:
- High volume of 0xC000006A (bad password) from single source
- Systematic attempts against disabled accounts (0xC0000072)
- Username enumeration via 0xC0000064 errors

### Event ID 4634: Logoff Correlation

**Session Closure Analysis**:
- **Logon ID Matching**: Links to corresponding logon event
- **Session Duration**: Calculate time between logon and logoff
- **Logoff Type**: Normal vs. forced disconnection

---

## Investigative Scenarios

### Scenario 1: Brute Force Attack Detection

**Pattern**: Multiple 4625 events with 0xC000006A status code
```
Timeline Analysis:
09:00:01 - Failed logon (0xC000006A) - user: admin
09:00:03 - Failed logon (0xC000006A) - user: admin  
09:00:05 - Failed logon (0xC000006A) - user: admin
09:00:15 - Account lockout (0xC0000234) - user: admin
```

### Scenario 2: Session Duration Analysis

**Objective**: Determine how long a user was active
```
Session Tracking:
4624 - Logon ID: 0x25A036D - 09:15:23 (Interactive logon)
4672 - Logon ID: 0x25A036D - 09:15:23 (Admin privileges)
4634 - Logon ID: 0x25A036D - 17:30:45 (Logoff)

Result: 8 hours, 15 minutes of active session
```

### Scenario 3: Unauthorized Access Investigation

**Focus**: Network logons outside business hours
```
Suspicious Activity:
4624 - Logon Type 3 (Network) - 02:30:15
Source: External IP address
Account: Service account (unexpected usage)
```

---

## Analysis Best Practices

### Timeline Construction
1. **Export events to CSV** for spreadsheet analysis
2. **Sort by timestamp** to establish chronological order
3. **Group by Logon ID** to track complete sessions
4. **Filter by account** to focus on specific users

### Correlation Techniques
- **Match Logon IDs** between 4624/4672 and 4634 events
- **Cross-reference with other logs** (application, system events)
- **Validate with network logs** for remote access verification
- **Check for concurrent sessions** indicating account sharing

### Red Flags to Investigate
- **Logons outside business hours** (especially administrative accounts)
- **Geographically impossible logons** (requires network correlation)
- **Service account interactive logons** (should be service-only)
- **Rapid logon/logoff cycles** (potential automation/scripting)

[⬆️ Back to Digital Forensics](./README.md)
