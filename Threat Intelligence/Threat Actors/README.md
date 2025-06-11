# Threat Actors and APTs

## What Are Threats?

A **threat** is a danger that can exploit a vulnerability, resulting in a breach.  
**Example**:  
- **Vulnerability**: Lack of input validation  
- **Threat**: SQL injection  
- **Impact**: Attacker retrieves sensitive database content

## What Are Threat Actors?

A **threat actor** (or agent) is an individual or group—either intentional or unintentional—that causes harm to an organization.

### Examples
- **Intentional**: Cybercrime syndicate exploiting RCE to steal data  
- **Unintentional**: Employee deletes critical data due to lack of training

## Threat Actor Categories

### 1. Cyber Criminals
Motivated by financial gain. Varies from elite hackers to script kiddies.

### 2. Nation-States
Government-backed hackers (APT groups), extremely resourced and persistent.

### 3. Hacktivists
Social or political motivation. Known for DDoS and website defacement.

### 4. Insider Threats
Employees or contractors who leak data, whether intentional or accidental.

---

## Real-World Threat Actor Examples

### APT29 (Nation-State – Russia)
Also known as **Cozy Bear**. Famous for:
- Spear-phishing Pentagon (2015)
- Targeting diplomats and governments
- Continuously evolving malware

### Anonymous (Hacktivist)
Known for:
- **Operation Megaupload** (2012)
- DDoSing DOJ, Copyright Office in protest of SOPA/PIPA

---

## Motivations

### 1. Financial
- **Individuals**: Corporate espionage  
- **Groups**: Ransomware, banking trojans  
- **Governments**: Lazarus Group stealing funds to bypass sanctions

### 2. Political
- Cyberwarfare (e.g., **Stuxnet** virus against Iran’s nuclear program)
- Disinformation campaigns
- Website defacements by activists

### 3. Social
- Script kiddies or groups like **Lizard Squad** seeking notoriety
- Motivated by ego, status, or recognition

### 4. Unknown
- Attribution is difficult
- Actor motives may surface after investigation

---

## Actor Naming Conventions

### CrowdStrike Naming

- **Russia** = Bear (e.g., Fancy Bear)  
- **China** = Panda (e.g., Goblin Panda)  
- **Iran** = Kitten  
- **North Korea** = Chollima  
- **Vietnam** = Buffalo  
- **India** = Tiger  
- **Pakistan** = Leopard  
- **South Korea** = Crane  

**Hacktivists** = Jackal  
**eCrime groups** = Spider (e.g., Mummy Spider/Emotet)

### Mandiant (FireEye) Naming

- Uses `APTxx` format:
  - **Russia** = APT28, APT29  
  - **China** = APT1, APT3, APT41  
  - **Iran** = APT33, APT35  
  - **North Korea** = APT37, APT38  

- Financial groups use `FINxx`:
  - **Examples**: FIN4, FIN7 (POS malware)

- Unclassified groups: `UNCxxxx`

---

## What Are APTs?

**Advanced Persistent Threats (APTs)** are:
- State-sponsored or highly resourced groups
- Use zero-days, custom malware, and stealth tactics
- Prioritize persistence and stealth over time

### Key Characteristics
- Financial, political, or military targets
- Sophisticated toolchains
- Long-term, stealthy operations

---

## APT Case Studies

### APT28 (Russia)
Also known as Fancy Bear. Linked to:
- US election interference
- NATO and Eastern European espionage

### Cobalt Group (Financial)
- Uses **SpicyOmelette**, **Threadkit**
- Targets banks and ATMs
- Caused >€1 billion in damages

### APT32 (Vietnam)
- Targets Southeast Asian governments
- Uses web-based exploits and phishing

---

## TTPs – Tools, Techniques, and Procedures

**Mapped in the MITRE ATT&CK Framework**, TTPs are categorized under:

1. Initial Access  
2. Execution  
3. Persistence  
4. Privilege Escalation  
5. Defense Evasion  
6. Credential Access  
7. Discovery  
8. Lateral Movement  
9. Collection  
10. Command & Control  
11. Exfiltration  
12. Impact

### Example: T1020 – Data Exfiltration
- Can be used to map backward through an attacker’s behavior
- Enables defenders to recreate attack paths and implement controls

---

## Proactive Defense with TTPs

- Review likely adversaries (e.g., APT30) on [MITRE ATT&CK Groups](https://attack.mitre.org/groups/)
- Simulate attacks or conduct red team exercises using known TTPs
- Ensure your systems have detection rules and monitoring in place

---

## Case Study: Cobalt Group Exploit Chain

1. **Spear-phishing** email with malicious doc
2. Link downloads Word file with VBA macro
3. Exploits via Threadkit (MS Office vulnerabilities)
4. Payload evades AppLocker (e.g., via CMSTP)
5. Shellcode launches encrypted **Cobalt Strike** beacon
6. System compromised – full access achieved

> Well-orchestrated attack paths make APTs deadly effective.

