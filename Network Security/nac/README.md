# Network Access Control (NAC)

Network Access Control (NAC) is a security solution that manages and enforces policies on devices trying to connect to a network—ensuring that only trusted, compliant devices are allowed access.

---

## Purpose of NAC

- Prevent unauthorized or rogue devices from connecting
- Enforce security posture (e.g., patches, antivirus, configurations)
- Protect internal network segments from exposure
- Support BYOD and guest access without compromising security

---

## How NAC Works

- **Device Authentication**: Uses credentials, certificates, or profiling to verify identity
- **Posture Assessment**: Checks for compliance with security policies (e.g., latest OS updates, AV running)
- **Access Enforcement**:
  - Grants access
  - Denies access
  - Places device in quarantine VLAN for remediation

---

## Key Components

- **Policy Server**: Central brain that defines and evaluates access policies
- **Enforcement Point**: Usually a switch or wireless controller that allows/blocks access
- **Agent or Agentless Check**: Installed software (or passive inspection) to assess device health

---

## Use Case Examples

- Blocking an unpatched device from accessing the internal LAN
- Redirecting a guest device to a captive portal for authentication
- Enforcing network segmentation based on device type (e.g., printers vs. laptops)

---

## Common NAC Technologies

- Cisco ISE
- Aruba ClearPass
- Microsoft NPS with 802.1X
- FortiNAC

---

## Best Practices

- Define clear access policies for each device category
- Start in monitor mode to reduce false positives during deployment
- Regularly update compliance checks (e.g., AV, OS versions)
- Integrate NAC with AD, SIEM, and patch management tools

---

> NAC is your digital bouncer—it checks who you are, what you're carrying, and whether you’re allowed inside. It’s critical for secure, scalable, and compliant network environments.

