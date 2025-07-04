# 5.4: Splunk SIEM Platform Fundamentals

Splunk represents the industry standard for SIEM platforms, providing comprehensive log analysis, real-time monitoring, and security operations capabilities. This section covers essential Splunk navigation, search techniques, and operational workflows for security analysts.

---

## Splunk Architecture and Navigation

### Platform Overview

**Splunk** serves as both a data platform and security analytics engine, enabling organizations to collect, index, search, and analyze machine-generated data at scale.

#### Core Capabilities
- **Universal data ingestion** from any machine-generated source
- **Real-time search and analysis** across petabytes of data
- **Machine learning** and behavioral analytics
- **Extensible app ecosystem** for specialized use cases
- **RESTful API** for integration and automation

### User Interface Components

#### Primary Interface Elements

| Component | Location | Function |
|-----------|----------|----------|
| **Apps Panel** | Left sidebar | Navigate between installed applications |
| **Splunk Bar** | Top navigation | System-wide controls and job monitoring |
| **Explore Splunk Panel** | Main area | Quick access to help and documentation |
| **Home Dashboard** | Configurable center | Customizable information display |

#### App-Based Architecture

**Search & Reporting App** (Default):
- **Primary interface** for data analysis and investigation
- **Search capabilities** with SPL (Search Processing Language)
- **Report creation** and dashboard development
- **Alert configuration** and management

**Specialized Apps** (Additional):
- **Enterprise Security (ES)**: Advanced SIEM functionality
- **IT Service Intelligence (ITSI)**: Service monitoring and analytics
- **User Behavior Analytics (UBA)**: Behavioral anomaly detection
- **Infrastructure Monitoring**: System performance analysis

---

## Search Fundamentals and Data Discovery

### Index-Based Data Organization

**Indexes** serve as data containers that organize and store ingested logs for efficient searching and analysis.

#### Basic Search Structure
```spl
index="<index_name>" earliest=<time> latest=<time>
```

**Example**:
```spl
index="botsv1" earliest=0
```
- **index="botsv1"**: Search within the BOTSv1 dataset
- **earliest=0**: Start from the first event in the dataset
- **Alternative**: `index=*` for searching across all available indexes

### Field Discovery and Analysis

#### Field Categories in Splunk

**Selected Fields** (Always displayed):
- **host**: System or device generating the log
- **source**: Specific log file or data source
- **sourcetype**: Data classification and parsing rules

**Interesting Fields** (Dynamically identified):
- **Fields with significant values** across the current search results
- **Automatically extracted** from raw log data
- **Clickable for rapid filtering** and analysis

#### Field Exploration Workflow

```spl
# Step 1: Basic index search
index="botsv1" earliest=0

# Step 2: Examine field distributions
# Click on fields in the sidebar to see value counts

# Step 3: Filter by specific field values
index="botsv1" earliest=0 sourcetype="wineventlog"
```

#### Practical Field Analysis Example

**Network Traffic Investigation**:
```spl
index="botsv1" sourcetype="fgt_traffic"
```

**Key Fields Identified**:
- **srcip**: Source IP address
- **dstip**: Destination IP address  
- **srcport**: Source port number
- **dstport**: Destination port number
- **action**: Allow/deny decision

### Sampling for Performance

**Event Sampling Benefits**:
- **Faster query execution** during rule development
- **Resource conservation** for large datasets
- **Rapid prototyping** of search logic

**Sampling Configuration**:
- **Location**: Below search bar, left side
- **Options**: No sampling, 1:10, 1:100, 1:1000
- **Usage**: Enable during development, disable for production searches

---

## Advanced Search Techniques

### Field and Value Pair Searches

#### Basic Search Patterns

**Single Field Search**:
```spl
index="botsv1" src="10.10.10.50"
```

**Multiple Field Search with Operators**:
```spl
index="botsv1" (src="10.10.10.50" OR dst="10.10.10.50")
```

**Complex Scenario Example**:
```spl
# DDoS Investigation: Traffic to web server
index="botsv1" dst="10.10.100.5" sourcetype="fgt_traffic"
```

#### Logical Operators

| Operator | Function | Example |
|----------|----------|---------|
| **AND** | Both conditions must be true | `src="192.168.1.1" AND dst="10.0.0.1"` |
| **OR** | Either condition can be true | `action="allow" OR action="deny"` |
| **NOT** | Excludes matching results | `NOT src="192.168.1.1"` |

### Wildcard Usage

#### Wildcard Applications

**IP Address Ranges**:
```spl
# All traffic from 10.10.10.x subnet
src="10.10.10.*"

# Lateral movement detection
src="10.10.10.73" dst="10.10.10.*"
```

**Text Pattern Matching**:
```spl
# Password-related failures
index="botsv1" pass* AND fail*
```

**Results Include**:
- "password" + "failure"
- "pass" + "fail"  
- "password" + "fail"
- "pass" + "failure"

### Process Analysis with Sysmon

#### Sysmon Integration

**Sysmon Event ID 1** (Process Creation):
```spl
index="botsv1" earliest=0 Image="*\\cmd.exe" 
| stats values(CommandLine) by host
```

**Query Breakdown**:
- **Image**: Executable path (Sysmon field)
- **CommandLine**: Full command with arguments
- **stats values()**: Unique command line values
- **by host**: Grouped by hostname

**Investigative Value**:
- **Command execution** timeline reconstruction
- **Attack technique** identification
- **Lateral movement** pattern analysis
- **Persistence mechanism** detection

---

## Search Commands and Data Manipulation

### Essential Search Commands

#### Sort Command

**Purpose**: Order results by specified field values

**Syntax**:
```spl
| sort [limit=<number>] <field> [asc|desc]
```

**Examples**:
```spl
# Chronological ordering
| sort time asc

# Top results with limit
| sort limit=10 count desc
```

#### Stats Command

**Purpose**: Generate statistical summaries and aggregations

**Common Patterns**:
```spl
# Count occurrences by field
| stats count by srcip

# Multiple statistics
| stats count, avg(bytes), max(duration) by host

# Combined with sort for top talkers
| stats count by srcip | sort count desc
```

#### Table Command

**Purpose**: Create custom field displays and hide irrelevant data

**Usage**:
```spl
| table date, time, srcip, dstport, action, msg
```

**Benefits**:
- **Focused analysis** on relevant fields only
- **Improved readability** for large datasets
- **Column-based filtering** capabilities
- **Export-friendly** format for reporting

#### Deduplication Commands

**Unique Values**:
```spl
| table srcip | uniq
```

**Alternative Deduplication**:
```spl
| table action | dedup action
```

**Use Cases**:
- **Asset enumeration** (unique IP addresses)
- **Service identification** (unique ports/protocols)
- **Attack vector analysis** (unique attack patterns)

---

## Alert Development and Management

### Alert Architecture

**Alert Components**:
1. **Search Query**: Defines what to detect
2. **Search Timing**: When to execute the search
3. **Alert Triggers**: Conditions that generate alerts
4. **Alert Actions**: Response when triggered

### Alert Configuration Process

#### 1. Search Query Development

**Example**: Failed Authentication Detection
```spl
index="security" EventCode=4625 
| stats count by Account_Name 
| where count > 5
```

#### 2. Search Timing Options

**Real-time Alerts**:
- **Continuous execution** for immediate detection
- **Higher resource consumption** but faster response
- **Ideal for**: Critical security events, active threats

**Scheduled Alerts**:
- **Periodic execution** (hourly, daily, weekly)
- **Resource efficient** for trend analysis
- **Ideal for**: Baseline monitoring, compliance reporting

#### 3. Alert Trigger Configuration

**Threshold Examples**:

| Scenario | Threshold Configuration | Rationale |
|----------|------------------------|-----------|
| **Brute Force** | 6 failed logins in 5 minutes | Account normal typos vs. attack |
| **Data Exfiltration** | >1GB upload in 1 hour | Normal usage vs. bulk transfer |
| **Port Scanning** | >100 unique ports in 10 minutes | Service discovery vs. scanning |

#### 4. Alert Actions

**Standard Actions**:
- **Email notifications** for high-priority events
- **Alert listing** for analyst queue management
- **Event logging** for audit and correlation
- **Custom webhooks** for integration with external systems

### Alert Creation Workflow

```spl
# Step 1: Develop and test search query
index="security" EventCode=4625 
| stats count by Account_Name, src_ip 
| where count > 6

# Step 2: Save as Alert
# Click "Save As" → "Alert"

# Step 3: Configure alert properties
# - Title: "Multiple Failed Logins Detected"
# - Description: "Detects potential brute force attacks"
# - Permissions: Private or Shared in App
# - Type: Real-time or Scheduled
# - Triggers: Custom thresholds
# - Actions: Email + Alert listing
```

---

## Dashboard Development

### Dashboard Concepts

**Dashboard Purpose**:
- **Single pane of glass** for security operations
- **Visual representation** of key metrics and trends
- **Executive reporting** and stakeholder communication
- **Operational awareness** for SOC analysts

#### Common SOC Dashboard Panels

| Panel Type | Metrics | Operational Value |
|------------|---------|------------------|
| **Firewall Activity** | Allows vs. denies over time | DDoS detection, network issues |
| **Alert Volume** | Current alerts by severity | Workload management |
| **Investigation Metrics** | Cases closed in 24 hours | Team efficiency tracking |
| **Data Collection** | Log volume by source | Infrastructure health monitoring |
| **Threat Geography** | Attack sources by location | Threat landscape awareness |
| **Attack Categories** | Event types and frequencies | Threat trend analysis |

### Dashboard Creation Process

#### Step 1: Report Development

**Reports** serve as the foundation for dashboard panels.

**Report Creation**:
```spl
# Example: HTTP Error Monitoring
index="web" status!=200 
| stats count by status 
| sort status
```

**Naming Convention**:
```
<group>_<object>_<description>
Examples:
- SOC_report_HttpErrors
- Security_dashboard_LoginFailures  
- Network_report_TopTalkers
```

#### Step 2: Dashboard Assembly

**Dashboard Creation Workflow**:
1. **Navigate to saved report**
2. **Click "Add to Dashboard"**
3. **Configure dashboard properties**:
   - Dashboard Title: "Security Operations Dashboard"
   - Dashboard ID: "sec_ops_main"
   - Dashboard Description: "Primary SOC monitoring dashboard"
   - Permissions: Private (initially) → Shared after testing
4. **Configure panel properties**:
   - Panel Title: "HTTP Response Errors"
   - Panel Type: Line chart, bar chart, single value, etc.

#### Step 3: Dashboard Enhancement

**Multi-Panel Dashboard Example**:
```
Security Operations Dashboard
├── Login Failures (Line Chart)
├── Firewall Denies (Area Chart)  
├── Top Attacking IPs (Table)
├── Alert Volume by Severity (Bar Chart)
└── Data Ingestion Health (Single Value)
```

**Advanced Features**:
- **Time range selectors** for dynamic analysis
- **Drill-down capabilities** for detailed investigation
- **Auto-refresh** for real-time monitoring
- **Dashboard sharing** with role-based access

### Home Dashboard Configuration

**Setting Default Dashboard**:
1. Navigate to Home app
2. Select "Choose a home dashboard"
3. Select desired dashboard for default display
4. Dashboard appears automatically upon login

This provides **immediate situational awareness** for analysts and reduces time-to-detection for critical events.

[⬆️ Back to SIEM & Monitoring](./README.md)
