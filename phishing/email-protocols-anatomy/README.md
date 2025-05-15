# Email Protocols & Anatomy

Understanding email protocols and structure is key to analyzing, securing, and investigating phishing attempts.

---

## Common Email Protocols

### SMTP (Simple Mail Transfer Protocol)
- **Port:** TCP 25 (legacy), now often TCP 587
- **Purpose:** Sends email between servers
- **Security Note:** TCP 25 is often blocked due to spam abuse

### POP3 (Post Office Protocol v3)
- **Port:** TCP 110
- **Purpose:** Downloads email from server to client, then deletes it from the server
- **Limitation:** Emails are not accessible from multiple devices after download

### IMAP (Internet Message Access Protocol)
- **Port:** TCP 143 (unencrypted), TCP 993 (encrypted)
- **Purpose:** Allows reading and managing emails directly on the server
- **Benefit:** Supports access from multiple devices

---

## Email Header Anatomy

Headers are metadata attached to every email that describe its origin and path.

### Key Fields:
- **From:** Sender’s email address
- **To:** Recipient’s email address
- **Date:** When the email was sent
- **Subject:** Email title/summary
- **Reply-To:** Alternate address for replies
- **Message-ID:** Unique identifier for the message
- **Received:** Lists all servers the message passed through, in reverse order

---

## Email Body

- Contains the actual message content
- Can be plain text, HTML, or a mix
- Often encoded (e.g., Base64) to support styling or attachments

**Tip:** Use tools like [CyberChef](https://gchq.github.io/CyberChef/) to decode Base64 content for analysis.

---

## Why This Matters for Phishing Analysis

- Spoofed “From” fields are common in phishing
- `Reply-To` may redirect to a malicious domain
- `Received` headers reveal the true sending path
- Encoded bodies can hide malicious links or payloads

---

> Every phishing email is a lie wrapped in metadata. Knowing how to read the headers and decode the message reveals the truth attackers hope you miss.

