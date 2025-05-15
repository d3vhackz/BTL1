# Data Loss Prevention (DLP)

DLP is a security control designed to prevent sensitive or confidential information from leaving an organization in an unauthorized manner—whether by accident or through malicious intent.

## What DLP Protects

- **Personally Identifiable Information (PII)**  
  Names, addresses, Social Security numbers, dates of birth, etc.

- **Financial Data**  
  Bank details, credit card numbers, payroll info

- **Login Credentials**  
  Usernames, passwords, session tokens

- **Business Secrets**  
  Internal documents, source code, strategic plans

## How DLP Works

- **Scanning Outbound Email**  
  - Analyzes body, headers, and attachments
  - Detects keywords, patterns, and file types

- **Pattern Matching**  
  - Uses predefined or custom rules (e.g., regex for credit cards or SSNs)

- **Keyword and Phrase Detection**  
  - Flags sensitive language (e.g., “confidential,” “SSN,” “wire transfer”)

- **Policy Enforcement**  
  - Can block, quarantine, or log attempts to send restricted data
  - May alert IT or security teams

## Use Cases

- Preventing accidental data exposure via email
- Blocking insider threats from exfiltrating data
- Enforcing compliance with regulations (HIPAA, GDPR, PCI-DSS)

## Best Practices

- Define clear DLP policies for users and departments
- Integrate DLP with email scanning, SIEM, and incident response tools
- Train employees on safe data handling

---

> DLP is essential for reducing insider risk, maintaining customer trust, and meeting regulatory requirements. It turns policy into enforcement.

