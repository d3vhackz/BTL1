# 4.2: Data Representation and Encoding

Digital evidence is stored in various formats. Understanding these representations is crucial for interpreting raw data during an investigation. This section covers key formats and the tools used to work with them.

---

## Core Data Formats

### Binary
Binary is a base-2 number system using only `0` (off) and `1` (on) to represent data at the most fundamental level of computing.
-   **Bit**: A single binary digit (0 or 1).
-   **Byte**: A group of 8 bits, capable of representing 256 different values (2⁸).
-   **Relevance**: All digital data, no matter how complex, is ultimately a collection of binary digits.

### Hexadecimal
Hexadecimal is a base-16 number system that uses digits `0-9` and letters `A-F`. It's a more human-readable and compact way to represent binary data.
-   **Usage**: Commonly seen in memory dumps, file headers, and network packet analysis.
-   **Conversion**: Four binary bits (a nibble) can be represented by a single hexadecimal digit.

### Octal
Octal is a base-8 number system using digits `0-7`.
-   **Usage**: Most famously used in Linux/UNIX file permissions (`chmod` command).
-   **Conversion**: Three binary bits can be represented by a single octal digit.

### Base64
Base64 is an encoding scheme that converts binary data into a text-only format. It is reversible.
-   **Purpose**: To safely transmit binary data (like images or files) through systems that are designed to handle only text, such as in email attachments or some API calls.
-   **Forensic Note**: Attackers may use Base64 to obfuscate malicious payloads, scripts, or exfiltrated data within text logs.

### ASCII
The **American Standard Code for Information Interchange (ASCII)** is a character encoding standard that represents text in computers. Each character (alphabetic, numeric, or special) is represented by a number (e.g., 'A' is 65, 'a' is 97).

---

## Tool: CyberChef

**CyberChef** is a powerful web-based tool developed by the UK's GCHQ, often called the "Cyber Swiss Army Knife." It allows analysts to easily encode, decode, and transform data using a simple drag-and-drop interface.

### How It Works
1.  **Input**: Paste the data you want to analyze into the Input pane.
2.  **Operations**: Search for an operation (e.g., "From Base64," "To Hex").
3.  **Recipe**: Drag the chosen operation into the Recipe pane. You can chain multiple operations together.
4.  **Output**: The Output pane automatically displays the result of the transformation.

CyberChef is an essential tool for quickly decoding obfuscated data found during an investigation.

[⬆️ Back to Digital Forensics](./README.md)
