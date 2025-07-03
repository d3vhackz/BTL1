# 5.2: Logging and Data Aggregation

Effective SIEM implementation depends on comprehensive log collection and intelligent aggregation strategies. Understanding different logging protocols, data sources, and aggregation methods is fundamental to building a robust security monitoring capability.

---

## Fundamentals of Security Logging

### What Constitutes a Security Event

**Security events** are any activities within an IT environment that have potential security implications. Every user action, system operation, and network communication generates events that should be logged for security analysis.

#### Critical Event Categories

| Event Type | Examples | Security Significance |
|------------|----------|----------------------|
| **Authentication Events** | Login attempts, password changes, privilege escalation | Account compromise detection |
| **Network Activity** | Connection attempts, firewall denies, port scans | Network intrusion identification |
| **File Operations** | File access, modification, deletion | Data theft and integrity monitoring |
| **System Changes** | Software installation, configuration changes | Unauthorized modification detection |
| **Application Events** | Database queries, web requests, API calls | Application-layer attack detection |

### Strategic Log Collection

#### Scope Definition
Effective SIEM implementations require careful consideration of **what to log** versus **what's available to log**:

**Key Principle**: *SIEMs are analysis platforms, not log repositories*

**Scoping Considerations**:
- **Business risk priorities** drive log source selection
- **Data volume management** prevents platform overload
- **Storage cost optimization** through intelligent filtering
- **Analysis capability** focusing on actionable intelligence

#### Log Source Prioritization

**Tier 1 Sources** (Critical):
- Domain controllers and authentication systems
- Firewalls and network security devices
- Endpoint detection and response (EDR) systems
- Critical server and application logs

**Tier 2 Sources** (Important):
- Network infrastructure devices
- Web proxies and content filters
- Email security gateways
- Database audit logs

**Tier 3 Sources** (Supplementary):
- Workstation event logs
- IoT and facility systems
- Development and test environments
- Third-party cloud services

---

## Syslog Protocol and Implementation

### Syslog Architecture

**Syslog** serves as the universal logging protocol for network devices and Unix-based systems, providing standardized log transmission across heterogeneous environments.

#### Protocol Specifications
- **Standard**: RFC 5424 (current), RFC 3164 (legacy)
- **Transport**: UDP 514 (default), TCP 514 (reliable), TCP 6514 (secure)
- **Limitations**: No built-in authentication or encryption

#### Network Deployment Model

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Network   │    │   Security  │    │    Server   │
│   Devices   │───▶│   Gateway   │──▶│   Systems   │
│             │    │             │    │             │
└─────────────┘    └─────────────┘    └─────────────┘
       │                   │                   │
       ▼                   ▼                   ▼
┌──────────────────────────────────────────────────────┐
│                  Syslog Server/SIEM                  │
│  ┌─────────────────────────────────────────────────┐ │
│  │            Centralized Log Analysis             │ │
│  └─────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────┘
```

### Syslog Message Structure

#### Component Breakdown

**Complete Syslog Message Format**:
```
<PRI>HEADER MESSAGE
```

#### Priority Value (PRI) Calculation

**Formula**: `PRI = (Facility Code × 8) + Severity Level`

**Facility Codes**:
| Code | Facility | Description |
|------|----------|-------------|
| 0 | Kernel | Core system messages |
| 1 | User | User-level messages |
| 2 | Mail | Mail system messages |
| 3 | Daemon | System daemon messages |
| 4 | Security/Auth | Security/authorization messages |
| 5 | Syslogd | Syslog daemon messages |
| 6 | Line Printer | Line printer subsystem |
| 16 | Local0-7 | Local use facilities |

**Severity Levels**:
| Level | Keyword | Description | Usage |
|-------|---------|-------------|-------|
| 0 | Emergency | System unusable | Critical system failures |
| 1 | Alert | Action required immediately | Hardware failures, security breaches |
| 2 | Critical | Critical conditions | Disk full, backup failures |
| 3 | Error | Error conditions | Configuration errors, timeouts |
| 4 | Warning | Warning conditions | Deprecated features, minor issues |
| 5 | Notice | Normal but significant | User login, configuration changes |
| 6 | Informational | Informational messages | Process startup, routine operations |
| 7 | Debug | Debug-level messages | Debugging information |

#### Practical Example

**Message**: Security authentication failure
- **Facility**: 4 (Security/Auth)
- **Severity**: 3 (Error)
- **PRI Calculation**: (4 × 8) + 3 = 35
- **Complete Message**: `<35>Oct 12 08:15:32 firewall01 sshd: authentication failure for user admin`

---

## Windows Event Logging

### Windows Event Log Architecture

Windows provides comprehensive logging through the **Windows Event Log** service, storing events in binary format for efficient processing and analysis.

#### Event Log Categories

| Log Category | File Location | Content |
|--------------|---------------|---------|
| **Application** | Application.evtx | Application events, crashes, installations |
| **System** | System.evtx | Hardware events, driver loading, system errors |
| **Security** | Security.evtx | Authentication, authorization, audit events |
| **Directory Service** | Directory Service.evtx | Active Directory events (domain controllers only) |
| **DNS Server** | DNS Server.evtx | DNS service events (DNS servers only) |
| **File Replication** | File Replication Service.evtx | Domain controller replication events |

#### File System Locations

**Legacy Systems** (Windows 2000-Server 2003):
```
%WinDir%\system32\Config\*.evt
```

**Modern Systems** (Windows Vista+):
```
%WinDir%\system32\WinEVT\Logs\*.evtx
```

### Security Event Log Analysis

#### Critical Security Events

**Authentication Events**:
- **Event ID 4624**: Successful logon
- **Event ID 4625**: Failed logon attempt
- **Event ID 4634**: User logoff
- **Event ID 4672**: Special privileges assigned (administrator logon)

**Account Management**:
- **Event ID 4720**: User account created
- **Event ID 4726**: User account deleted
- **Event ID 4738**: User account changed

#### Event Viewer Custom Views

**Purpose**: Filter noise and focus on security-relevant events

**Custom View Creation Workflow**:
```
1. Open Event Viewer
2. Right-click "Custom Views" → "Create Custom View"
3. Configure filter criteria:
   - Time range (Last 24 hours, Last 7 days, Custom)
   - Event levels (Critical, Error, Warning, Information)
   - Event logs (Security, System, Application)
   - Event IDs (specific events of interest)
4. Save view with descriptive name
```

**Example: Login Monitoring View**:
- **Event IDs**: 4624, 4625, 4634, 4672
- **Time Range**: Last 24 hours
- **Purpose**: Monitor all authentication activity

### Advanced Windows Logging with Sysmon

#### Sysmon Enhanced Capabilities

**System Monitor (Sysmon)** provides granular system activity logging beyond standard Windows Event Logs.

**Key Advantages**:
- **Process creation** logging with full command lines
- **Network connection** monitoring with process attribution
- **File creation time** change detection
- **Driver and DLL** loading with hash validation
- **Session GUID** correlation for logon session tracking

#### Sysmon Installation and Configuration

**Basic Installation**:
```powershell
# Download from Microsoft Sysinternals
# Extract to directory
# Open elevated command prompt
sysmon -i
```

**Configuration Management**:
```powershell
# Install with configuration file
sysmon -i sysmonconfig.xml

# Update configuration
sysmon -c sysmonconfig.xml
```

**Example Configuration Sources**:
- **SwiftOnSecurity Config**: Community-maintained baseline configuration
- **Modular Sysmon**: Customizable, use-case specific configurations
- **HELK Project**: Complete logging ecosystem with Sysmon integration

#### Event Viewer Integration

**Custom View for Sysmon**:
```
Event Logs: Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
Event Levels: All levels selected
Filter: None (capture all Sysmon events)
```

---

## Alternative Logging Approaches

### Cloud Platform Logging

#### Microsoft Azure Logging

**Azure Monitor Architecture**:
- **Log Analytics Workspaces**: Centralized log storage and analysis
- **Azure REST API**: Programmatic log access and integration
- **Microsoft Graph API**: Identity and access management logs

**Kusto Query Language (KQL)**:
```kql
SecurityAlert
| where TimeGenerated > ago(1h)
| summarize count() by AlertSeverity
```

**Integration Capabilities**:
- Native SIEM integration (Azure Sentinel)
- Third-party SIEM connectors (Splunk, QRadar)
- Custom API integrations

#### Amazon Web Services Logging

**AWS CloudTrail and CloudWatch**:
- **API call logging** for all AWS service interactions
- **REST API structure** for programmatic access
- **Real-time streaming** to SIEM platforms

**Example API Call Structure**:
```
https://ec2.amazonaws.com/?Action=RunInstances
&ImageId=ami-60a54009
&MaxCount=3
&MinCount=1
&Placement.AvailabilityZone=us-east-1b
&Monitoring.Enabled=true
&AUTHPARAMS
```

### Endpoint Visibility with OSQuery

#### OSQuery Framework

**Concept**: Expose operating system as SQL-queryable database

**Cross-Platform Support**:
- Windows, macOS, Linux endpoint coverage
- Unified query language across operating systems
- Rich data collection from system APIs

**Production Considerations**:
- **Agent deployment** and configuration management
- **Query pack** development and maintenance
- **Data storage** and retention planning
- **Analysis platform** integration requirements

### Network Traffic Analysis with Arkime

#### Full Packet Capture and Indexing

**Arkime (formerly Moloch)** provides comprehensive network traffic capture and analysis:

**Core Capabilities**:
- **PCAP format** storage for tool compatibility
- **Elasticsearch indexing** for fast search and retrieval
- **Web interface** for intuitive packet analysis
- **API access** for programmatic integration
- **Scalable architecture** supporting multi-gigabit capture rates

**Deployment Characteristics**:
- **Distributed sensor** deployment across network segments
- **Centralized analysis** through web interface
- **Retention policies** based on available storage
- **Integration capabilities** with existing security tools

---

## Log Aggregation Strategies

### Aggregation Methods

#### Collection Approaches

| Method | Description | Use Cases |
|--------|-------------|-----------|
| **Syslog** | Standard protocol with server/client model | Network devices, Unix systems |
| **Event Streaming** | Real-time protocols (SNMP, NetFlow, IPFIX) | Network monitoring, traffic analysis |
| **Log Collectors** | Agent-based collection with parsing | Endpoints, applications, databases |
| **Direct Access** | API-based or protocol-based direct collection | Cloud services, custom applications |

### Data Structure Considerations

#### Structured vs. Unstructured Data

**Structured Data Characteristics**:
- **Predefined fields** with consistent formatting
- **Common sources**: Apache logs, IIS logs, Windows events, Cisco devices
- **Parsing advantage**: Easily normalized and correlated
- **Examples**: Source IP, destination port, timestamp, event code

**Unstructured Data Challenges**:
- **Variable formatting** across different operations
- **Common sources**: Custom applications, debug logs, free-form text
- **Processing complexity**: Requires advanced parsing and normalization
- **Multi-line events**: No clear start/end boundaries

#### Normalization Requirements

**Purpose**: Create consistent field mapping across diverse log sources

**Common Normalized Fields**:
- **src_ip**, **dest_ip**: Network source and destination
- **user**: Account identifier across different systems
- **event_time**: Standardized timestamp format
- **event_type**: Categorized event classification
- **severity**: Normalized criticality levels

**Benefits**:
- **Cross-source correlation** capabilities
- **Simplified search** and analysis queries
- **Consistent alerting** rules across platforms
- **Standardized reporting** and dashboards

[⬆️ Back to SIEM & Monitoring](./README.md)
