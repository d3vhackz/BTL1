# 5.15: Threat Hunting with SIEM

Threat hunting transforms SIEM platforms from reactive alerting systems into proactive threat detection engines. By leveraging SIEM's data aggregation and search capabilities, hunters can systematically search for indicators of advanced threats that bypass traditional automated defenses.

---

## Understanding Threat Hunting

**Threat hunting** is the iterative process of proactively searching for cyber threats that have evaded existing security controls. Unlike automated detection systems that rely on known signatures and rules, hunting uses human intuition, experience, and hypothesis-driven investigation to uncover sophisticated attacks.

### Hunting vs. Traditional Detection

| Aspect | Traditional Detection | Threat Hunting |
|--------|----------------------|----------------|
| **Approach** | Reactive, rule-based alerts | Proactive, hypothesis-driven investigation |
| **Triggers** | Known signatures and patterns | Analyst intuition and threat intelligence |
| **Scope** | Specific, predefined scenarios | Broad, exploratory analysis |
| **Methodology** | Automated correlation rules | Manual investigation with tool assistance |
| **Outcome** | Immediate alerts and blocking | New detection rules, TTPs, and IOCs |

### The Hunter Mindset

Effective threat hunters operate under the assumption that **adversaries are already present** in the environment. This mindset shift drives hunters to:
- Question normal-looking activity for hidden anomalies
- Correlate seemingly unrelated events across time and systems
- Think like attackers to anticipate their methods and goals
- Develop testable hypotheses based on threat intelligence

---

## Threat Hunting Methodologies

### Hypothesis-Driven Hunting

The most mature hunting approach follows the scientific method with structured investigation phases.

#### Phase 1: Hypothesis Formation
Hunters develop specific, testable theories about potential threats:

```
Example Hypotheses:
• "Attackers are using Living-off-the-Land techniques with PowerShell"
• "Lateral movement is occurring via compromised service accounts"  
• "Data exfiltration is happening through DNS tunneling"
• "Privilege escalation is using known CVE-2023-XXXX exploit"
```

#### Phase 2: Data Collection and Analysis
Using SIEM to gather relevant evidence:

```spl
# Hunt for suspicious PowerShell activity
index=windows EventCode=4688 
| where match(CommandLine, "(?i)-enc|-encodedcommand|-windowstyle\s+hidden")
| stats count values(CommandLine) as commands by Computer, User
| where count > 5
```

#### Phase 3: Pattern Recognition and Validation
Identifying anomalies and confirming malicious intent:

```spl
# Correlate PowerShell with network connections
index=windows EventCode=4688 CommandLine="*powershell*"
| join Computer [search index=network src_host=Computer dest_port!=80,443,53]
| table _time Computer User CommandLine dest_ip dest_port
```

### Intelligence-Based Hunting

This reactive methodology leverages external threat intelligence to search for known IOCs and TTPs.

#### IOC-Based Searches
```spl
# Search for known malicious IP addresses
index=network 
| search dest_ip IN ("192.168.100.1", "10.0.0.50", "203.0.113.15")
| stats count by src_ip, dest_ip, dest_port
| sort -count
```

#### TTP-Based Hunting
Using frameworks like MITRE ATT&CK to hunt for specific techniques:

```spl
# Hunt for T1059.001 - PowerShell execution
index=windows source=WinEventLog:Microsoft-Windows-PowerShell/Operational
| search EventCode=4104 
| regex ScriptBlockText="(Invoke-|IEX|DownloadString|System\.Net\.WebClient)"
| table _time Computer UserID ScriptBlockText
```

### Situational Awareness Hunting

Custom hunting based on organizational context, industry threats, and environmental factors.

#### Crown Jewel Analysis
Focus hunting efforts on critical assets:

```spl
# Monitor access to critical servers
index=windows EventCode=4624 
| lookup critical_assets Computer OUTPUT asset_criticality
| where asset_criticality="High" 
| stats count by Computer, Account_Name, Logon_Type
| where count > 10
```

---

## SIEM-Enabled Hunting Techniques

### Frequency Analysis

Identifying rare or unusual events that may indicate malicious activity.

```spl
# Find rare process executions
index=windows EventCode=4688
| stats count by Process_Name
| sort count
| head 20
| eval rarity_score=if(count<5, "Very Rare", if(count<20, "Rare", "Common"))
```

### Temporal Analysis

Hunting for activities occurring at unusual times or in suspicious patterns.

```spl
# Detect off-hours administrative activity
index=windows EventCode=4672
| eval hour=strftime(_time, "%H")
| where hour<6 OR hour>22
| stats count by Account_Name, hour
| sort -count
```

### Stacking and Clustering

Grouping similar data to identify outliers and anomalies.

#### Domain Stacking
```spl
# Stack DNS requests to identify suspicious domains
index=dns
| stats count by query
| sort count
| head 50
| eval reputation=if(count<3, "Suspicious", "Normal")
```

#### User Behavior Clustering
```spl
# Cluster users by authentication patterns
index=windows EventCode=4624
| stats count, dc(Computer) as unique_computers by Account_Name
| where unique_computers > 10 OR count > 100
| sort -unique_computers
```

### Data Volume Analysis

Hunting for unusual data transfer patterns indicating exfiltration.

```spl
# Detect large data transfers
index=network
| stats sum(bytes_out) as total_bytes by src_ip, dest_ip
| where total_bytes > 1000000000
| eval GB_transferred=round(total_bytes/1024/1024/1024, 2)
| sort -GB_transferred
```

---

## Advanced Hunting Scenarios

### Lateral Movement Detection

Identifying attackers moving through the network after initial compromise.

```spl
# Hunt for credential usage across multiple systems
index=windows EventCode=4624 Logon_Type=3
| stats count, values(Computer) as accessed_systems by Account_Name
| eval system_count=mvcount(accessed_systems)
| where system_count > 5
| sort -system_count
```

### Living-off-the-Land Detection

Finding abuse of legitimate system tools for malicious purposes.

```spl
# Detect suspicious use of legitimate tools
index=windows EventCode=4688
| search Process_Name IN ("powershell.exe", "cmd.exe", "wmic.exe", "rundll32.exe")
| rex field=CommandLine "(?<technique>-enc|-w\s+hidden|\/c\s+\w+|process\s+call\s+create)"
| where isnotnull(technique)
| stats count by Computer, User, Process_Name, technique
```

### Persistence Mechanism Hunting

Searching for techniques used to maintain access.

```spl
# Hunt for registry persistence mechanisms
index=windows EventCode=13
| search TargetObject="*\\Run*" OR TargetObject="*\\RunOnce*" OR TargetObject="*\\Services\\*"
| stats count by Computer, ProcessName, TargetObject, Details
| where count < 5
```

---

## SIEM Hunting Workflows

### Structured Hunt Process

#### 1. Hunt Planning
```yaml
Hunt Plan Template:
Hypothesis: "Attackers are using WMI for lateral movement"
Scope: Domain controllers and critical servers
Timeframe: Last 30 days
Data Sources: Windows event logs, network logs
Success Criteria: Identify unauthorized WMI activity
```

#### 2. Data Exploration
```spl
# Initial reconnaissance of WMI activity
index=windows EventCode=5857,5858,5859,5860,5861
| stats count by EventCode, ComputerName
| sort -count
```

#### 3. Hypothesis Testing
```spl
# Detailed WMI analysis
index=windows EventCode=5857
| eval src_ip=if(isnotnull(ClientMachine), ClientMachine, "localhost")
| stats count, values(Operation) as operations by src_ip, User
| where count > 10
```

### Iterative Refinement

Hunters continuously refine searches based on findings:

```spl
# Refined search based on initial findings
index=windows EventCode=5857 User!="SYSTEM" User!="LOCAL SERVICE"
| join type=left ClientMachine [search index=network | stats count by src_ip | rename src_ip as ClientMachine]
| where isnull(count)
| table _time ClientMachine User Operation
```

---

## Hunt Team Organization

### Hunting Maturity Levels

| Level | Characteristics | Capabilities | Focus Areas |
|-------|----------------|-------------|-------------|
| **Level 1** | Ad-hoc hunting | Manual searches based on IOCs | Known threats, basic analysis |
| **Level 2** | Procedural hunting | Defined processes and playbooks | Regular cadence, documentation |
| **Level 3** | Innovative hunting | Custom analytics and automation | Hypothesis-driven, new TTPs |
| **Level 4** | Leading hunting | Continuous improvement and sharing | Threat intelligence production |

### Team Structure and Roles

#### Senior Threat Hunter
- Develops hunting hypotheses and methodologies
- Leads complex investigations and analysis
- Mentors junior hunters and shares expertise

#### Threat Intelligence Analyst
- Provides context and IOCs for hunting activities  
- Analyzes threat landscape and emerging TTPs
- Creates threat profiles and hunt recommendations

#### SIEM Analyst
- Executes hunt queries and data analysis
- Maintains hunt playbooks and documentation
- Performs initial triage and validation

---

## Measuring Hunt Effectiveness

### Key Performance Indicators

```spl
# Track hunt metrics over time
| inputlookup hunt_metrics.csv
| stats avg(time_to_detect) as avg_detection_time,
        avg(time_to_respond) as avg_response_time,
        sum(true_positives) as total_threats_found,
        sum(false_positives) as total_false_positives
        by month
| eval detection_accuracy=round(total_threats_found/(total_threats_found+total_false_positives)*100, 2)
```

### Hunt Success Metrics

| Metric | Description | Target |
|--------|-------------|---------|
| **Mean Time to Detection (MTTD)** | Average time from compromise to discovery | < 24 hours |
| **Hunt Accuracy** | Ratio of true positives to total findings | > 80% |
| **Coverage Breadth** | Percentage of MITRE ATT&CK techniques hunted | > 70% |
| **Intelligence Generation** | New IOCs/TTPs discovered per hunt | > 3 per hunt |

### Continuous Improvement

```spl
# Analyze hunt effectiveness by technique
| inputlookup hunt_results.csv
| stats count(eval(result="true_positive")) as successful_hunts,
        count as total_hunts
        by mitre_technique
| eval success_rate=round(successful_hunts/total_hunts*100, 2)
| sort -success_rate
```

---

## Integration with Threat Intelligence

### IOC Enrichment in Hunts

```spl
# Enrich hunt results with threat intelligence
index=network
| lookup threat_intel_feed ip as dest_ip OUTPUT threat_category, confidence_score
| where isnotnull(threat_category)
| stats count by src_ip, dest_ip, threat_category, confidence_score
| sort -confidence_score
```

### Hunt-Generated Intelligence

Successful hunts produce valuable intelligence for the broader security program:

- **New IOCs**: IP addresses, domains, file hashes discovered during hunting
- **TTPs**: Novel attack techniques and behavioral patterns
- **Attribution**: Links between different attack campaigns
- **Detection Logic**: New correlation rules and search queries

### Feedback Loop

```spl
# Convert hunt findings into detection rules
# Hunt finding: Suspicious PowerShell with network connections
# Convert to alerting rule:
index=windows EventCode=4688 CommandLine="*powershell*"
| join Computer [search index=network earliest=-1h latest=now src_host=Computer dest_port!=80,443,53]
| eval alert_severity="medium"
| table _time Computer User CommandLine dest_ip alert_severity
```

---

## Best Practices for SIEM-Based Hunting

### Data Preparation
- **Normalize field names** across different log sources for consistent hunting
- **Enrich logs** with asset criticality, user roles, and threat intelligence
- **Optimize retention** to support long-term hunting and forensic analysis
- **Index strategically** to balance search performance with storage costs

### Query Optimization
- **Start broad** then narrow down based on findings
- **Use time boundaries** to improve search performance
- **Leverage statistical functions** to identify outliers and anomalies  
- **Document queries** for reuse and knowledge sharing

### Operational Excellence
- **Maintain hunt calendars** to ensure regular and systematic coverage
- **Create playbooks** for common hunt scenarios and TTPs
- **Share findings** with detection engineering and threat intelligence teams
- **Measure and improve** hunt effectiveness through metrics and feedback

> **Key Takeaway**: Effective threat hunting with SIEM requires combining human expertise with powerful data analysis capabilities. Success comes from structured methodologies, comprehensive data access, and continuous refinement of hunting techniques based on evolving threat landscapes.

[⬆️ Back to SIEM & Monitoring](./README.md)
