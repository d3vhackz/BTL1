# 5.5: Windows Event Logging

Windows Event Logging provides comprehensive visibility into system activities, security events, and application behavior across Windows-based infrastructure. Understanding the Windows Event Log architecture, critical security events, and advanced monitoring capabilities is essential for effective SIEM implementation in enterprise environments.

---

## Windows Event Log Architecture

### Event Log System Evolution

#### Legacy Event Logging (Windows 2000 - Windows XP)

**File Structure and Location**:
- **Storage Path**: `%WinDir%\system32\Config\*.evt`
- **File Format**: Binary format with limited extensibility
- **Log Categories**: Application, System, Security (limited additional logs)
- **Size Limitations**: Fixed maximum file sizes with circular overwriting

**Architectural Constraints**:
- **Limited log sources**: Restricted to core system and application events
- **Basic filtering**: Minimal query and filtering capabilities
- **Performance issues**: Sequential read requirements for analysis
- **Scalability problems**: Poor performance with large log files

#### Modern Event Logging (Windows Vista+)

**Enhanced Architecture**:
- **Storage Path**: `%WinDir%\system32\WinEVT\Logs\*.evtx`
- **File Format**: XML-based structure with structured data elements
- **Expanded Categories**: Hundreds of specialized event logs for detailed monitoring
- **Advanced Features**: Filtering, forwarding, and subscription capabilities

**Architectural Improvements**:
- **Structured XML**: Consistent parsing and automated processing
- **Channel-based**: Isolated logs for different components and applications
- **Performance optimization**: Indexed access and efficient querying
- **Retention management**: Flexible archival and retention policies

### Event Log Categories and Structure

#### Core Event Log Categories

| Category | Purpose | Typical Events | Security Relevance |
|----------|---------|----------------|-------------------|
| **Application** | Application and service events | Program crashes, service starts/stops, errors | Application security monitoring, malware detection |
| **System** | Operating system events | Hardware events, driver loading, system startup | System integrity, unauthorized changes |
| **Security** | Security and audit events | Authentication, authorization, object access | Primary security monitoring and compliance |
| **Setup** | Installation and configuration | Software installation, updates, patches | Change management, unauthorized software |
| **Forwarded Events** | Remote event collection | Events from other systems | Centralized monitoring, distributed analysis |

#### Specialized Event Logs

**Directory Service Logs** (Domain Controllers):
- **Directory Service**: Active Directory operations and replication
- **DNS Server**: DNS query processing and security events
- **File Replication Service**: SYSVOL and database replication monitoring

**Application-Specific Logs**:
- **Windows PowerShell**: Script execution and command logging
- **Microsoft-Windows-Sysmon/Operational**: Enhanced system monitoring
- **Microsoft-Windows-TerminalServices**: Remote desktop and session management
- **Microsoft-Windows-WinRM/Operational**: Windows Remote Management activities

**Security-Enhanced Logs**:
- **Microsoft-Windows-AppLocker/EXE and DLL**: Application control events
- **Microsoft-Windows-CodeIntegrity/Operational**: Driver and code signing validation
- **Microsoft-Windows-Bits-Client/Operational**: Background transfer service monitoring

### Event Structure and Components

#### Windows Event Schema

```xml
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
  <System>
    <Provider Name="Microsoft-Windows-Security-Auditing" Guid="{54849625-5478-4994-A5BA-3E3B0328C30D}"/>
    <EventID>4624</EventID>
    <Version>2</Version>
    <Level>0</Level>
    <Task>12544</Task>
    <Opcode>0</Opcode>
    <Keywords>0x8020000000000000</Keywords>
    <TimeCreated SystemTime="2023-10-15T09:30:45.123456Z"/>
    <EventRecordID>1234567</EventRecordID>
    <Correlation/>
    <Execution ProcessID="664" ThreadID="123"/>
    <Channel>Security</Channel>
    <Computer>DC01.corp.com</Computer>
    <Security/>
  </System>
  <EventData>
    <Data Name="SubjectUserSid">S-1-5-21-1234567890-123456789-123456789-1001</Data>
    <Data Name="SubjectUserName">admin</Data>
    <Data Name="SubjectDomainName">CORP</Data>
    <Data Name="SubjectLogonId">0x25a04d</Data>
    <Data Name="TargetUserSid">S-1-5-21-1234567890-123456789-123456789-1001</Data>
    <Data Name="TargetUserName">admin</Data>
    <Data Name="TargetDomainName">CORP</Data>
    <Data Name="TargetLogonId">0x25a04e</Data>
    <Data Name="LogonType">2</Data>
    <Data Name="LogonProcessName">User32</Data>
    <Data Name="AuthenticationPackageName">Negotiate</Data>
    <Data Name="WorkstationName">WORKSTATION01</Data>
    <Data Name="LogonGuid">{12345678-1234-1234-1234-123456789012}</Data>
    <Data Name="TransmittedServices">-</Data>
    <Data Name="LmPackageName">-</Data>
    <Data Name="KeyLength">0</Data>
    <Data Name="ProcessId">0x29c</Data>
    <Data Name="ProcessName">C:\Windows\System32\winlogon.exe</Data>
    <Data Name="IpAddress">192.168.1.100</Data>
    <Data Name="IpPort">52341</Data>
  </EventData>
</Event>
```

#### Event Component Analysis

**System Section**:
- **Provider**: Source application or service generating the event
- **EventID**: Unique identifier for event type and meaning
- **Level**: Severity level (Information, Warning, Error, Critical)
- **TimeCreated**: Precise timestamp with microsecond precision
- **Computer**: Hostname of the system generating the event

**EventData Section**:
- **Structured Fields**: Named data elements specific to event type
- **Subject Information**: User or service account context performing action
- **Target Information**: Object or resource being accessed or modified
- **Process Context**: Application or service responsible for the activity
- **Network Context**: Source IP address and port information when applicable

---

## Critical Security Events Analysis

### Authentication and Access Control Events

#### Event ID 4624 - Successful Logon

**Event Significance**: Records successful user authentication to Windows systems

**Critical Data Fields**:
- **LogonType**: Method of authentication (see table below)
- **TargetUserName**: Account that successfully authenticated
- **WorkstationName**: Source computer name for network logons
- **IpAddress**: Source IP address for network-based authentication
- **LogonProcessName**: Authentication subsystem used

**Logon Type Classification**:

| Type | Description | Security Implications | Investigation Focus |
|------|-------------|----------------------|-------------------|
| **2** | Interactive | Direct console/keyboard logon | Physical access, stolen credentials |
| **3** | Network | Network-based authentication | Lateral movement, remote access |
| **4** | Batch | Scheduled task execution | Persistence mechanisms, unauthorized tasks |
| **5** | Service | Windows service startup | Service account abuse, persistence |
| **7** | Unlock | Workstation unlock operation | Session hijacking, screen lock bypass |
| **8** | NetworkCleartext | Network logon with cleartext password | Protocol security, credential exposure |
| **9** | NewCredentials | RunAs with alternate credentials | Privilege escalation, credential misuse |
| **10** | RemoteInteractive | Terminal Services/RDP logon | Remote access, administrative activity |
| **11** | CachedInteractive | Cached credential logon | Offline authentication, cached credential theft |

#### Event ID 4625 - Failed Logon

**Event Significance**: Records unsuccessful authentication attempts

**Status Code Analysis**:

| Status Code | Description | Attack Indication |
|-------------|-------------|------------------|
| **0xC0000064** | User name does not exist | Username enumeration attempts |
| **0xC000006A** | Incorrect password | Password guessing, brute force attacks |
| **0xC000006C** | Password policy not met | Password complexity bypass attempts |
| **0xC000006D** | Invalid user name | Malformed authentication requests |
| **0xC000006E** | Account restriction violation | Time/location-based access violations |
| **0xC000006F** | Time restriction violation | After-hours access attempts |
| **0xC0000070** | Workstation restriction violation | Unauthorized source computer access |
| **0xC0000071** | Password expired | Account maintenance needed |
| **0xC0000072** | Account disabled | Disabled account access attempts |
| **0xC0000234** | Account locked out | Automated lockout from failed attempts |

#### Event ID 4672 - Special Privileges Assigned

**Event Significance**: Records when administrative privileges are assigned during logon

**Critical Privileges**:
- **SeDebugPrivilege**: Debug programs and access process memory
- **SeBackupPrivilege**: Backup files and directories (bypass file permissions)
- **SeRestorePrivilege**: Restore files and directories (bypass file permissions)
- **SeTakeOwnershipPrivilege**: Take ownership of files and objects
- **SeLoadDriverPrivilege**: Load and unload device drivers

### Account Management Events

#### Event ID 4720 - User Account Created

**Monitoring Focus**:
- **Account creation timing**: Creation outside normal business hours
- **Account naming patterns**: Service account-like names for user accounts
- **Privilege assignment**: Immediate administrative privilege grant
- **Creator context**: Which administrative account created the new user

#### Event ID 4728 - Member Added to Security-Enabled Global Group

**Critical Group Monitoring**:
- **Domain Admins**: Complete enterprise administrative access
- **Enterprise Admins**: Cross-domain administrative privileges
- **Schema Admins**: Active Directory schema modification rights
- **Backup Operators**: Backup and restore privileges across systems

#### Event ID 4738 - User Account Changed

**Change Monitoring**:
- **Password reset**: Administrative password changes for accounts
- **Account enablement**: Previously disabled accounts being reactivated
- **Group membership**: Addition to privileged groups
- **User rights assignment**: Direct privilege grant to user accounts

### Object Access and File System Events

#### Event ID 4663 - Object Access Attempted

**Prerequisites**: Requires object-level auditing configuration

**Monitoring Scenarios**:
- **Sensitive file access**: Access to confidential or classified documents
- **System file modification**: Changes to critical system configuration files
- **Database access**: Queries against sensitive database tables
- **Registry modification**: Changes to security-relevant registry keys

#### Event ID 4656 - Handle to Object Requested

**Access Request Analysis**:
- **Desired Access**: Specific permissions requested (read, write, delete)
- **Access Mask**: Hexadecimal representation of requested permissions
- **Process Information**: Application requesting access to the object
- **Subject Information**: User account context for the access request

### Process Creation and Execution Events

#### Event ID 4688 - Process Created

**Enhanced Auditing Required**: Command line auditing must be enabled

**Critical Monitoring Fields**:
- **NewProcessName**: Full path to executed process
- **CommandLine**: Complete command line with arguments and parameters
- **ParentProcessName**: Parent process that spawned the new process
- **SubjectUserName**: User account context for process execution

**Suspicious Pattern Detection**:
```
Common Attack Patterns:
├── PowerShell execution: powershell.exe -ExecutionPolicy Bypass -Command ...
├── Command line obfuscation: cmd.exe /c "echo [encoded_command] | base64 -d | bash"
├── Living-off-the-land: legitimate tools used maliciously (certutil, bitsadmin)
├── Process injection: rundll32.exe executing suspicious DLLs
└── Lateral movement: PsExec, WMI, or PowerShell remoting execution
```

---

## Event Viewer and Analysis Tools

### Windows Event Viewer Interface

#### Event Viewer Navigation

**Standard Log Categories**:
- **Windows Logs**: Core system logs (Application, Security, System, Setup)
- **Applications and Services Logs**: Application-specific and feature logs
- **Subscriptions**: Centralized collection from remote systems
- **Custom Views**: User-defined filters for specific investigation needs

#### Event Filtering and Search

**Filter Creation Interface**:
```
Filter Options:
├── Time Range: Custom date/time selection for focused analysis
├── Event Level: Critical, Error, Warning, Information, Verbose
├── Event Sources: Specific applications or services
├── Event IDs: Include or exclude specific event types
├── User: Filter by specific user account
├── Computer: Filter by specific computer name
└── Keywords: Text-based search within event content
```

### Custom Views for Security Analysis

#### Authentication Monitoring View

**Configuration**:
- **Event IDs**: 4624, 4625, 4634, 4647, 4672, 4648
- **Time Range**: Last 24 hours for operational monitoring
- **Event Level**: All levels for comprehensive coverage
- **Purpose**: Monitor all authentication-related activities

**Query Example**:
```xml
<QueryList>
  <Query Id="0" Path="Security">
    <Select Path="Security">
      *[System[(EventID=4624 or EventID=4625 or EventID=4634 or EventID=4672)]]
    </Select>
  </Query>
</QueryList>
```

#### Privilege Escalation Detection View

**Configuration**:
- **Event IDs**: 4672, 4673, 4674, 4728, 4732, 4756
- **Focus**: Administrative privilege usage and group membership changes
- **Alerting**: Immediate notification for unexpected privilege assignment

#### Logon Session Analysis

**Session Correlation**:
- **Logon ID tracking**: Correlate logon (4624) with logoff (4634) events
- **Session duration**: Calculate active session times for behavior analysis
- **Concurrent sessions**: Identify multiple simultaneous sessions for single accounts
- **Geographic analysis**: Detect impossible travel scenarios

### Advanced Event Analysis Techniques

#### PowerShell for Event Log Analysis

**Mass Event Processing**:
```powershell
# Failed logon analysis
Get-WinEvent -FilterHashtable @{LogName='Security';ID=4625;StartTime=(Get-Date).AddDays(-1)} | 
  ForEach-Object {
    $xml = [xml]$_.ToXml()
    [PSCustomObject]@{
      Time = $_.TimeCreated
      Account = $xml.Event.EventData.Data | Where {$_.Name -eq 'TargetUserName'} | Select -ExpandProperty '#text'
      SourceIP = $xml.Event.EventData.Data | Where {$_.Name -eq 'IpAddress'} | Select -ExpandProperty '#text'
      Reason = $xml.Event.EventData.Data | Where {$_.Name -eq 'Status'} | Select -ExpandProperty '#text'
    }
  } | Group-Object Account | Sort Count -Descending

# Process execution monitoring
Get-WinEvent -FilterHashtable @{LogName='Security';ID=4688;StartTime=(Get-Date).AddHours(-1)} |
  ForEach-Object {
    $xml = [xml]$_.ToXml()
    [PSCustomObject]@{
      Time = $_.TimeCreated
      Process = $xml.Event.EventData.Data | Where {$_.Name -eq 'NewProcessName'} | Select -ExpandProperty '#text'
      CommandLine = $xml.Event.EventData.Data | Where {$_.Name -eq 'CommandLine'} | Select -ExpandProperty '#text'
      User = $xml.Event.EventData.Data | Where {$_.Name -eq 'SubjectUserName'} | Select -ExpandProperty '#text'
    }
  } | Where {$_.CommandLine -match "powershell|cmd|wmic"}
```

#### Event Correlation Strategies

**Cross-Event Analysis**:
- **Authentication correlation**: Link successful logons with subsequent activities
- **Process tree reconstruction**: Build parent-child process relationships
- **Network session mapping**: Correlate network logons with file access patterns
- **Time-based clustering**: Group related activities within temporal windows

---

## Advanced Windows Logging with Sysmon

### System Monitor (Sysmon) Overview

#### Sysmon Architecture and Benefits

**Enhanced Monitoring Capabilities**:
- **Process creation**: Detailed process execution with command lines and hashes
- **Network connections**: Process-level network activity monitoring
- **File creation time changes**: Detection of timestamp manipulation
- **Driver/DLL loading**: Monitoring of code injection and process hollowing
- **WMI activity**: Windows Management Instrumentation event monitoring

**Installation and Configuration**:
```cmd
# Download from Microsoft Sysinternals
# Basic installation
sysmon -accepteula -i

# Installation with configuration file
sysmon -accepteula -i sysmonconfig.xml

# Configuration update
sysmon -c sysmonconfig.xml
```

#### Critical Sysmon Event Types

**Event ID 1 - Process Creation**:
```xml
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
  <EventData>
    <Data Name="RuleName">-</Data>
    <Data Name="UtcTime">2023-10-15 09:30:45.123</Data>
    <Data Name="ProcessGuid">{12345678-1234-1234-1234-123456789012}</Data>
    <Data Name="ProcessId">1234</Data>
    <Data Name="Image">C:\Windows\System32\cmd.exe</Data>
    <Data Name="FileVersion">10.0.19041.1 (WinBuild.160101.0800)</Data>
    <Data Name="Description">Windows Command Processor</Data>
    <Data Name="Product">Microsoft® Windows® Operating System</Data>
    <Data Name="Company">Microsoft Corporation</Data>
    <Data Name="OriginalFileName">Cmd.Exe</Data>
    <Data Name="CommandLine">cmd.exe /c "whoami"</Data>
    <Data Name="CurrentDirectory">C:\Users\admin\</Data>
    <Data Name="User">CORP\admin</Data>
    <Data Name="LogonGuid">{12345678-1234-1234-1234-123456789012}</Data>
    <Data Name="LogonId">0x25a04e</Data>
    <Data Name="TerminalSessionId">1</Data>
    <Data Name="IntegrityLevel">High</Data>
    <Data Name="Hashes">MD5=5B6CE5B3025C74F7F8F73B0C8C4A3E1F,SHA256=7A8E7B...</Data>
    <Data Name="ParentProcessGuid">{12345678-1234-1234-1234-123456789012}</Data>
    <Data Name="ParentProcessId">2345</Data>
    <Data Name="ParentImage">C:\Windows\System32\powershell.exe</Data>
    <Data Name="ParentCommandLine">powershell.exe -ExecutionPolicy Bypass</Data>
  </EventData>
</Event>
```

**Event ID 3 - Network Connection**:
- **Process attribution**: Which process initiated the network connection
- **Connection details**: Source/destination IP addresses and ports
- **Protocol information**: TCP/UDP protocol and connection state
- **Timing correlation**: Network activity timing relative to process execution

**Event ID 7 - Image/DLL Loaded**:
- **DLL injection detection**: Unusual library loading patterns
- **Code integrity**: Digital signature validation for loaded images
- **Process hollowing**: Detection of image replacement techniques
- **Malware analysis**: Identification of malicious library dependencies

### Sysmon Configuration Management

#### Configuration Best Practices

**SwiftOnSecurity Configuration**:
```xml
<!-- High-quality, community-maintained configuration -->
<Sysmon schemaversion="4.30">
  <EventFiltering>
    <!-- Process creation monitoring -->
    <ProcessCreate onmatch="exclude">
      <Image condition="is">C:\Windows\System32\conhost.exe</Image>
      <Image condition="is">C:\Windows\System32\wermgr.exe</Image>
    </ProcessCreate>
    
    <!-- Network connection monitoring -->
    <NetworkConnect onmatch="exclude">
      <Image condition="is">C:\Program Files\Windows Defender\MsMpEng.exe</Image>
      <DestinationPort condition="is">443</DestinationPort>
    </NetworkConnect>
    
    <!-- File creation monitoring -->
    <FileCreate onmatch="include">
      <TargetFilename condition="contains">startup</TargetFilename>
      <TargetFilename condition="contains">temp</TargetFilename>
    </FileCreate>
  </EventFiltering>
</Sysmon>
```

**Configuration Principles**:
- **High-fidelity logging**: Focus on security-relevant events while excluding noise
- **Performance balance**: Optimize for minimal system impact while maintaining coverage
- **Environment-specific**: Customize for organizational applications and processes
- **Regular updates**: Keep configuration current with emerging threats and techniques

#### Event Volume Management

**Filtering Strategies**:
- **Include approach**: Explicitly define what to monitor (high maintenance)
- **Exclude approach**: Monitor everything except known-good activities (recommended)
- **Hybrid approach**: Combination of include/exclude for different event types

**Performance Optimization**:
- **Rule ordering**: Place most common exclusions first for efficiency
- **Regular expression efficiency**: Optimize regex patterns for performance
- **Process exclusions**: Exclude known-good system processes generating high volumes
- **Network filtering**: Focus on suspicious protocols and destination ranges

---

## Windows Event Log Integration with SIEM

### Event Forwarding and Collection

#### Windows Event Forwarding (WEF)

**Architecture Components**:
- **Event Collector**: Central server receiving forwarded events
- **Event Sources**: Windows systems configured to forward events
- **Subscriptions**: Configuration defining which events to forward
- **Collection Transport**: Encrypted HTTPS transmission with authentication

**Subscription Configuration**:
```xml
<Subscription xmlns="http://schemas.microsoft.com/2006/03/windows/events/subscription">
  <SubscriptionId>Security-Events</SubscriptionId>
  <SubscriptionType>SourceInitiated</SubscriptionType>
  <Description>Security events from domain controllers</Description>
  <Enabled>true</Enabled>
  <Uri>http://schemas.microsoft.com/wbem/wsman/1/windows/EventLog</Uri>
  <ConfigurationMode>Custom</ConfigurationMode>
  <Delivery Mode="Push">
    <Batching>
      <MaxItems>5</MaxItems>
      <MaxLatencyTime>15000</MaxLatencyTime>
    </Batching>
    <PushSettings>
      <Heartbeat Interval="900000"/>
    </PushSettings>
  </Delivery>
  <Query>
    <![CDATA[
    <QueryList>
      <Query Id="0">
        <Select Path="Security">*[System[(EventID=4624 or EventID=4625 or EventID=4672)]]</Select>
      </Query>
    </QueryList>
    ]]>
  </Query>
</Subscription>
```

#### Agent-Based Collection

**SIEM Agent Deployment**:
- **Universal forwarders**: Lightweight agents for log collection and forwarding
- **Local processing**: Event filtering and enrichment at source systems
- **Encryption and compression**: Secure and efficient transmission to SIEM
- **Buffering and retry**: Reliable delivery during network outages

**Agent Configuration Benefits**:
- **Selective collection**: Precise control over which events are transmitted
- **Local enrichment**: Addition of context information at the source
- **Bandwidth optimization**: Compression and deduplication before transmission
- **Security enhancement**: Encrypted transmission and agent authentication

### Event Processing and Normalization

#### Common Event Format (CEF) Translation

**Windows Security Event to CEF**:
```
Original Event: 4624 Successful Logon
CEF Translation: CEF:0|Microsoft|Windows|6.1|4624|Successful Logon|Low|src=192.168.1.100 duser=CORP\\admin dst=192.168.1.50 dhost=DC01
```

**Field Mapping Strategy**:
- **Source IP**: Map IpAddress field to CEF src field
- **User Context**: Map TargetUserName to CEF duser field
- **Destination**: Map Computer field to CEF dst field
- **Event Classification**: Map EventID to CEF signatureId field

#### SIEM-Specific Integration

**Splunk Integration**:
```conf
# inputs.conf
[WinEventLog://Security]
disabled = false
start_from = oldest
current_only = false
checkpointInterval = 5
index = windows_security

# props.conf
[WinEventLog:Security]
SHOULD_LINEMERGE = false
TRUNCATE = 100000
KV_MODE = xml
```

**QRadar Integration**:
- **Device Support Module (DSM)**: Pre-built parser for Windows events
- **Custom properties**: Extended field extraction for specific use cases
- **Reference data**: Integration with asset and user context information
- **Automatic categorization**: Rule-based event classification and normalization

[⬆️ Back to SIEM & Monitoring](./README.md)
