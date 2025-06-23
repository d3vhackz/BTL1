# 4.9: Evidence Destruction

Once an investigation is concluded and the legal or organizational retention period has expired, digital evidence must be securely and permanently destroyed to ensure sensitive data is not compromised.

---

## Methods of Evidence Destruction

The method chosen depends on whether the storage media needs to be reused.

### Physical Destruction Methods
These methods render the storage media completely unusable.

-   **Physical Shredding**: An industrial shredder grinds the hard drive, SSD, or USB drive into small, unrecognizable pieces, destroying the drive platters and electronic components.
-   **Hydraulic Crusher / Punch**: A hydraulic press drives a metal rod through the storage device with immense force, shattering the platters and rendering the data unrecoverable.
-   **Degaussing**: Exposes the storage media (HDDs, magnetic tapes) to a powerful magnetic field that neutralizes the magnetic data, effectively erasing it. This is the guaranteed standard for magnetic media erasure but does not work on SSDs.

### Non-Destructive Methods
These methods allow the storage media to be reused.

-   **Overwriting / Wiping**: This process involves writing new data over the entire surface of a drive, replacing the original information. A simple overwrite might write all zeros.
-   **Secure Erasure Standards**: More robust methods overwrite the drive multiple times with different patterns to ensure data is unrecoverable. A well-known example is the **DoD 5220.22-M** method, which involves three passes:
    1.  Writes a zero and verifies.
    2.  Writes a one and verifies.
    3.  Writes a random character and verifies.

> **Important Note**: Simply deleting files via the operating system (e.g., moving to Recycle Bin) is **not** a secure method of destruction. As covered in file carving, the data often remains on the disk until it is overwritten.

[⬆️ Back to Digital Forensics](./README.md)
