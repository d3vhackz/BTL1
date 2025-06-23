# 4.5: Evidence Handling and Integrity

Proper evidence handling is the cornerstone of a forensically sound investigation. This section covers the core principles that govern the treatment of digital evidence to ensure its integrity and admissibility.

---

## Digital Evidence

**Digital evidence** is any probative information stored or transmitted in digital form. This includes emails, logs, files, browser history, and photos. It is often more voluminous, easily duplicated, and more fragile than physical evidence.

> **Key Principle**: Digital evidence is easily modified. A single piece of evidence should always be verified with corroborating data before being trusted.

---

## ACPO Principles for Digital Evidence

The Association of Chief Police Officers (ACPO) established four core principles for handling computer-based electronic evidence. These are the gold standard in forensic investigations.

> **Principle 1:** No action taken by law enforcement or their agents should change data held on a computer or storage media which may subsequently be relied upon in court.

> **Principle 2:** In circumstances where a person finds it necessary to access original data held on a computer or on storage media, that person must be competent to do so and be able to give evidence explaining the relevance and the implications of their actions.

> **Principle 3:** An audit trail or other record of all processes applied to computer-based electronic evidence should be created and preserved. An independent third party should be able to examine those processes and achieve the same result.

> **Principle 4:** The person in charge of the investigation (the case officer) has overall responsibility for ensuring that the law and these principles are adhered to.

---

## Chain of Custody

The **Chain of Custody** is a crucial process that documents the chronological history of evidence. It ensures the integrity of evidence by tracking who handled it, when, and for what purpose, from the moment of collection to its presentation in court.

A broken chain of custody can lead to evidence being dismissed, as it introduces doubt about whether the evidence was tampered with.

### Maintaining the Chain of Custody
1.  **Integrity Hashing**: Before handling any evidence, calculate its hash value (e.g., SHA256). After creating a forensic copy, hash the copy. The two hashes must match exactly to prove the copy is identical. Use at least two different hash algorithms (e.g., MD5 and SHA256) to guard against hash collisions.
2.  **Forensic Copies**: Always work on a verified forensic copy (a bit-by-bit image) of the original evidence. The original should be securely stored and left untouched.
3.  **Secure Storage**: Physical evidence should be stored in anti-static, tamper-evident bags. Faraday cages may be used to block wireless signals. All evidence must be kept in a secure, access-controlled location.
4.  **Documentation**: Every interaction with the evidence must be logged on a Chain of Custody form. This includes who accessed it, when, where, and why.

[⬆️ Back to Digital Forensics](./README.md)
