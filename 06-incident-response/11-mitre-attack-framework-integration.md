# 6.11: MITRE ATT&CK Framework Integration

The MITRE ATT&CK™ framework provides a comprehensive knowledge base of adversary tactics and techniques that transforms incident response capabilities by offering a standardized language for describing, analyzing, and defending against cyber attacks.

---

## Framework Foundation

ATT&CK serves as the backbone for modern incident response by creating a common vocabulary that bridges technical teams, management, and external stakeholders.

### Core Framework Structure

#### **The 14 Tactical Objectives**

MITRE ATT&CK organizes adversary behavior into 14 high-level tactics representing the "why" behind attacker actions:

```mermaid
graph LR
    A[Reconnaissance] --> B[Resource Development]
    B --> C[Initial Access]
    C --> D[Execution]
    D --> E[Persistence]
    E --> F[Privilege Escalation]
    F --> G[Defense Evasion]
    G --> H[Credential Access]
    H --> I[Discovery]
    I --> J[Lateral Movement]
    J --> K[Collection]
    K --> L[Command & Control]
    L --> M[Exfiltration]
    M --> N[Impact]
```

| Tactic | IR Application | Key Investigation Focus |
|--------|----------------|----------------------|
| **Reconnaissance** | Pre-attack indicators, threat hunting | OSINT collection, target profiling |
| **Initial Access** | Attack vector analysis, entry point identification | Phishing, exploits, supply chain |
| **Persistence** | Cleanup verification, eradication validation | Backdoors, scheduled tasks, registry |
| **Privilege Escalation** | Impact assessment, containment planning | Account compromise, vulnerability exploitation |
| **Defense Evasion** | Detection gap analysis, rule improvement | AV bypasses, obfuscation, process injection |
| **Credential Access** | Account security, password policy review | Credential dumping, brute force, keylogging |
| **Lateral Movement** | Network analysis, additional system identification | SMB, RDP, WMI abuse |
| **Collection** | Data impact assessment, exfiltration analysis | Screen capture, keylogging, file collection |
| **Exfiltration** | Data loss quantification, regulatory notification | C2 channels, cloud storage, physical media |

#### **Techniques and Sub-Techniques Hierarchy**

```markdown
Example: Credential Access Tactic

T1003 - OS Credential Dumping
├── T1003.001 - LSASS Memory
├── T1003.002 - Security Account Manager  
├── T1003.003 - NTDS
├── T1003.004 - LSA Secrets
├── T1003.005 - Cached Domain Credentials
├── T1003.006 - DCSync
├── T1003.007 - Proc Filesystem
└── T1003.008 - /etc/passwd and /etc/shadow
```

---

## Threat Hunting Integration

ATT&CK transforms threat hunting from reactive searching to proactive, hypothesis-driven investigations.

### Hunt Hypothesis Development

#### **ATT&CK-Driven Hunting Process**

**Systematic Hunt Planning**:

| Phase | Duration | Activities | ATT&CK Integration |
|-------|----------|------------|-------------------|
| **Intelligence Gathering** | 1-2 hours | Threat landscape analysis, APT research | Map active threat actors to techniques |
| **Hypothesis Creation** | 30 minutes | Specific, testable assumptions | Focus on high-risk, low-visibility techniques |
| **Query Development** | 1-2 hours | Platform-specific detection logic | Technique-based search criteria |
| **Hunt Execution** | 2-8 hours | Data analysis and investigation | Evidence collection and technique validation |
| **Results Analysis** | 1-2 hours | Finding assessment and follow-up | Update technique coverage assessment |

#### **Hunt Hypothesis Examples**

**T1003.001 - LSASS Memory Dumping**:
```sql
-- Splunk hunting query
index=sysmon EventCode=10 TargetImage="*lsass.exe" 
| where NOT (SourceImage="C:\\Windows\\System32\\wbem\\WmiPrvSE.exe" 
           OR SourceImage="C:\\Windows\\System32\\csrss.exe")
| stats count by SourceImage, SourceProcessId, TargetProcessId
| where count > 1
```

**T1055 - Process Injection**:
```kql
// KQL hunting query for Microsoft Sentinel
DeviceProcessEvents
| where ProcessCommandLine has_any ("CreateRemoteThread", "VirtualAllocEx", "WriteProcessMemory")
| where not(InitiatingProcessFileName has_any ("chrome.exe", "firefox.exe", "outlook.exe"))
| project Timestamp, DeviceName, ProcessCommandLine, InitiatingProcessFileName
```

### Coverage Assessment and Gap Analysis

#### **ATT&CK Navigator for Defense Mapping**

**Detection Coverage Matrix**:

| ATT&CK Technique | Detection Method | Coverage Level | Data Source | Alert Quality Score |
|------------------|------------------|----------------|-------------|-------------------|
| **T1003.001** | Process monitoring | High | EDR, Sysmon | 8/10 - Low false positives |
| **T1055** | Behavioral analysis | Medium | EDR | 6/10 - Requires tuning |
| **T1071** | Network monitoring | Low | Proxy logs | 4/10 - High false positives |
| **T1095** | Traffic analysis | None | No capability | N/A - Blind spot identified |

**Gap Prioritization Framework**:
```
Risk Score = (Threat Actor Usage × Business Impact × Detection Difficulty) / Current Coverage

Where:
- Threat Actor Usage: Frequency of technique use by relevant APTs (1-5)
- Business Impact: Potential damage if technique succeeds (1-5)  
- Detection Difficulty: Technical complexity of detection (1-5)
- Current Coverage: Existing detection capability (0-5)
```

---

## Detection Engineering with ATT&CK

ATT&CK provides the foundation for systematic detection rule development and validation.

### Rule Development Methodology

#### **ATT&CK-Mapped Detection Rules**

**Standard Rule Template**:
```markdown
## Detection Rule: [Rule Name]

**ATT&CK Mapping**: [T####] - [Technique Name]
**Tactic**: [Primary Tactic]
**Sub-Technique**: [T####.###] - [Sub-Technique Name] (if applicable)

**Rule Logic**:
```sql
[Detection query with platform-specific syntax]
```

**Data Sources Required**:
- [Primary data source]
- [Secondary data source]
- [Additional context sources]

**False Positive Considerations**:
- [Common legitimate activity that may trigger]
- [Recommended filtering approaches]

**Validation Tests**:
- [ ] Rule triggers on known malicious activity
- [ ] False positive rate < 5% in production
- [ ] Alert provides actionable information
- [ ] Response procedures documented
```

#### **Technique-Specific Detection Examples**

**T1059.001 - PowerShell**:
```yaml
# Sigma rule format
title: Suspicious PowerShell Command Execution
id: 12345678-1234-1234-1234-123456789012
status: experimental
description: Detects suspicious PowerShell command patterns
references:
    - https://attack.mitre.org/techniques/T1059/001/
tags:
    - attack.execution
    - attack.t1059.001
logsource:
    product: windows
    service: powershell
detection:
    selection:
        EventID: 4104
    keywords:
        - "DownloadString"
        - "IEX"
        - "Invoke-Expression"
        - "bypass"
        - "EncodedCommand"
    condition: selection and keywords
falsepositives:
    - Administrative scripts
    - Software deployment tools
level: medium
```

### Detection Validation and Testing

#### **Purple Team Testing with ATT&CK**

**Validation Test Framework**:

| Test Type | Scope | Method | Success Criteria |
|-----------|-------|---------|------------------|
| **Unit Testing** | Single technique | Isolated technique simulation | Detection triggers correctly |
| **Integration Testing** | Multi-technique chain | Attack path simulation | Full kill chain detected |
| **Stress Testing** | High-volume scenarios | Automated technique execution | Performance maintained under load |
| **Evasion Testing** | Advanced techniques | Red team simulation | Detection resilient to bypass attempts |

---

## Incident Analysis and Response

ATT&CK transforms incident analysis from ad-hoc investigation to systematic attack reconstruction.

### Attack Chain Reconstruction

#### **ATT&CK-Based Timeline Analysis**

**Incident Mapping Template**:

```markdown
## Incident ATT&CK Analysis - [Incident ID]

### Attack Overview
**Threat Actor Assessment**: [Known/Suspected attribution]
**Campaign Correlation**: [Related to known campaigns]
**Attack Sophistication**: [Novice/Intermediate/Advanced/Expert]

### Technique Timeline

| Time | ATT&CK ID | Technique | Evidence | Confidence |
|------|-----------|-----------|----------|------------|
| 14:23 | T1566.001 | Spearphishing Attachment | Malicious PDF opened | High |
| 14:24 | T1204.002 | Malicious File | Macro execution observed | High |
| 14:25 | T1059.001 | PowerShell | Base64 encoded command | Medium |
| 14:27 | T1003.001 | LSASS Memory | Mimikatz execution detected | High |
| 14:35 | T1021.001 | Remote Desktop Protocol | RDP connection to DC | High |
| 14:42 | T1083 | File and Directory Discovery | Mass file enumeration | Medium |
| 15:18 | T1041 | Exfiltration Over C2 | Large data transfer | High |

### Tactical Analysis Summary
- **Primary Tactics**: Initial Access → Execution → Credential Access → Lateral Movement → Exfiltration
- **Technique Count**: 7 distinct techniques observed
- **Attack Duration**: 55 minutes from initial access to data exfiltration
- **Sophistication Assessment**: Intermediate - Common tools with effective execution
```

#### **Defensive Gap Analysis**

**Post-Incident ATT&CK Assessment**:

| Observed Technique | Current Detection | Gap Analysis | Recommended Improvement |
|-------------------|------------------|--------------|------------------------|
| **T1027** | Limited PowerShell monitoring | Missed obfuscated commands | Enhanced PowerShell logging, behavioral analysis |
| **T1021.001** | Basic RDP logging | No anomaly detection | Implement RDP behavior analytics |
| **T1083** | No file access monitoring | Complete blind spot | Deploy file access monitoring on critical systems |

---

## Response Planning and Automation

ATT&CK enables technique-specific response strategies that match the sophistication of observed threats.

### Technique-Specific Response Actions

#### **Automated Response Mapping**

| ATT&CK Technique | Containment Actions | Eradication Steps | Recovery Validation |
|------------------|-------------------|------------------|-------------------|
| **T1003** | Force credential resets, disable affected accounts | Remove credential harvesting tools, clear cached credentials | Verify no unauthorized access, implement enhanced monitoring |
| **T1021** | Block remote access protocols, audit sessions | Remove unauthorized remote access tools, patch vulnerabilities | Test legitimate remote access, update access policies |
| **T1041** | Block C2 domains/IPs, monitor egress traffic | Remove malware, clean infected systems | Verify no ongoing C2 communication, enhance DLP |
| **T1055** | Terminate injected processes, isolate affected systems | Memory cleanup, remove injection tools | Validate process integrity, implement memory protection |

#### **SOAR Integration for ATT&CK Response**

**Automated Playbook Example**:
```python
class ATTACKResponseOrchestrator:
    def __init__(self):
        self.technique_playbooks = {
            'T1003.001': self.lsass_memory_response,
            'T1059.001': self.powershell_abuse_response,
            'T1021.001': self.rdp_abuse_response
        }
    
    def execute_technique_response(self, technique_id, incident_context):
        """Execute automated response based on ATT&CK technique"""
        
        if technique_id in self.technique_playbooks:
            response_actions = self.technique_playbooks[technique_id](incident_context)
        else:
            response_actions = self.generic_response_playbook(technique_id, incident_context)
        
        return {
            'technique_id': technique_id,
            'automated_actions': response_actions,
            'manual_review_items': self.generate_manual_tasks(technique_id),
            'follow_up_monitoring': self.define_monitoring_requirements(technique_id)
        }
    
    def lsass_memory_response(self, context):
        """Specific response for T1003.001 - LSASS Memory dumping"""
        actions = []
        
        # Immediate containment
        actions.append(self.isolate_affected_systems(context['affected_hosts']))
        actions.append(self.force_credential_reset(context['potentially_compromised_accounts']))
        
        # Evidence collection
        actions.append(self.collect_memory_dump(context['affected_hosts']))
        actions.append(self.preserve_security_logs(context['time_range']))
        
        # Monitoring enhancement
        actions.append(self.enable_enhanced_process_monitoring(context['affected_hosts']))
        actions.append(self.deploy_lsass_protection_monitoring())
        
        return actions
```

---

## Adversary Emulation and Testing

ATT&CK enables realistic adversary emulation that tests defensive capabilities against actual threat actor behaviors.

### Red Team Integration

#### **APT Emulation Framework**

**Threat Actor Profile Mapping**:

| APT Group | Primary Techniques | Emulation Focus | Blue Team Test Objectives |
|-----------|-------------------|-----------------|---------------------------|
| **APT28** | T1566.001, T1059.003, T1003.001 | Spearphishing → PowerShell → Credential harvesting | Email security, endpoint detection, credential protection |
| **APT29** | T1195.002, T1055, T1070.004 | Supply chain → Process injection → Log clearing | Third-party risk, memory protection, log integrity |
| **FIN7** | T1566.002, T1204.002, T1057 | Malicious links → File execution → Process discovery | Web security, execution prevention, behavioral analytics |

#### **Controlled Attack Simulation**

**Emulation Exercise Structure**:

```markdown
## APT Emulation Exercise - [Threat Actor]

### Pre-Exercise Planning (1-2 weeks)
**Scope Definition**:
- [ ] Target systems and networks identified
- [ ] Business impact boundaries established  
- [ ] Success criteria and metrics defined
- [ ] Stakeholder notification procedures confirmed

**Technique Selection**:
- [ ] Representative ATT&CK techniques chosen
- [ ] Progression timeline developed
- [ ] Detection opportunities identified
- [ ] Response triggers documented

### Exercise Execution (1-3 days)
**Phase 1: Initial Access** ([Technique IDs])
- Execute selected initial access techniques
- Monitor blue team detection and response
- Document response effectiveness and timing

**Phase 2: Post-Exploitation** ([Technique IDs])
- Progress through attack chain
- Test defensive capabilities at each stage
- Assess containment and eradication efforts

### Post-Exercise Analysis (1 week)
**Performance Assessment**:
- [ ] Detection coverage gaps identified
- [ ] Response time metrics calculated
- [ ] Communication effectiveness evaluated
- [ ] Process improvement opportunities documented

**Improvement Planning**:
- [ ] Priority enhancement areas identified
- [ ] Resource requirements determined
- [ ] Implementation timeline established
- [ ] Follow-up exercise scheduled
```

---

## Organizational Maturity and Program Development

ATT&CK provides a framework for measuring and advancing organizational defensive maturity.

### ATT&CK Maturity Assessment

#### **Maturity Level Framework**

| Maturity Level | ATT&CK Integration | Capabilities | Metrics |
|----------------|-------------------|--------------|---------|
| **Level 1: Basic** | Technique awareness | Manual technique identification | Incident categorization by tactic |
| **Level 2: Developing** | Systematic technique tracking | Hunt hypotheses based on techniques | Detection coverage by technique |
| **Level 3: Defined** | Automated technique mapping | Technique-driven detection engineering | False positive rates by technique |
| **Level 4: Managed** | Performance measurement | Quantitative coverage analysis | Response effectiveness by technique |
| **Level 5: Optimizing** | Continuous innovation | Predictive threat modeling | Proactive technique mitigation |

#### **Implementation Roadmap**

**Phase 1: Foundation Building (Months 1-3)**
- [ ] **Team Training**: ATT&CK framework education and certification
- [ ] **Tool Deployment**: ATT&CK Navigator setup and configuration  
- [ ] **Historical Analysis**: Map past incidents to ATT&CK techniques
- [ ] **Baseline Assessment**: Current detection coverage evaluation

**Phase 2: Operational Integration (Months 4-6)**
- [ ] **Detection Enhancement**: SIEM rules with ATT&CK mapping
- [ ] **Hunt Program**: ATT&CK-driven threat hunting initiatives
- [ ] **Response Updates**: Incident playbooks with technique-specific procedures
- [ ] **Exercise Integration**: Red team activities using ATT&CK scenarios

**Phase 3: Advanced Capabilities (Months 7-12)**
- [ ] **Automation Implementation**: SOAR playbooks for technique response
- [ ] **Coverage Analytics**: Automated gap analysis and reporting
- [ ] **Threat Modeling**: Business-specific technique risk assessment
- [ ] **Intelligence Integration**: Threat feeds with ATT&CK context

**Phase 4: Continuous Improvement (Month 13+)**
- [ ] **Metric-Driven Enhancement**: Performance-based optimization
- [ ] **Community Contribution**: Knowledge sharing and framework contribution
- [ ] **Advanced Analytics**: Machine learning for technique prediction
- [ ] **Strategic Planning**: Long-term defensive strategy based on ATT&CK insights

### Training and Skill Development

#### **ATT&CK Competency Framework**

**Role-Based Training Paths**:

| Role | Training Focus | Duration | Key Competencies |
|------|---------------|----------|------------------|
| **Analysts** | Technique identification, mapping, hunting | 40 hours | Navigator usage, hunt development, incident mapping |
| **Engineers** | Detection rule development, automation | 60 hours | Rule creation, SOAR integration, coverage analysis |
| **Managers** | Program strategy, metrics, resource planning | 20 hours | Maturity assessment, ROI calculation, strategic planning |
| **Executives** | Business context, risk communication | 8 hours | Framework value, resource justification, strategic alignment |

#### **Practical Exercise Examples**

**Hands-On ATT&CK Workshop**:

```markdown
## Exercise: Advanced Persistent Threat Analysis

**Scenario**: Multi-stage attack on financial services organization
**Duration**: 4 hours
**Participants**: IR team, threat hunters, detection engineers

**Materials Provided**:
- Network packet captures
- Endpoint telemetry data  
- Email headers and attachments
- Authentication logs
- SIEM alert data

**Exercise Tasks**:

### Phase 1: Technique Identification (60 minutes)
1. **Artifact Analysis**: Examine provided evidence
2. **Technique Mapping**: Map observed activities to ATT&CK techniques
3. **Timeline Construction**: Build chronological attack progression
4. **Gap Identification**: Note missing visibility or detection capabilities

### Phase 2: Attack Reconstruction (90 minutes)
1. **Kill Chain Mapping**: Connect techniques into coherent attack flow
2. **Attribution Assessment**: Compare techniques to known threat actors
3. **Impact Analysis**: Evaluate potential business and technical impact
4. **Defense Evaluation**: Assess current defensive posture effectiveness

### Phase 3: Response Planning (90 minutes)
1. **Containment Strategy**: Technique-specific containment approaches
2. **Detection Development**: Create rules for observed techniques
3. **Hunt Query Creation**: Develop proactive hunting procedures
4. **Improvement Planning**: Prioritize defensive enhancements

**Deliverables**:
- ATT&CK technique timeline
- Attack flow visualization
- Detection gap analysis
- Improvement roadmap with priorities
```

---

## Tools and Integration

### Essential ATT&CK Tools

#### **Primary ATT&CK Resources**

| Tool | Purpose | Key Features | Best Practices |
|------|---------|--------------|----------------|
| **ATT&CK Navigator** | Technique visualization and annotation | Heat maps, layer comparison, scoring | Regular updates, team collaboration, coverage tracking |
| **CALDERA** | Automated adversary emulation | Technique execution, campaign simulation | Controlled environments, gradual complexity, blue team coordination |
| **ATT&CK Workbench** | Customizable knowledge base | Local deployment, custom techniques | Organization-specific adaptations, version control |
| **DeTT&CK** | Detection engineering platform | Coverage analysis, rule management | Integration with existing tools, regular assessment |

#### **Integration Implementation**

**SIEM Integration Example**:
```python
class ATTACKSIEMIntegration:
    def __init__(self, siem_connector):
        self.siem = siem_connector
        self.attack_data = self.load_attack_framework()
        
    def enrich_alerts_with_attack(self, alert):
        """Enrich SIEM alerts with ATT&CK context"""
        
        # Map alert indicators to potential techniques
        potential_techniques = self.map_indicators_to_techniques(alert.indicators)
        
        # Enrich with technique metadata
        enriched_context = {}
        for technique in potential_techniques:
            enriched_context[technique.id] = {
                'name': technique.name,
                'tactic': technique.tactic,
                'description': technique.description,
                'mitigation_strategies': technique.mitigations,
                'detection_data_sources': technique.data_sources,
                'threat_actors': self.get_technique_usage_by_actors(technique.id)
            }
        
        # Update alert with ATT&CK enrichment
        enhanced_alert = alert.copy()
        enhanced_alert.attack_context = enriched_context
        enhanced_alert.attack_kill_chain_stage = self.determine_kill_chain_stage(potential_techniques)
        enhanced_alert.attack_severity_modifier = self.calculate_attack_severity(potential_techniques)
        
        return enhanced_alert
    
    def generate_coverage_dashboard(self):
        """Create ATT&CK coverage visualization for SIEM"""
        
        coverage_data = self.analyze_detection_coverage()
        
        dashboard_config = {
            'technique_coverage_heatmap': self.create_coverage_heatmap(coverage_data),
            'tactic_coverage_summary': self.summarize_tactic_coverage(coverage_data),
            'gap_priority_ranking': self.rank_coverage_gaps(coverage_data),
            'improvement_recommendations': self.generate_improvement_suggestions(coverage_data)
        }
        
        return self.siem.create_dashboard('ATT&CK Coverage Analysis', dashboard_config)
```

### Performance Measurement

#### **ATT&CK Program Metrics**

**Quantitative Assessment Framework**:

```python
class ATTACKProgramMetrics:
    def __init__(self):
        self.metrics_categories = {
            'coverage': self.calculate_technique_coverage,
            'effectiveness': self.measure_detection_effectiveness,
            'maturity': self.assess_program_maturity,
            'performance': self.analyze_response_performance
        }
    
    def generate_comprehensive_assessment(self, assessment_period):
        """Generate comprehensive ATT&CK program assessment"""
        
        assessment_results = {}
        
        # Calculate coverage metrics
        coverage_metrics = {
            'total_techniques_covered': self.count_covered_techniques(),
            'coverage_percentage': self.calculate_coverage_percentage(),
            'high_priority_technique_coverage': self.assess_priority_coverage(),
            'detection_method_distribution': self.analyze_detection_methods()
        }
        
        # Measure detection effectiveness
        effectiveness_metrics = {
            'true_positive_rate': self.calculate_true_positive_rate(assessment_period),
            'false_positive_rate': self.calculate_false_positive_rate(assessment_period),
            'detection_accuracy': self.calculate_detection_accuracy(assessment_period),
            'response_time_by_technique': self.analyze_response_times(assessment_period)
        }
        
        # Assess program maturity
        maturity_assessment = {
            'current_maturity_level': self.determine_maturity_level(),
            'maturity_progression': self.track_maturity_progression(),
            'capability_gaps': self.identify_capability_gaps(),
            'improvement_recommendations': self.generate_improvement_plan()
        }
        
        return {
            'coverage_metrics': coverage_metrics,
            'effectiveness_metrics': effectiveness_metrics,
            'maturity_assessment': maturity_assessment,
            'overall_score': self.calculate_overall_program_score(),
            'benchmark_comparison': self.compare_to_industry_benchmarks(),
            'executive_summary': self.create_executive_summary(assessment_results)
        }
```

**Key Performance Indicators Dashboard**:

```
ATT&CK Program Performance Dashboard

┌─────────────────────────┬─────────┬─────────┬─────────┐
│ Metric                  │ Current │ Target  │ Trend   │
├─────────────────────────┼─────────┼─────────┼─────────┤
│ Technique Coverage      │ 67%     │ > 80%   │ ↑       │
├─────────────────────────┼─────────┼─────────┼─────────┤
│ High-Priority Coverage  │ 85%     │ > 90%   │ →       │
├─────────────────────────┼─────────┼─────────┼─────────┤
│ Detection Accuracy      │ 92%     │ > 95%   │ ↑       │
├─────────────────────────┼─────────┼─────────┼─────────┤
│ False Positive Rate     │ 8%      │ < 5%    │ ↓       │
├─────────────────────────┼─────────┼─────────┼─────────┤
│ Avg Response Time       │ 24 min  │ < 15min │ ↓       │
├─────────────────────────┼─────────┼─────────┼─────────┤
│ Hunt Success Rate       │ 12%     │ > 15%   │ ↑       │
└─────────────────────────┴─────────┴─────────┴─────────┘
```

[⬆️ Back to Incident Response](./README.md)
