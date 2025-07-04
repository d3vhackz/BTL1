# 5.3: SIEM Data Correlation and Processing

SIEM correlation transforms raw log data into actionable security intelligence through normalization, enrichment, and rule-based analysis. This process enables security teams to detect complex attacks that span multiple systems and timeframes.

---

## Data Processing Pipeline

### Log Normalization Fundamentals

**Normalization** standardizes diverse log formats into consistent field structures, enabling cross-platform correlation and analysis.

#### The Normalization Challenge

Different vendors, applications, and systems produce logs in unique formats with varying field names and structures:

| System Type | Example Log Format | Key Fields |
|-------------|-------------------|------------|
| **Cisco ASA** | `%ASA-6-302013: Built outbound TCP connection 12345 for outside:1.1.1.1/80` | src_ip, dest_ip, port |
| **Juniper SRX** | `RT_FLOW - RT_FLOW_SESSION_CREATE [source: 192.168.1.10] [destination: 8.8.8.8]` | source_address, destination_address |
| **Windows Security** | `Event ID 4624: An account was successfully logged on. Account Name: user01` | Account_Name, Logon_Type |
| **Linux Syslog** | `sshd[1234]: Accepted password for admin from 10.0.0.5 port 22` | user, src_host, service |

#### Normalized Field Mapping

**Common Normalized Fields**:
```
Original Field Names → Normalized Field Name
├── Cisco: src_ip → source_ip
├── Juniper: source_address → source_ip  
├── Windows: Source_Network_Address → source_ip
└── Linux: from → source_ip
```

**Benefits of Normalization**:
- **Universal search** across all log sources using standardized field names
- **Cross-platform correlation** rules without vendor-specific syntax
- **Simplified reporting** with consistent data presentation
- **Reduced complexity** in rule creation and maintenance

### Log Enrichment Strategies

#### Contextual Data Enhancement

**Enrichment** adds valuable context to raw log data to improve analysis efficiency and accuracy.

#### Enrichment Categories

| Enrichment Type | Purpose | Example Enhancement |
|----------------|---------|-------------------|
| **Geolocation** | IP address geographic attribution | IP: 8.8.8.8 → Country: United States, City: Mountain View |
| **Threat Intelligence** | Malicious indicator identification | Domain: evil.com → Threat: Known C2 server |
| **Asset Information** | Internal system identification | IP: 192.168.1.100 → Hostname: DC01, Owner: IT Department |
| **User Context** | Account attribute mapping | Username: jsmith → Department: Finance, Title: Analyst |
| **DNS Resolution** | Hostname enrichment | IP: 1.1.1.1 → Hostname: one.one.one.one |

#### Practical Enrichment Examples

**Geographic Intelligence**:
```
Original Log: Failed login from 91.189.88.152
Enriched Log: Failed login from 91.189.88.152 (London, United Kingdom)
Analysis Value: Identifies potential geographic anomalies
```

**Threat Intelligence Integration**:
```
Original Log: HTTP request to badactor.com
Enriched Log: HTTP request to badactor.com [IOC: Command & Control]
Analysis Value: Immediate threat classification
```

### Data Storage and Indexing

#### Storage Architecture Considerations

**Storage Requirements Scale**:
- **Small Organization** (100 users): 1-5 GB/day
- **Medium Enterprise** (1,000 users): 10-50 GB/day  
- **Large Enterprise** (10,000+ users): 100+ GB/day

**Storage Technology Options**:

| Technology | Advantages | Use Cases |
|------------|------------|-----------|
| **On-Premises SAN** | High performance, full control | Security-sensitive environments |
| **AWS S3** | Scalable, cost-effective | Cloud-first organizations |
| **Hadoop HDFS** | Big data optimized | Analytics-heavy deployments |
| **Hybrid Cloud** | Flexibility, cost optimization | Mixed infrastructure environments |

#### Indexing for Performance

**Index Strategy**:
- **Primary indexes** on frequently searched fields (timestamp, source_ip, user)
- **Secondary indexes** on correlation fields (session_id, process_id)
- **Full-text indexes** for content-based searches
- **Time-based partitioning** for efficient historical queries

**Performance Impact**:
```
Without Indexing: Search across 30 days = 45 seconds
With Indexing: Search across 30 days = 2 seconds
```

---

## SIEM Rule Development

### Rule Architecture and Types

#### Rule Categories

**Provider Rules** (Out-of-the-box):
- **Generic attack patterns** applicable across environments
- **Compliance templates** for regulatory requirements  
- **Vendor-specific detections** for security tools
- **Baseline behavioral rules** for common anomalies

**Custom Rules** (Organization-specific):
- **Business logic** tailored to organizational processes
- **Environment-specific** detections based on unique infrastructure
- **Threat-specific** rules based on intelligence and incidents
- **Performance-optimized** rules for high-volume environments

### Detection Logic Implementation

#### Rule Execution Models

**Real-time Processing**:
- **Stream-based analysis** of incoming log data
- **Immediate alerting** for critical security events
- **Low latency** detection for time-sensitive threats
- **Resource intensive** due to continuous processing

**Scheduled Processing**:
- **Batch analysis** at defined intervals (hourly, daily)
- **Historical correlation** across extended timeframes
- **Resource efficient** for complex analytical rules
- **Delayed detection** acceptable for trend analysis

#### Example Detection Scenarios

**Authentication Monitoring**:
```
Rule Logic: Failed Login Threshold
- Monitor: Windows Event ID 4625 (Failed Logon)
- Condition: Account fails login > 10 times in 10 minutes
- Action: Generate high-priority alert
- Rationale: Detect brute force attacks
```

**Process Execution Analysis**:
```
Rule Logic: Suspicious Process Relationships  
- Monitor: Process creation events (Sysmon Event ID 1)
- Condition: winword.exe spawns cmd.exe OR powershell.exe
- Action: Generate medium-priority alert
- Rationale: Detect malicious macro execution
```

**Network Activity Detection**:
```
Rule Logic: Port Scanning Identification
- Monitor: Firewall connection logs
- Condition: Single source IP connects to >100 unique ports in 5 minutes
- Action: Generate alert and block source IP
- Rationale: Detect reconnaissance activity
```

### False Positive Management

#### Threshold Optimization

**Single Event vs. Pattern Detection**:

**Ineffective Rule**:
```
Trigger: ANY failed login attempt
Result: 500+ alerts per day from normal user typos
Value: Near zero due to noise
```

**Optimized Rule**:
```
Trigger: 10+ failed logins within 10 minutes from same source
Result: 2-3 relevant alerts per day
Value: High-confidence brute force detection
```

#### Exclusion Strategies

**Whitelist Implementation Example**:
```
Scenario: Vulnerability scanner triggers network scanning alerts
Original Rule: Alert on >50 connection attempts to different hosts
Modified Rule: Alert on >50 connection attempts to different hosts
                EXCLUDE source_ip: 192.168.100.50 (vulnerability scanner)
Result: Legitimate scanning activity ignored, malicious scanning detected
```

**Dynamic Exclusions**:
- **Time-based exclusions** for scheduled maintenance windows
- **Service account exclusions** for automated processes
- **Geographic exclusions** for known business locations
- **Asset-based exclusions** for approved security tools

---

## Sigma Rule Framework

### Universal Detection Language

**Sigma** provides a vendor-agnostic format for sharing detection rules across different SIEM platforms, promoting collaborative security and reducing vendor lock-in.

#### Sigma Architecture Benefits

| Benefit | Description | Impact |
|---------|-------------|--------|
| **Platform Agnostic** | Write once, deploy everywhere | Reduced development time |
| **Community Sharing** | Standardized rule exchange | Improved collective defense |
| **Vendor Independence** | Avoid SIEM vendor lock-in | Strategic flexibility |
| **Research Integration** | Academic and industry collaboration | Enhanced detection quality |

### Supported SIEM Platforms

**Sigma Converter (Sigmac) Support**:
- **Splunk** - SPL (Search Processing Language)
- **IBM QRadar** - AQL (Ariel Query Language)  
- **Micro Focus ArcSight** - ArcSight Search
- **Elasticsearch** - Query DSL, Lucene syntax
- **LogPoint** - LogPoint Query Language
- **Microsoft Sentinel** - KQL (Kusto Query Language)

### Sigma Rule Structure

#### Component Breakdown

```yaml
title: Web Shell Detection via Suspicious URL Parameters
id: 12345678-1234-1234-1234-123456789012
status: experimental
description: Detects potential web shell access through suspicious URL parameters
author: Security Team
date: 2024/01/15
references:
    - https://owasp.org/www-community/attacks/Web_Shell
logsource:
    category: webserver
detection:
    selection:
        cs-uri-query|contains:
            - '=whoami'
            - '=id'
            - '=pwd'
            - '=ls'
            - '=dir'
            - '=cat'
            - '=type'
    condition: selection
falsepositives:
    - Legitimate applications using command-like parameters
    - Development/testing environments
level: high
tags:
    - attack.persistence
    - attack.t1505.003
```

#### Field Definitions

| Field | Purpose | Example |
|-------|---------|---------|
| **title** | Human-readable rule description | "Suspicious PowerShell Execution" |
| **status** | Rule maturity level | experimental, testing, stable |
| **logsource** | Required log type/category | webserver, dns, process_creation |
| **detection** | Core detection logic | selection criteria and conditions |
| **falsepositives** | Known benign triggers | legitimate tools, business processes |
| **level** | Alert priority/severity | low, medium, high, critical |
| **tags** | MITRE ATT&CK technique mapping | attack.t1059.001 (PowerShell) |

### Practical Sigma Development

#### Threat Intelligence Integration

**DNS Tunneling Detection Example**:

**Intelligence Brief**:
- **Threat**: TRANSPORTER malware family  
- **C2 Domain**: redhunt.net
- **Behavior**: DNS tunneling with ~500 queries average
- **MITRE Technique**: T1071.004 (DNS Application Layer Protocol)

**Sigma Rule Implementation**:
```yaml
title: TRANSPORTER Malware DNS C2 Communication
status: production  
description: Detects DNS tunneling by TRANSPORTER malware family
author: Threat Intelligence Team
date: 2024/07/03
references:
    - internal://threat-intel/TRANSPORTER-analysis-2024
logsource:
    category: dns
detection:
    selection:
        query|endswith: '.redhunt.net'
    condition: selection | count() by src_ip > 300
    timeframe: 1h
falsepositives:
    - CDN services with high query volumes  
    - Legitimate DNS-based applications
level: high
tags:
    - attack.command_and_control
    - attack.t1071.004
```

#### Community Rule Repositories

**Primary Sources**:
- **SigmaHQ Repository**: https://github.com/SigmaHQ/sigma/tree/master/rules
- **Florian Roth Collection**: High-quality, battle-tested rules
- **MITRE Cyber Analytics Repository**: ATT&CK-mapped detections
- **Vendor Communities**: Platform-specific rule sharing

**Rule Categories Available**:
- **Process execution** patterns and anomalies
- **Network communication** and C2 detection  
- **File system** manipulation and persistence
- **Registry** modifications and privilege escalation
- **Cloud services** and SaaS security monitoring

#### Conversion and Deployment Workflow

```bash
# Convert Sigma rule to Splunk SPL
sigmac -t splunk rule.yml

# Convert to Elasticsearch Query DSL  
sigmac -t es-qs rule.yml

# Convert to QRadar AQL
sigmac -t qradar rule.yml

# Batch convert entire rule directory
sigmac -t splunk rules/*.yml --output-dir converted/
```

**Deployment Process**:
1. **Rule validation** in test environment
2. **False positive assessment** with historical data
3. **Performance impact** analysis
4. **Gradual rollout** with monitoring
5. **Continuous tuning** based on operational feedback

[⬆️ Back to SIEM & Monitoring](./README.md)
