# 5.8: Data Normalization and Enrichment

Data normalization and enrichment transform raw, heterogeneous log data into a standardized, analytically valuable format optimized for security correlation and investigation. This chapter explores the methodologies, technologies, and best practices for creating consistent, enriched data sets that maximize SIEM effectiveness and analyst productivity.

---

## Data Normalization Fundamentals

### Understanding Normalization Requirements

**Data Normalization** is the process of transforming log data from diverse sources into a consistent, standardized format that enables cross-platform correlation, unified querying, and efficient analysis across the entire security infrastructure.

#### The Heterogeneous Data Challenge

**Source Diversity Examples**:
```
Cisco ASA Firewall:
%ASA-6-302013: Built outbound TCP connection 12345 for outside:192.168.1.100/52341 to inside:10.0.0.1/80

Juniper SRX Firewall:
RT_FLOW - RT_FLOW_SESSION_CREATE [source: 192.168.1.100] [destination: 10.0.0.1] [protocol: TCP] [service: 80]

Windows Security Event:
<Event><s><EventID>5156</EventID></s><EventData><Data Name="SourceAddress">192.168.1.100</Data><Data Name="DestinationAddress">10.0.0.1</Data></EventData></Event>

Linux IPTables:
Oct 15 09:30:45 gateway kernel: [iptables] IN=eth0 OUT=eth1 SRC=192.168.1.100 DST=10.0.0.1 PROTO=TCP SPT=52341 DPT=80
```

**Normalization Objectives**:
| Challenge | Normalization Solution | Business Value |
|-----------|----------------------|----------------|
| **Field Name Variations** | Standardized field mapping | Universal search syntax across all sources |
| **Data Type Inconsistencies** | Type conversion and validation | Reliable numerical and temporal operations |
| **Format Differences** | Unified structure and encoding | Consistent parsing and processing |
| **Missing Context** | Default value assignment | Complete data sets for analysis |

#### Normalization Benefits

**Operational Advantages**:
- **Universal search capability** using consistent field names across all log sources
- **Cross-platform correlation** enabling detection of multi-stage attacks
- **Simplified rule development** with predictable data structures and field names
- **Improved analyst productivity** through familiar, consistent data presentation

**Technical Benefits**:
- **Query optimization** through consistent data types and indexing strategies
- **Storage efficiency** via standardized compression and deduplication techniques
- **Processing performance** using optimized parsers and transformation pipelines
- **Integration simplification** with downstream analysis and reporting tools

### Field Mapping and Standardization

#### Common Field Taxonomy

**Network Communication Fields**:
```
Source System Variations → Normalized Field Name
├── Cisco ASA: outside:192.168.1.100/52341 → src_ip: "192.168.1.100", src_port: 52341
├── Juniper SRX: [source: 192.168.1.100] → src_ip: "192.168.1.100"
├── Windows: SourceAddress → src_ip: "192.168.1.100"
├── Linux: SRC=192.168.1.100 → src_ip: "192.168.1.100"
└── pfSense: 192.168.1.100:52341 → src_ip: "192.168.1.100", src_port: 52341
```

**Temporal Information Standardization**:
```
Timestamp Variations → Normalized Format (ISO 8601 UTC)
├── Cisco: Oct 15 09:30:45 → 2023-10-15T09:30:45.000Z
├── Windows: 2023-10-15T09:30:45.123456 → 2023-10-15T09:30:45.123Z
├── Apache: 15/Oct/2023:09:30:45 +0000 → 2023-10-15T09:30:45.000Z
├── Syslog: Oct 15 09:30:45 → 2023-10-15T09:30:45.000Z
└── Unix: 1697356245 → 2023-10-15T09:30:45.000Z
```

**User and Authentication Standardization**:
```
User Identity Variations → Normalized Fields
├── Windows: DOMAIN\username → user: "username", domain: "DOMAIN"
├── Linux: username → user: "username", domain: "localhost"
├── LDAP: cn=John Smith,ou=users,dc=company,dc=com → user: "jsmith", full_name: "John Smith"
├── Email: john.smith@company.com → user: "john.smith", domain: "company.com"
└── Certificate: CN=John Smith/emailAddress=john.smith@company.com → user: "john.smith"
```

#### Normalization Implementation Strategies

**Rule-Based Mapping**:
```python
# Field normalization configuration
FIELD_MAPPING_RULES = {
    'cisco_asa': {
        'src_ip_pattern': r'for\s+\w+:(\d+\.\d+\.\d+\.\d+)/(\d+)',
        'dst_ip_pattern': r'to\s+\w+:(\d+\.\d+\.\d+\.\d+)/(\d+)',
        'action_pattern': r'%ASA-\d+-(\d+):',
        'field_mapping': {
            'src_ip': lambda m: m.group(1) if m else None,
            'src_port': lambda m: int(m.group(2)) if m else None,
            'dst_ip': lambda m: m.group(1) if m else None,
            'dst_port': lambda m: int(m.group(2)) if m else None
        }
    },
    'windows_security': {
        'xml_fields': {
            'SourceAddress': 'src_ip',
            'DestinationAddress': 'dst_ip',
            'SourcePort': 'src_port',
            'DestinationPort': 'dst_port',
            'TargetUserName': 'user',
            'TargetDomainName': 'domain'
        }
    },
    'linux_iptables': {
        'kv_pattern': r'(\w+)=([^\s]+)',
        'field_mapping': {
            'SRC': 'src_ip',
            'DST': 'dst_ip',
            'SPT': 'src_port',
            'DPT': 'dst_port',
            'PROTO': 'protocol'
        }
    }
}

def normalize_event(raw_event, source_type):
    """Normalize event based on source type and mapping rules"""
    normalized = {
        'timestamp': None,
        'src_ip': None,
        'dst_ip': None,
        'src_port': None,
        'dst_port': None,
        'protocol': None,
        'user': None,
        'domain': None,
        'action': None,
        'message': raw_event.get('message', ''),
        'source_type': source_type,
        'raw_event': raw_event
    }
    
    if source_type in FIELD_MAPPING_RULES:
        rules = FIELD_MAPPING_RULES[source_type]
        
        # Apply pattern-based extraction
        for pattern_name, pattern in rules.items():
            if pattern_name.endswith('_pattern'):
                field_name = pattern_name.replace('_pattern', '')
                match = re.search(pattern, raw_event.get('message', ''))
                if match and field_name in rules.get('field_mapping', {}):
                    mapping_func = rules['field_mapping'][field_name]
                    normalized[field_name] = mapping_func(match)
        
        # Apply XML field mapping for Windows events
        if 'xml_fields' in rules and 'event_data' in raw_event:
            for xml_field, norm_field in rules['xml_fields'].items():
                if xml_field in raw_event['event_data']:
                    normalized[norm_field] = raw_event['event_data'][xml_field]
        
        # Apply key-value mapping for syslog
        if 'kv_pattern' in rules:
            kv_matches = re.findall(rules['kv_pattern'], raw_event.get('message', ''))
            for key, value in kv_matches:
                if key in rules.get('field_mapping', {}):
                    norm_field = rules['field_mapping'][key]
                    normalized[norm_field] = value
    
    # Standardize timestamp
    normalized['timestamp'] = standardize_timestamp(raw_event.get('timestamp'))
    
    # Validate and type convert
    normalized = validate_and_convert_types(normalized)
    
    return normalized
```

**Statistical Learning Approach**:
```python
# ML-based field mapping using clustering
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.cluster import KMeans
import numpy as np

class FieldMappingLearner:
    def __init__(self):
        self.vectorizer = TfidfVectorizer(ngram_range=(1, 3), max_features=1000)
        self.field_clusters = {}
        
    def learn_field_mappings(self, training_events):
        """Learn field mappings from labeled training data"""
        
        # Extract field names and values from training events
        field_examples = {}
        for event in training_events:
            for field_name, field_value in event.items():
                if field_name not in field_examples:
                    field_examples[field_name] = []
                field_examples[field_name].append(str(field_value))
        
        # Cluster similar field patterns
        for norm_field in ['src_ip', 'dst_ip', 'user', 'timestamp']:
            if norm_field in field_examples:
                # Vectorize field values
                vectors = self.vectorizer.fit_transform(field_examples[norm_field])
                
                # Cluster to find patterns
                kmeans = KMeans(n_clusters=3, random_state=42)
                clusters = kmeans.fit_predict(vectors)
                
                # Store cluster centers for classification
                self.field_clusters[norm_field] = {
                    'vectorizer': self.vectorizer,
                    'kmeans': kmeans,
                    'examples': field_examples[norm_field]
                }
    
    def suggest_field_mapping(self, unknown_field_value):
        """Suggest normalized field name for unknown field value"""
        suggestions = {}
        
        for norm_field, cluster_info in self.field_clusters.items():
            # Transform unknown value using stored vectorizer
            vector = cluster_info['vectorizer'].transform([str(unknown_field_value)])
            
            # Calculate similarity to cluster centers
            similarity = cluster_info['kmeans'].transform(vector).min()
            suggestions[norm_field] = 1.0 / (1.0 + similarity)  # Convert distance to similarity
        
        # Return field with highest similarity
        return max(suggestions, key=suggestions.get) if suggestions else None
```

---

## Advanced Normalization Techniques

### Semantic Normalization

#### Context-Aware Field Interpretation

**Multi-Context Field Resolution**:
```python
# Context-aware normalization
class ContextAwareNormalizer:
    def __init__(self):
        self.context_rules = {
            'network_context': {
                'patterns': [r'connection', r'traffic', r'flow', r'packet'],
                'field_priorities': {
                    'ip_address': ['src_ip', 'dst_ip', 'client_ip', 'server_ip'],
                    'port': ['src_port', 'dst_port', 'local_port', 'remote_port'],
                    'protocol': ['protocol', 'proto', 'ip_proto']
                }
            },
            'authentication_context': {
                'patterns': [r'login', r'auth', r'logon', r'credential'],
                'field_priorities': {
                    'user': ['user', 'username', 'account', 'subject_user'],
                    'domain': ['domain', 'realm', 'authority'],
                    'source': ['src_ip', 'client_ip', 'workstation', 'source_address']
                }
            },
            'file_context': {
                'patterns': [r'file', r'document', r'download', r'upload'],
                'field_priorities': {
                    'filename': ['filename', 'file_name', 'document', 'path'],
                    'file_size': ['size', 'bytes', 'length'],
                    'file_hash': ['hash', 'md5', 'sha1', 'sha256']
                }
            }
        }
    
    def determine_context(self, raw_event):
        """Determine event context from message content"""
        message = raw_event.get('message', '').lower()
        
        for context_name, context_info in self.context_rules.items():
            for pattern in context_info['patterns']:
                if re.search(pattern, message):
                    return context_name
        
        return 'generic_context'
    
    def normalize_with_context(self, raw_event):
        """Apply context-aware normalization"""
        context = self.determine_context(raw_event)
        normalized = {}
        
        if context in self.context_rules:
            context_rules = self.context_rules[context]
            
            # Apply field priorities based on context
            for norm_field, candidate_fields in context_rules['field_priorities'].items():
                for candidate in candidate_fields:
                    if candidate in raw_event and raw_event[candidate]:
                        normalized[norm_field] = raw_event[candidate]
                        break  # Use first matching field with value
        
        # Add context information
        normalized['event_context'] = context
        normalized['confidence_score'] = self.calculate_confidence(raw_event, context)
        
        return normalized
    
    def calculate_confidence(self, raw_event, context):
        """Calculate confidence score for context determination"""
        message = raw_event.get('message', '').lower()
        pattern_matches = 0
        
        if context in self.context_rules:
            for pattern in self.context_rules[context]['patterns']:
                if re.search(pattern, message):
                    pattern_matches += 1
        
        # Simple confidence based on pattern matches
        return min(pattern_matches / len(self.context_rules[context]['patterns']), 1.0)
```

#### Hierarchical Field Mapping

**Inheritance-Based Normalization**:
```python
# Hierarchical field mapping with inheritance
class HierarchicalFieldMapper:
    def __init__(self):
        self.mapping_hierarchy = {
            'base': {
                'timestamp': ['timestamp', 'time', 'datetime', '@timestamp'],
                'message': ['message', 'msg', 'description', 'event_description'],
                'severity': ['severity', 'level', 'priority', 'criticality'],
                'host': ['host', 'hostname', 'computer', 'system']
            },
            'network': {
                'inherits': 'base',
                'src_ip': ['src_ip', 'source_ip', 'client_ip', 'srcaddr'],
                'dst_ip': ['dst_ip', 'dest_ip', 'destination_ip', 'server_ip', 'dstaddr'],
                'src_port': ['src_port', 'source_port', 'client_port', 'sport'],
                'dst_port': ['dst_port', 'dest_port', 'destination_port', 'server_port', 'dport'],
                'protocol': ['protocol', 'proto', 'ip_protocol']
            },
            'authentication': {
                'inherits': 'network',
                'user': ['user', 'username', 'account_name', 'subject_user', 'target_user'],
                'domain': ['domain', 'realm', 'authority', 'subject_domain'],
                'auth_method': ['auth_method', 'authentication_type', 'logon_type'],
                'auth_result': ['result', 'status', 'outcome', 'success']
            },
            'firewall': {
                'inherits': 'network',
                'action': ['action', 'verdict', 'decision', 'disposition'],
                'rule': ['rule', 'rule_name', 'policy', 'acl'],
                'interface': ['interface', 'if', 'zone']
            }
        }
    
    def get_merged_mapping(self, category):
        """Get complete field mapping including inherited fields"""
        mapping = {}
        
        # Traverse inheritance chain
        current_category = category
        inheritance_chain = []
        
        while current_category:
            inheritance_chain.insert(0, current_category)
            parent = self.mapping_hierarchy.get(current_category, {}).get('inherits')
            current_category = parent
        
        # Merge mappings from base to specific
        for cat in inheritance_chain:
            if cat in self.mapping_hierarchy:
                cat_mapping = {k: v for k, v in self.mapping_hierarchy[cat].items() 
                              if k != 'inherits'}
                mapping.update(cat_mapping)
        
        return mapping
    
    def normalize_event(self, raw_event, category='base'):
        """Normalize event using hierarchical mapping"""
        mapping = self.get_merged_mapping(category)
        normalized = {}
        
        for norm_field, candidate_fields in mapping.items():
            for candidate in candidate_fields:
                if candidate in raw_event and raw_event[candidate] is not None:
                    normalized[norm_field] = raw_event[candidate]
                    break
        
        # Add metadata
        normalized['normalization_category'] = category
        normalized['fields_mapped'] = len([f for f in normalized.values() if f is not None])
        normalized['original_fields'] = len(raw_event)
        
        return normalized
```

### Data Type Standardization

#### Type Conversion and Validation

**Robust Type Conversion**:
```python
# Comprehensive data type standardization
import ipaddress
from datetime import datetime
import re

class DataTypeStandardizer:
    def __init__(self):
        self.type_validators = {
            'ip_address': self._validate_ip_address,
            'port': self._validate_port,
            'timestamp': self._validate_timestamp,
            'mac_address': self._validate_mac_address,
            'hash': self._validate_hash,
            'url': self._validate_url,
            'email': self._validate_email
        }
        
        self.type_converters = {
            'ip_address': self._convert_ip_address,
            'port': self._convert_port,
            'timestamp': self._convert_timestamp,
            'mac_address': self._convert_mac_address,
            'hash': self._convert_hash
        }
    
    def standardize_field(self, field_name, field_value, expected_type=None):
        """Standardize field value based on expected type"""
        if field_value is None or field_value == '':
            return None
        
        # Auto-detect type if not specified
        if expected_type is None:
            expected_type = self._detect_field_type(field_name, field_value)
        
        # Validate and convert
        if expected_type in self.type_validators:
            if self.type_validators[expected_type](field_value):
                if expected_type in self.type_converters:
                    return self.type_converters[expected_type](field_value)
                return field_value
            else:
                return None  # Invalid data
        
        return str(field_value)  # Default to string
    
    def _detect_field_type(self, field_name, field_value):
        """Auto-detect field type based on name and value patterns"""
        field_name_lower = field_name.lower()
        value_str = str(field_value)
        
        # IP address detection
        if any(term in field_name_lower for term in ['ip', 'addr', 'address']):
            if self._validate_ip_address(value_str):
                return 'ip_address'
        
        # Port detection
        if any(term in field_name_lower for term in ['port', 'spt', 'dpt']):
            if self._validate_port(value_str):
                return 'port'
        
        # Timestamp detection
        if any(term in field_name_lower for term in ['time', 'date', 'stamp']):
            if self._validate_timestamp(value_str):
                return 'timestamp'
        
        # Hash detection
        if any(term in field_name_lower for term in ['hash', 'md5', 'sha', 'checksum']):
            if self._validate_hash(value_str):
                return 'hash'
        
        return 'string'
    
    def _validate_ip_address(self, value):
        """Validate IP address format"""
        try:
            ipaddress.ip_address(value)
            return True
        except ValueError:
            return False
    
    def _validate_port(self, value):
        """Validate port number"""
        try:
            port = int(value)
            return 1 <= port <= 65535
        except ValueError:
            return False
    
    def _validate_timestamp(self, value):
        """Validate timestamp format"""
        timestamp_patterns = [
            r'\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}',  # ISO 8601
            r'\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}',  # MySQL format
            r'\d{2}/\d{2}/\d{4} \d{2}:\d{2}:\d{2}',  # US format
            r'\d{10}',  # Unix timestamp
            r'\d{13}'   # Unix timestamp milliseconds
        ]
        
        for pattern in timestamp_patterns:
            if re.match(pattern, str(value)):
                return True
        return False
    
    def _convert_timestamp(self, value):
        """Convert timestamp to ISO 8601 UTC format"""
        try:
            # Unix timestamp (seconds)
            if re.match(r'^\d{10}$', str(value)):
                return datetime.fromtimestamp(int(value)).isoformat() + 'Z'
            
            # Unix timestamp (milliseconds)
            if re.match(r'^\d{13}$', str(value)):
                return datetime.fromtimestamp(int(value) / 1000).isoformat() + 'Z'
            
            # ISO 8601 format
            if 'T' in str(value):
                dt = datetime.fromisoformat(str(value).replace('Z', '+00:00'))
                return dt.isoformat() + 'Z'
            
            # Other formats - attempt parsing
            for fmt in ['%Y-%m-%d %H:%M:%S', '%m/%d/%Y %H:%M:%S', '%d/%m/%Y %H:%M:%S']:
                try:
                    dt = datetime.strptime(str(value), fmt)
                    return dt.isoformat() + 'Z'
                except ValueError:
                    continue
            
            return str(value)  # Return original if can't parse
        except Exception:
            return str(value)
```

---

## Data Enrichment Strategies

### Contextual Enhancement Sources

#### Geographic Intelligence Integration

**IP Geolocation Enrichment**:
```python
# Geolocation enrichment service
import geoip2.database
import geoip2.errors

class GeolocationEnricher:
    def __init__(self, geoip_db_path):
        self.geoip_reader = geoip2.database.Reader(geoip_db_path)
        self.cache = {}  # Simple in-memory cache
    
    def enrich_ip_address(self, ip_address):
        """Add geographic information to IP address"""
        if ip_address in self.cache:
            return self.cache[ip_address]
        
        try:
            response = self.geoip_reader.city(ip_address)
            
            geo_data = {
                'country': response.country.name,
                'country_code': response.country.iso_code,
                'region': response.subdivisions.most_specific.name,
                'region_code': response.subdivisions.most_specific.iso_code,
                'city': response.city.name,
                'postal_code': response.postal.code,
                'latitude': float(response.location.latitude) if response.location.latitude else None,
                'longitude': float(response.location.longitude) if response.location.longitude else None,
                'timezone': response.location.time_zone,
                'isp': response.traits.isp if hasattr(response.traits, 'isp') else None,
                'organization': response.traits.organization if hasattr(response.traits, 'organization') else None,
                'asn': response.traits.autonomous_system_number if hasattr(response.traits, 'autonomous_system_number') else None,
                'as_organization': response.traits.autonomous_system_organization if hasattr(response.traits, 'autonomous_system_organization') else None
            }
            
            # Add risk indicators
            geo_data['is_private'] = ipaddress.ip_address(ip_address).is_private
            geo_data['is_tor_exit'] = self._check_tor_exit(ip_address)
            geo_data['is_vpn'] = self._check_vpn_provider(geo_data.get('organization', ''))
            
            self.cache[ip_address] = geo_data
            return geo_data
            
        except geoip2.errors.AddressNotFoundError:
            return {'error': 'IP address not found in database'}
        except Exception as e:
            return {'error': f'Geolocation lookup failed: {str(e)}'}
    
    def _check_tor_exit(self, ip_address):
        """Check if IP is known Tor exit node"""
        # Implement Tor exit node check logic
        # This would typically query a Tor exit node list
        return False
    
    def _check_vpn_provider(self, organization):
        """Check if organization is known VPN provider"""
        vpn_indicators = [
            'vpn', 'virtual private network', 'proxy', 'tunnel',
            'nordvpn', 'expressvpn', 'surfshark', 'cyberghost'
        ]
        org_lower = organization.lower() if organization else ''
        return any(indicator in org_lower for indicator in vpn_indicators)
```

#### Threat Intelligence Integration

**IOC Matching and Context Addition**:
```python
# Threat intelligence enrichment
import requests
import json
from datetime import datetime, timedelta

class ThreatIntelEnricher:
    def __init__(self, api_keys):
        self.api_keys = api_keys
        self.cache = {}
        self.cache_ttl = timedelta(hours=6)  # Cache for 6 hours
    
    def enrich_with_threat_intel(self, indicator, indicator_type):
        """Add threat intelligence context to indicator"""
        cache_key = f"{indicator_type}:{indicator}"
        
        # Check cache first
        if cache_key in self.cache:
            cached_data, timestamp = self.cache[cache_key]
            if datetime.now() - timestamp < self.cache_ttl:
                return cached_data
        
        threat_context = {
            'indicator': indicator,
            'indicator_type': indicator_type,
            'is_malicious': False,
            'threat_types': [],
            'threat_actors': [],
            'campaigns': [],
            'first_seen': None,
            'last_seen': None,
            'confidence': 0,
            'sources': []
        }
        
        # Query multiple threat intelligence sources
        if indicator_type == 'ip':
            threat_context.update(self._check_ip_reputation(indicator))
        elif indicator_type == 'domain':
            threat_context.update(self._check_domain_reputation(indicator))
        elif indicator_type == 'hash':
            threat_context.update(self._check_file_reputation(indicator))
        elif indicator_type == 'url':
            threat_context.update(self._check_url_reputation(indicator))
        
        # Cache result
        self.cache[cache_key] = (threat_context, datetime.now())
        return threat_context
    
    def _check_ip_reputation(self, ip_address):
        """Check IP reputation across multiple sources"""
        reputation_data = {
            'is_malicious': False,
            'sources': [],
            'threat_types': [],
            'confidence': 0
        }
        
        # VirusTotal IP lookup
        if 'virustotal' in self.api_keys:
            vt_result = self._query_virustotal_ip(ip_address)
            if vt_result and vt_result.get('malicious_count', 0) > 0:
                reputation_data['is_malicious'] = True
                reputation_data['sources'].append('VirusTotal')
                reputation_data['threat_types'].extend(vt_result.get('threat_types', []))
                reputation_data['confidence'] = max(reputation_data['confidence'], 
                                                  vt_result.get('confidence', 0))
        
        # AbuseIPDB lookup
        if 'abuseipdb' in self.api_keys:
            abuse_result = self._query_abuseipdb(ip_address)
            if abuse_result and abuse_result.get('abuse_confidence', 0) > 50:
                reputation_data['is_malicious'] = True
                reputation_data['sources'].append('AbuseIPDB')
                reputation_data['confidence'] = max(reputation_data['confidence'],
                                                  abuse_result.get('abuse_confidence', 0) / 100)
        
        return reputation_data
    
    def _query_virustotal_ip(self, ip_address):
        """Query VirusTotal for IP reputation"""
        try:
            headers = {'x-apikey': self.api_keys['virustotal']}
            response = requests.get(
                f'https://www.virustotal.com/api/v3/ip_addresses/{ip_address}',
                headers=headers,
                timeout=10
            )
            
            if response.status_code == 200:
                data = response.json()
                stats = data.get('data', {}).get('attributes', {}).get('last_analysis_stats', {})
                
                return {
                    'malicious_count': stats.get('malicious', 0),
                    'suspicious_count': stats.get('suspicious', 0),
                    'confidence': min(stats.get('malicious', 0) / 10.0, 1.0),
                    'threat_types': ['malware'] if stats.get('malicious', 0) > 0 else []
                }
        except Exception as e:
            print(f"VirusTotal API error: {e}")
        
        return None
```

#### Asset Context Integration

**Asset Management Integration**:
```python
# Asset context enrichment
class AssetContextEnricher:
    def __init__(self, asset_db_connection):
        self.asset_db = asset_db_connection
        self.asset_cache = {}
    
    def enrich_with_asset_context(self, ip_address, hostname=None):
        """Add asset management context"""
        cache_key = ip_address or hostname
        
        if cache_key in self.asset_cache:
            return self.asset_cache[cache_key]
        
        # Query asset database
        asset_info = self._query_asset_database(ip_address, hostname)
        
        asset_context = {
            'asset_id': asset_info.get('asset_id'),
            'asset_name': asset_info.get('name'),
            'asset_type': asset_info.get('type', 'unknown'),
            'criticality': asset_info.get('criticality', 'medium'),
            'owner': asset_info.get('owner'),
            'department': asset_info.get('department'),
            'location': asset_info.get('location'),
            'operating_system': asset_info.get('os'),
            'environment': asset_info.get('environment', 'production'),
            'compliance_scope': asset_info.get('compliance_scope', []),
            'vulnerability_score': asset_info.get('vuln_score', 0),
            'last_scan_date': asset_info.get('last_scan'),
            'patch_status': asset_info.get('patch_status', 'unknown')
        }
        
        # Add derived context
        asset_context['is_critical'] = asset_context['criticality'] in ['high', 'critical']
        asset_context['requires_monitoring'] = asset_context['compliance_scope'] or asset_context['is_critical']
        asset_context['risk_score'] = self._calculate_risk_score(asset_context)
        
        self.asset_cache[cache_key] = asset_context
        return asset_context
    
    def _query_asset_database(self, ip_address, hostname):
        """Query asset management database"""
        query = """
        SELECT asset_id, name, type, criticality, owner, department, 
               location, operating_system, environment, compliance_scope,
               vulnerability_score, last_scan_date, patch_status
        FROM assets 
        WHERE ip_address = ? OR hostname = ?
        """
        
        cursor = self.asset_db.cursor()
        cursor.execute(query, (ip_address, hostname))
        result = cursor.fetchone()
        
        if result:
            return dict(zip([col[0] for col in cursor.description], result))
        return {}
    
    def _calculate_risk_score(self, asset_context):
        """Calculate composite risk score"""
        base_score = 0
        
        # Criticality factor
        criticality_scores = {'low': 1, 'medium': 3, 'high': 7, 'critical': 10}
        base_score += criticality_scores.get(asset_context['criticality'], 3)
        
        # Vulnerability factor
        base_score += min(asset_context['vulnerability_score'] / 10, 5)
        
        # Environment factor
        if asset_context['environment'] == 'production':
            base_score += 2
        
        # Compliance factor
        if asset_context['compliance_scope']:
            base_score += 3
        
        return min(base_score, 10)  # Cap at 10
```

### User and Identity Enrichment

**Directory Services Integration**:
```python
# User context enrichment from directory services
import ldap
from datetime import datetime

class UserContextEnricher:
    def __init__(self, ldap_server, bind_dn, bind_password):
        self.ldap_server = ldap_server
        self.bind_dn = bind_dn
        self.bind_password = bind_password
        self.user_cache = {}
        
    def enrich_user_context(self, username, domain=None):
        """Add user directory information"""
        cache_key = f"{domain}\\{username}" if domain else username
        
        if cache_key in self.user_cache:
            return self.user_cache[cache_key]
        
        try:
            # Connect to LDAP
            conn = ldap.initialize(f"ldap://{self.ldap_server}")
            conn.simple_bind_s(self.bind_dn, self.bind_password)
            
            # Search for user
            search_filter = f"(sAMAccountName={username})"
            attributes = [
                'displayName', 'mail', 'department', 'title', 'manager',
                'memberOf', 'userAccountControl', 'lastLogon', 'passwordLastSet',
                'whenCreated', 'whenChanged', 'telephoneNumber', 'physicalDeliveryOfficeName'
            ]
            
            result = conn.search_s(
                "dc=company,dc=com",  # Base DN
                ldap.SCOPE_SUBTREE,
                search_filter,
                attributes
            )
            
            if result:
                user_dn, user_attrs = result[0]
                
                user_context = {
                    'username': username,
                    'display_name': self._get_ldap_value(user_attrs, 'displayName'),
                    'email': self._get_ldap_value(user_attrs, 'mail'),
                    'department': self._get_ldap_value(user_attrs, 'department'),
                    'title': self._get_ldap_value(user_attrs, 'title'),
                    'manager': self._get_ldap_value(user_attrs, 'manager'),
                    'groups': self._extract_groups(user_attrs.get('memberOf', [])),
                    'account_status': self._get_account_status(user_attrs),
                    'last_logon': self._convert_filetime(user_attrs.get('lastLogon', [None])[0]),
                    'password_last_set': self._convert_filetime(user_attrs.get('passwordLastSet', [None])[0]),
                    'account_created': self._convert_generalized_time(user_attrs.get('whenCreated', [None])[0]),
                    'phone': self._get_ldap_value(user_attrs, 'telephoneNumber'),
                    'office': self._get_ldap_value(user_attrs, 'physicalDeliveryOfficeName')
                }
                
                # Add derived attributes
                user_context['is_admin'] = self._check_admin_privileges(user_context['groups'])
                user_context['is_service_account'] = self._check_service_account(username, user_context)
                user_context['risk_level'] = self._assess_user_risk(user_context)
                
                self.user_cache[cache_key] = user_context
                return user_context
                
        except Exception as e:
            return {'error': f"LDAP lookup failed: {str(e)}"}
        
        return {'username': username, 'error': 'User not found'}
    
    def _get_ldap_value(self, attrs, key):
        """Extract single value from LDAP attributes"""
        values = attrs.get(key, [])
        return values[0].decode('utf-8') if values else None
    
    def _extract_groups(self, group_dns):
        """Extract group names from DN format"""
        groups = []
        for group_dn in group_dns:
            dn_str = group_dn.decode('utf-8')
            # Extract CN value from DN
            import re
            match = re.search(r'CN=([^,]+)', dn_str)
            if match:
                groups.append(match.group(1))
        return groups
    
    def _check_admin_privileges(self, groups):
        """Check if user has administrative privileges"""
        admin_groups = [
            'Domain Admins', 'Enterprise Admins', 'Schema Admins',
            'Administrators', 'Server Operators', 'Backup Operators'
        ]
        return any(group in admin_groups for group in groups)
    
    def _assess_user_risk(self, user_context):
        """Assess user risk level"""
        risk_factors = 0
        
        # Administrative privileges
        if user_context['is_admin']:
            risk_factors += 3
        
        # Service account
        if user_context['is_service_account']:
            risk_factors += 2
        
        # Recent password change
        if user_context['password_last_set']:
            days_since_pwd_change = (datetime.now() - user_context['password_last_set']).days
            if days_since_pwd_change > 365:
                risk_factors += 2
        
        # Inactive account
        if user_context['last_logon']:
            days_since_logon = (datetime.now() - user_context['last_logon']).days
            if days_since_logon > 90:
                risk_factors += 1
        
        if risk_factors >= 5:
            return 'high'
        elif risk_factors >= 3:
            return 'medium'
        else:
            return 'low'
```

---

## Performance Optimization

### Efficient Processing Architectures

#### Stream Processing for Real-Time Enrichment

**Apache Kafka Streams Implementation**:
```python
# Real-time stream processing for normalization and enrichment
from kafka import KafkaProducer, KafkaConsumer
import json
import threading
from queue import Queue

class StreamProcessor:
    def __init__(self, kafka_bootstrap_servers):
        self.bootstrap_servers = kafka_bootstrap_servers
        self.enrichment_services = {}
        self.processing_queue = Queue(maxsize=10000)
        self.batch_size = 100
        
    def register_enrichment_service(self, name, service):
        """Register an enrichment service"""
        self.enrichment_services[name] = service
    
    def start_processing(self):
        """Start stream processing"""
        # Start consumer thread
        consumer_thread = threading.Thread(target=self._consume_events)
        consumer_thread.daemon = True
        consumer_thread.start()
        
        # Start processing threads
        for i in range(4):  # 4 processing threads
            processor_thread = threading.Thread(target=self._process_events)
            processor_thread.daemon = True
            processor_thread.start()
    
    def _consume_events(self):
        """Consume events from Kafka"""
        consumer = KafkaConsumer(
            'raw_logs',
            bootstrap_servers=self.bootstrap_servers,
            value_deserializer=lambda x: json.loads(x.decode('utf-8')),
            group_id='normalization_group'
        )
        
        for message in consumer:
            try:
                self.processing_queue.put(message.value, timeout=1)
            except:
                # Queue full, skip message (implement proper error handling)
                pass
    
    def _process_events(self):
        """Process events in batches"""
        producer = KafkaProducer(
            bootstrap_servers=self.bootstrap_servers,
            value_serializer=lambda x: json.dumps(x).encode('utf-8')
        )
        
        batch = []
        
        while True:
            try:
                # Collect batch
                while len(batch) < self.batch_size:
                    event = self.processing_queue.get(timeout=1)
                    batch.append(event)
                
                # Process batch
                processed_events = self._process_batch(batch)
                
                # Send to output topic
                for event in processed_events:
                    producer.send('normalized_logs', event)
                
                batch.clear()
                
            except:
                # Process partial batch if timeout
                if batch:
                    processed_events = self._process_batch(batch)
                    for event in processed_events:
                        producer.send('normalized_logs', event)
                    batch.clear()
    
    def _process_batch(self, events):
        """Process batch of events"""
        processed = []
        
        for event in events:
            try:
                # Normalize event
                normalized = self._normalize_event(event)
                
                # Enrich event
                enriched = self._enrich_event(normalized)
                
                processed.append(enriched)
                
            except Exception as e:
                # Log error and include original event
                error_event = event.copy()
                error_event['processing_error'] = str(e)
                processed.append(error_event)
        
        return processed
    
    def _normalize_event(self, event):
        """Apply normalization to single event"""
        # Determine source type
        source_type = event.get('source_type', 'unknown')
        
        # Apply appropriate normalizer
        if source_type in self.normalizers:
            return self.normalizers[source_type].normalize(event)
        
        return event
    
    def _enrich_event(self, event):
        """Apply enrichment services to event"""
        enriched = event.copy()
        
        # Apply each enrichment service
        for service_name, service in self.enrichment_services.items():
            try:
                enrichment = service.enrich(event)
                if enrichment:
                    enriched.update(enrichment)
            except Exception as e:
                enriched[f'{service_name}_error'] = str(e)
        
        return enriched
```

#### Caching Strategies for Performance

**Multi-Level Caching Implementation**:
```python
# Hierarchical caching for enrichment services
import redis
import json
from datetime import datetime, timedelta
import hashlib

class EnrichmentCache:
    def __init__(self, redis_host='localhost', redis_port=6379):
        self.redis_client = redis.Redis(host=redis_host, port=redis_port)
        self.local_cache = {}
        self.local_cache_size = 10000
        self.cache_ttl = {
            'geolocation': timedelta(hours=24),
            'threat_intel': timedelta(hours=6),
            'asset_context': timedelta(hours=12),
            'user_context': timedelta(hours=8)
        }
    
    def get_cached_enrichment(self, cache_type, key):
        """Get enrichment data from cache"""
        cache_key = f"{cache_type}:{key}"
        
        # Check local cache first
        if cache_key in self.local_cache:
            data, timestamp = self.local_cache[cache_key]
            if datetime.now() - timestamp < self.cache_ttl.get(cache_type, timedelta(hours=1)):
                return data
            else:
                del self.local_cache[cache_key]
        
        # Check Redis cache
        try:
            cached_data = self.redis_client.get(cache_key)
            if cached_data:
                data = json.loads(cached_data)
                
                # Store in local cache
                self._store_local(cache_key, data)
                return data
        except:
            pass
        
        return None
    
    def store_enrichment(self, cache_type, key, data):
        """Store enrichment data in cache"""
        cache_key = f"{cache_type}:{key}"
        
        # Store in local cache
        self._store_local(cache_key, data)
        
        # Store in Redis
        try:
            ttl_seconds = int(self.cache_ttl.get(cache_type, timedelta(hours=1)).total_seconds())
            self.redis_client.setex(
                cache_key,
                ttl_seconds,
                json.dumps(data)
            )
        except:
            pass
    
    def _store_local(self, cache_key, data):
        """Store in local cache with size management"""
        # Remove oldest entries if cache is full
        if len(self.local_cache) >= self.local_cache_size:
            # Remove 10% of oldest entries
            remove_count = int(self.local_cache_size * 0.1)
            oldest_keys = sorted(
                self.local_cache.keys(),
                key=lambda k: self.local_cache[k][1]
            )[:remove_count]
            
            for key in oldest_keys:
                del self.local_cache[key]
        
        self.local_cache[cache_key] = (data, datetime.now())
```

### Quality Metrics and Monitoring

#### Normalization Quality Assessment

**Quality Scoring Framework**:
```python
# Normalization quality assessment
class NormalizationQualityAssessor:
    def __init__(self):
        self.quality_metrics = {
            'field_completeness': 0.3,
            'type_accuracy': 0.25,
            'format_consistency': 0.2,
            'enrichment_success': 0.25
        }
    
    def assess_quality(self, original_event, normalized_event):
        """Assess normalization quality"""
        scores = {}
        
        # Field completeness
        scores['field_completeness'] = self._assess_field_completeness(
            original_event, normalized_event
        )
        
        # Type accuracy
        scores['type_accuracy'] = self._assess_type_accuracy(normalized_event)
        
        # Format consistency
        scores['format_consistency'] = self._assess_format_consistency(normalized_event)
        
        # Enrichment success
        scores['enrichment_success'] = self._assess_enrichment_success(normalized_event)
        
        # Calculate weighted score
        overall_score = sum(
            scores[metric] * weight 
            for metric, weight in self.quality_metrics.items()
        )
        
        return {
            'overall_score': overall_score,
            'detailed_scores': scores,
            'quality_grade': self._grade_quality(overall_score)
        }
    
    def _assess_field_completeness(self, original, normalized):
        """Assess how many expected fields were successfully extracted"""
        expected_fields = [
            'timestamp', 'src_ip', 'dst_ip', 'user', 'action', 'message'
        ]
        
        extracted_count = sum(
            1 for field in expected_fields 
            if field in normalized and normalized[field] is not None
        )
        
        return extracted_count / len(expected_fields)
    
    def _assess_type_accuracy(self, normalized):
        """Assess data type accuracy"""
        type_checks = {
            'src_ip': self._is_valid_ip,
            'dst_ip': self._is_valid_ip,
            'src_port': self._is_valid_port,
            'dst_port': self._is_valid_port,
            'timestamp': self._is_valid_timestamp
        }
        
        correct_types = 0
        total_checks = 0
        
        for field, validator in type_checks.items():
            if field in normalized and normalized[field] is not None:
                total_checks += 1
                if validator(normalized[field]):
                    correct_types += 1
        
        return correct_types / total_checks if total_checks > 0 else 1.0
    
    def _grade_quality(self, score):
        """Convert quality score to grade"""
        if score >= 0.9:
            return 'A'
        elif score >= 0.8:
            return 'B'
        elif score >= 0.7:
            return 'C'
        elif score >= 0.6:
            return 'D'
        else:
            return 'F'
```

[⬆️ Back to SIEM & Monitoring](./README.md)
