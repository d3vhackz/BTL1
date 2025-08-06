# 6.2: NIST Incident Response Lifecycle

The NIST SP 800-61r2 framework provides the industry-standard approach to incident response. This four-phase lifecycle ensures systematic, repeatable, and effective incident handling.

---

## The Four-Phase Framework

```mermaid
graph LR
    A[Preparation] --> B[Detection & Analysis]
    B --> C[Containment, Eradication & Recovery]
    C --> D[Lessons Learned]
    D --> A
    
    style A fill:#e1f5fe
    style B fill:#fff3e0
    style C fill:#fce4ec
    style D fill:#f3e5f5
```

> **Note**: This is a **continuous cycle** - each phase feeds into the next, creating an ongoing improvement process.

---

## Phase 1: Preparation 🛡️

**The most critical phase** - sets the foundation for effective incident response.

### Key Activities

#### 🎯 **Incident Readiness**
- **Contact information** for all stakeholders maintained and current
- **War room establishment** for central communication and coordination  
- **Comprehensive documentation** of procedures and escalation paths
- **System baselines** documented for "normal" operational states
- **Equipment procurement** including digital forensic toolkits

#### 🛡️ **Proactive Prevention**
- **Current risk assessments** identifying organizational vulnerabilities
- **Client and server security** controls implementation
- **User awareness training** programs with regular updates
- **Security control deployment** across all critical assets

### Preparation Success Metrics
- Time to activate IR team (target: < 30 minutes)
- Stakeholder notification speed (target: < 1 hour)
- Equipment availability (target: 100% ready state)
- Team training currency (target: quarterly exercises)

---

## Phase 2: Detection & Analysis 🔍

**Two distinct sub-phases** that bridge incident identification with response actions.

### Detection Sub-Phase

#### Automated Detection Systems
| System Type | Primary Function | Alert Generation |
|------------|------------------|------------------|
| **IDPS** | Network/host intrusion monitoring | Real-time threat alerts |
| **SIEM** | Log correlation and analysis | Pattern-based alerting |
| **Anti-malware** | Endpoint threat detection | Signature/behavior alerts |
| **EDR** | Advanced endpoint monitoring | Behavioral anomaly alerts |

#### Manual Detection Sources
- **User reports** of suspicious activity
- **System administrator** observations
- **External notifications** (law enforcement, partners)
- **Threat intelligence** feeds and warnings

### Analysis Sub-Phase

#### Core Analysis Activities
- **Initial attack vector** identification and documentation
- **Lateral movement** tracking through network and systems
- **Impact assessment** of affected systems and data
- **Evidence collection** using forensically sound methods

#### Critical Documentation Requirements
- **Timeline reconstruction** of attacker activities
- **Affected systems inventory** with detailed impact assessment
- **IOC compilation** for threat hunting and sharing
- **Attack methodology** mapping to MITRE ATT&CK framework

### Communication Plan Activation

#### Internal Notifications
- **Management chain** (immediate supervisors to C-suite)
- **IT leadership** for technical coordination
- **Human Resources** for personnel-related incidents
- **Legal team** for compliance and litigation concerns

#### External Notifications (as required)
- **Law enforcement** for criminal activity
- **Regulatory bodies** for compliance violations
- **Customers/partners** for service impacts
- **Media/public** through designated spokespersons

---

## Phase 3: Containment, Eradication & Recovery 🚨

**Three integrated sub-phases** focused on stopping damage and restoring operations.

### Containment Strategy Selection

#### NIST Containment Criteria
| Criterion | Consideration | Impact on Strategy |
|-----------|---------------|-------------------|
| **Potential Damage** | Scope of possible harm | Aggressive vs conservative approach |
| **Evidence Preservation** | Legal/forensic requirements | Live analysis vs system shutdown |
| **Service Availability** | Business continuity needs | Immediate isolation vs gradual containment |
| **Resource Requirements** | Time and personnel costs | Simple vs complex containment |
| **Solution Effectiveness** | Likelihood of success | Primary vs backup strategies |
| **Duration** | Temporary vs permanent fix | Short-term vs long-term planning |

#### Containment Approaches

**Short-term Containment**:
- Network isolation or segmentation
- User account disabling
- Service shutdown
- Memory preservation for analysis

**Long-term Containment**:
- System reimaging and rebuilding
- Network architecture changes
- Policy and procedure updates
- Enhanced monitoring deployment

### Eradication & Recovery

#### Eradication Activities
- **Malware removal** using specialized tools
- **Backdoor elimination** and access path closure  
- **Credential reset** for all potentially compromised accounts
- **Vulnerability patching** that enabled initial compromise
- **Configuration hardening** to prevent reoccurrence

#### Recovery Activities
- **System restoration** from known-good backups
- **Service restoration** with enhanced monitoring
- **User access restoration** with additional verification
- **Network security** enhancement and validation

### Evidence Documentation
> **Critical Reminder**: All containment, eradication, and recovery activities must be thoroughly documented for potential legal proceedings and lessons learned analysis.

---

## Phase 4: Lessons Learned 📚

**The most overlooked but valuable phase** - transforms incidents into organizational improvement.

### Post-Incident Meeting Framework

#### Key Questions to Address
1. **Timeline Analysis**: Exactly what happened and when?
2. **Performance Assessment**: How well did staff and management respond?
3. **Information Gaps**: What information was needed sooner?
4. **Process Hindrances**: What actions may have inhibited recovery?
5. **Future Improvements**: What would be done differently next time?
6. **Information Sharing**: How could external collaboration be improved?
7. **Corrective Actions**: What specific improvements can be implemented?
8. **Monitoring Enhancement**: What indicators should be watched for?
9. **Resource Needs**: What additional tools or resources are required?

### Implementation and Feedback Loop

#### Documentation Requirements
- **Detailed incident report** with timeline and impact analysis
- **Response effectiveness** assessment with metrics
- **Improvement recommendations** with priority rankings
- **Resource requirements** for recommended changes

#### Continuous Improvement
- **Process updates** based on lessons learned
- **Training modifications** to address identified gaps
- **Technology enhancements** to improve detection/response
- **Team structure adjustments** for better coordination

---

## Lifecycle Integration Examples

### Example 1: Ransomware Incident
1. **Preparation**: IR plan includes ransomware-specific procedures
2. **Detection**: EDR alerts on file encryption activities
3. **Analysis**: Scope assessment reveals 50 affected workstations
4. **Containment**: Network isolation prevents spread to servers
5. **Eradication**: Malware removal and vulnerability patching
6. **Recovery**: Restore from backups with enhanced monitoring
7. **Lessons Learned**: Update backup procedures and user training

### Example 2: Phishing Campaign
1. **Preparation**: Email security controls and user training in place
2. **Detection**: User reports suspicious email to security team
3. **Analysis**: Email analysis reveals credential harvesting attempt
4. **Containment**: Block malicious domains and reset credentials
5. **Eradication**: Remove emails from all mailboxes
6. **Recovery**: Restore normal email operations with enhanced filtering
7. **Lessons Learned**: Improve phishing training and email controls

---

## Success Metrics

### Response Time Metrics
- **Detection to acknowledgment**: < 15 minutes
- **Analysis completion**: < 2 hours for initial assessment
- **Containment initiation**: < 1 hour from analysis completion
- **Recovery completion**: Varies by incident scope

### Quality Metrics  
- **False positive rate**: < 5% for automated alerts
- **Escalation accuracy**: > 95% appropriate escalations
- **Documentation completeness**: 100% of required elements
- **Stakeholder satisfaction**: > 90% positive feedback

The NIST lifecycle provides a proven framework, but success depends on thorough preparation and continuous improvement through lessons learned.

[⬆️ Back to Incident Response](./README.md)
