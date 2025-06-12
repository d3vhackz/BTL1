# Intro to Digital Forensics

Digital forensics is the process of collecting, analyzing, and preserving digital evidence, sometimes so it can be submitted in court. While often associated with law enforcement, digital forensics is a valuable skill for security teams, especially when paired with incident response (IR)—together known as DFIR.

This section covers how forensic investigations are conducted and what information can be retrieved from Windows and Linux systems, including practical activities using real-world tools.

## The Digital Forensics Process

Digital forensics follows a scientific method and is mainly used in computer and mobile device investigations. The five core steps are:

- **Identification** – Locate potential sources of digital evidence.
- **Preservation** – Secure and document the crime scene and evidence.
- **Collection** – Extract and image relevant digital information.
- **Analysis** – Examine collected data to draw conclusions.
- **Reporting** – Document findings in a repeatable, verifiable manner.

A crucial companion step is **Contemporaneous Notetaking**, which ensures that actions taken are documented in real time and can be reproduced by others. Also, the **Chain of Custody** must be maintained at all stages to ensure evidence integrity.

This lesson provides additional material to help reinforce your understanding and prep for the BTL1 practical exam.

---

## Resources

- [Digital Forensics Resources by Forensic Focus](https://www.forensicfocus.com/articles/digital-forensics-resources/)
- [Top Online Digital Computer Forensics Resources by InfoSec Institute](https://www.infosecinstitute.com/resources/digital-forensics/)
- [Digital Forensics: Tools & Resources by Study.com](https://study.com/academy/lesson/digital-forensics-tools-resources.html)
- [Digital Forensics Cheat Sheet by Tech Republic](https://www.techrepublic.com/article/digital-forensics-the-smart-persons-guide/)
- [A Guide to Digital Forensics and Cybersecurity Tools (2020) by Forensics Colleges](https://www.forensicscolleges.com/blog/resources/guide-digital-forensics-tools)
- [Free Course – Digital Forensics by OpenLearn](https://www.open.edu/openlearn/science-maths-technology/digital-forensics/content-section-0?active-tab=description-tab)

---

## Digital Forensics Glossary

- **IOC** – *Indicator of Compromise*: Data points indicating malicious activity (e.g. malware hashes, IPs).
- **TTP** – *Tools, Techniques, and Procedures*: Adversarial behavior patterns defined in [MITRE ATT&CK](https://attack.mitre.org/).
- **PCAP** – *Packet Capture*: Captured network traffic used for analysis (e.g. in Wireshark).
- **HDD** – *Hard Disk Drive*: Traditional spinning disk-based storage.
- **SSD** – *Solid State Drive*: Flash-based storage, faster and more durable than HDDs.
- **USB Drive** – *Universal Serial Bus Drive*: Portable flash storage device.
- **ACPO** – *Association of Chief Police Officers*: Guidelines for handling computer-based evidence in the UK.
- **KAPE** – *Kroll Artifact Parser and Extractor*: Fast digital evidence collection tool.
- **FTK Imager** – *Forensic Tool Kit Imager*: Tool for imaging and analyzing disk drives.
- **Write-Blocker** – Device/software that prevents writing to evidence drives during examination.
- **BHC** – *Browser History Capturer*: Collects browser artifacts for analysis.
- **BHV** – *Browser History Viewer*: Visual tool for analyzing browser data collected by BHC.
- **/etc/passwd** – Linux file listing user accounts; used with `/etc/shadow` for password auditing.
- **/etc/shadow** – Linux file storing encrypted user passwords; used with `/etc/passwd` to crack hashes.

---

[Back to Digital Forensics](./Digital%20Forensics/README.md)
