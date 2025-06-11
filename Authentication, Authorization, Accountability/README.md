# AAA: Authentication, Authorization, and Accountability

The AAA framework is a foundational security model used to control access, track activity, and enforce policies across systems and networks.

---

## Authentication

**Definition:** Verifies the identity of a user, system, or device.

**Types:**
- **Something you know** – Password, PIN
- **Something you have** – Token, smart card, mobile device
- **Something you are** – Biometrics (fingerprint, face ID)

**Best Practices:**
- Enforce strong password policies
- Implement Multi-Factor Authentication (MFA)
- Use centralized identity providers (e.g., Active Directory, LDAP)

---

## Authorization

**Definition:** Determines what an authenticated user is allowed to do.

**Key Concepts:**
- Role-Based Access Control (RBAC)
- Principle of Least Privilege (PoLP)
- Access Control Lists (ACLs)

**Examples:**
- A regular employee cannot access HR files
- A database admin can access production databases, but not accounting systems

---

## Accountability

**Definition:** Tracks user actions and links them to their identity.

**Purpose:**
- Enables audit trails and forensic analysis
- Deters malicious activity by creating traceability

**Tools:**
- Logging and monitoring
- Session recording
- SIEM integration

---

> AAA ensures that only the right users get in, do only what they’re supposed to, and are held accountable for everything they do. It's critical for enforcing security and building trust across systems.

