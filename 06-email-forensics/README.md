# Chapter 6 — Email Forensics

[⬅ Previous Chapter](../05-legal-and-ethical-considerations/README.md) | [Back to Course Home](../README.md) | [Next Chapter ➡](../07-network-forensics/README.md)


---

## 1. Introduction to Email Forensics

Email forensics is the process of identifying, collecting, preserving, examining, and analyzing email-related evidence during a digital investigation. Email messages can contain valuable information about a security incident, including the identity or address of a sender, intended recipients, timestamps, routing information, message content, attachments, hyperlinks, and information about the systems involved in delivering the message.

Email evidence is commonly encountered in investigations involving phishing, business email compromise (BEC), malware distribution, cyber harassment, fraud, insider threats, data leakage, and unauthorized access.

An email may appear simple to the user, but technically it can contain several layers of information. For example, the visible sender address may not represent the actual system from which the message originated. Investigators therefore need to examine the complete message and its headers rather than relying only on the information displayed by an email application.

---

## 2. Role of Email Forensics in Digital Investigations

Email forensics forms part of the wider digital forensic investigation process. An investigator may use email evidence to establish:

* Who sent or received a message.
* When a message was created, sent, received, or forwarded.
* Which mail servers handled the message.
* Whether the sender address was spoofed.
* Whether a message was part of a phishing campaign.
* Whether an attachment contained malware.
* Which websites or domains were referenced.
* Whether multiple messages originated from the same infrastructure.
* Whether an account may have been compromised.
* Whether information was intentionally transmitted outside an organization.

Email evidence can also be correlated with evidence from computers, mobile devices, network logs, DNS records, web browsers, and cloud services.

For example, if an employee receives a suspicious email containing a malicious link, an investigator may compare the email timestamp with firewall, proxy, DNS, and endpoint logs to determine whether the link was accessed.

---

# 3. Email Architecture

Understanding how email works is important before attempting to analyze email evidence.

A typical email system consists of:

1. **Mail User Agent (MUA)** — the application used by a user to compose and read email.
2. **Mail Submission Agent (MSA)** — accepts outgoing messages from an email client.
3. **Mail Transfer Agent (MTA)** — transfers email between mail servers.
4. **Mail Delivery Agent (MDA)** — delivers messages into a recipient's mailbox.
5. **Mailbox** — stores messages for the user.

A simplified communication process is:

```text
Sender
   |
   v
Email Client (MUA)
   |
   v
Mail Submission Server
   |
   v
Sending Mail Server (MTA)
   |
   v
Internet / DNS
   |
   v
Receiving Mail Server (MTA)
   |
   v
Mailbox / MDA
   |
   v
Recipient Email Client
```

During an investigation, each stage may provide potential evidence.

---

# 4. Email Protocols

## 4.1 SMTP

**Simple Mail Transfer Protocol (SMTP)** is primarily used for sending and transferring email.

SMTP is involved when:

* A user sends an email.
* A mail server transfers an email to another mail server.
* A message is submitted to a mail server.

Common SMTP ports include:

| Port | Typical Use                                      |
| ---- | ------------------------------------------------ |
| 25   | SMTP server-to-server communication              |
| 587  | Message submission, commonly with authentication |
| 465  | SMTP over implicit TLS/SSL                       |

From a forensic perspective, SMTP-related logs can help establish when messages were submitted and which servers were involved.

---

## 4.2 IMAP

**Internet Message Access Protocol (IMAP)** allows users to access messages stored on a mail server.

Unlike traditional POP3 usage, IMAP generally keeps messages on the server and synchronizes them across devices.

Common IMAP ports include:

* 143 — standard IMAP
* 993 — IMAP over TLS

IMAP can be important during investigations because messages, folders, flags, and server-side information may remain available even when a local email application has limited evidence.

---

## 4.3 POP3

**Post Office Protocol version 3 (POP3)** is another protocol used to retrieve email.

Common ports include:

* 110 — standard POP3
* 995 — POP3 over TLS

POP3 traditionally downloads messages from a mail server to the client. Depending on configuration, messages may be removed from the server after download.

This behavior can affect the availability of evidence during an investigation.

---

# 5. Email Message Structure

An email generally consists of two major components:

```text
Email Message
├── Headers
└── Body
    ├── Plain text
    ├── HTML
    └── Attachments
```

## 5.1 Email Headers

Headers contain technical information about the message.

Important headers include:

| Header         | Purpose                                                |
| -------------- | ------------------------------------------------------ |
| From           | Displays the claimed sender                            |
| To             | Identifies intended recipient(s)                       |
| Date           | Indicates the message date/time claimed by the sender  |
| Subject        | Message subject                                        |
| Reply-To       | Address used when replying                             |
| Message-ID     | Unique identifier assigned to the message              |
| Received       | Shows mail-server processing and routing information   |
| Return-Path    | Address used for mail delivery/bounces                 |
| MIME-Version   | Indicates MIME support                                 |
| Content-Type   | Describes message content                              |
| DKIM-Signature | Cryptographic signature information where DKIM is used |

The **Received** headers are particularly important because they can show the path a message took through mail servers.

---

# 6. Analyzing Email Headers

Email header analysis is one of the most important techniques in email forensics.

A message can contain multiple `Received:` headers:

```text
Received: from mail.example.com
        by mx.example.org
        with ESMTPS
        id ABC123
        for <user@example.org>;
        Fri, 28 Aug 2026 10:15:20 +0000
```

Investigators can examine:

* Sending and receiving servers.
* IP addresses.
* Hostnames.
* Timestamps.
* Protocols used.
* Authentication information.
* Message identifiers.
* Routing inconsistencies.

### Important caution

The `From:` field alone should **not** be treated as proof of the true origin of an email. Email addresses can be spoofed.

Similarly, an IP address found in a header should not automatically be treated as the attacker's personal IP address. It may belong to a mail server, cloud service, VPN, proxy, relay, or compromised system.

Evidence should therefore be interpreted in context and correlated with other sources.

---

# 7. Email Metadata

Metadata is information about an email rather than the main message content.

Useful metadata can include:

* Sender address.
* Recipient address.
* Message ID.
* Date and time.
* Time zone.
* Mail-server information.
* IP addresses.
* Authentication results.
* MIME information.
* Attachment names and hashes.
* Domain information.
* DKIM signatures.
* SPF results.
* DMARC results.

Metadata can help investigators reconstruct the sequence of events and identify relationships between messages.

---

# 8. Email Authentication

Modern email systems use several technologies to reduce spoofing and improve message authentication.

## 8.1 SPF

**Sender Policy Framework (SPF)** allows a domain owner to publish information about which mail servers are authorized to send email on behalf of that domain.

An SPF result may appear as:

```text
Received-SPF: pass
```

or:

```text
Received-SPF: fail
```

SPF results can provide useful evidence, but a successful SPF check does not automatically prove that the human sender is legitimate.

---

## 8.2 DKIM

**DomainKeys Identified Mail (DKIM)** uses cryptographic signatures to allow receiving mail systems to verify that a message was associated with a particular domain and that signed content was not improperly modified.

An email may contain:

```text
DKIM-Signature: v=1; d=example.com; ...
```

Investigators should examine the DKIM domain and authentication result when available.

---

## 8.3 DMARC

**Domain-based Message Authentication, Reporting, and Conformance (DMARC)** builds on SPF and DKIM and allows domain owners to specify policies for messages that fail authentication or alignment checks.

A header may contain information such as:

```text
Authentication-Results:
spf=pass;
dkim=pass;
dmarc=pass
```

Authentication results are useful forensic indicators, but they must be interpreted together with the complete header and other evidence.

---

# 9. Email Spoofing

Email spoofing occurs when an attacker manipulates email information to make a message appear to come from another sender.

For example:

```text
From: manager@company.com
```

may be displayed to the victim even though the message was not legitimately sent by the manager.

Common warning signs include:

* Mismatch between visible sender and authenticated domain.
* Suspicious `Reply-To` address.
* Unexpected sending infrastructure.
* Authentication failures.
* Unusual `Received` headers.
* Domain names that closely resemble legitimate domains.
* Unexpected requests for passwords, money, or confidential information.

Investigators should compare the visible sender information with the technical headers and authentication results.

---

# 10. Phishing Investigation

Phishing is an attack in which an attacker attempts to deceive a victim into revealing information, installing malware, transferring money, or performing another harmful action.

Common phishing messages may:

* Create a sense of urgency.
* Request account verification.
* Pretend to originate from banks or organizations.
* Contain suspicious links.
* Include malicious attachments.
* Use fake login pages.
* Impersonate employees or executives.

### Indicators of phishing

Investigators should examine:

1. Sender address.
2. Reply-To address.
3. Email headers.
4. Authentication results.
5. Embedded hyperlinks.
6. Destination domains.
7. Attachments.
8. File hashes.
9. Language and social-engineering techniques.
10. Related messages received by other users.

---

# 11. Email Attachments

Attachments can contain important evidence or malicious content.

Common attachment types include:

* PDF
* Microsoft Office documents
* Images
* ZIP archives
* Executable files
* HTML files
* Scripts

Investigators should avoid opening suspicious attachments directly on a normal workstation.

A safer process is:

```text
Acquire Attachment
       |
       v
Calculate Hash
       |
       v
Identify File Type
       |
       v
Static Analysis
       |
       v
Extract Metadata
       |
       v
Malware Analysis
       |
       v
Document Findings
```

Useful information includes:

* Filename.
* File size.
* File type.
* Creation/modification metadata.
* Cryptographic hash.
* Embedded URLs.
* Macros or scripts.
* Digital signatures.
* Indicators of compromise.

---

# 12. Email Links and URLs

Links embedded in emails should be examined carefully.

An investigator can extract:

* Full URL.
* Domain.
* Subdomain.
* IP address.
* URL parameters.
* Redirect information.
* Domain registration information.
* Associated indicators of compromise.

For example:

```text
https://secure-login.example.com/account/verify
```

should not automatically be considered legitimate simply because it contains the word `secure`.

Investigators should identify the actual registered domain and compare it with the organization being impersonated.

---

# 13. Recovering Deleted Emails

Deleted email evidence may sometimes be recovered depending on how the email system stores and deletes messages.

Potential sources include:

* Email application databases.
* Local mail stores.
* Server-side mailboxes.
* Trash/deleted folders.
* Backups.
* Email archives.
* Cloud retention systems.
* Disk remnants.
* Mobile-device data.
* Synchronization caches.

Examples of local email storage formats include:

* PST
* OST
* MBOX
* EML
* Maildir

Recovery is not guaranteed. Evidence may have been overwritten, permanently deleted, encrypted, or removed by a retention policy.

Investigators should therefore preserve the original evidence before attempting recovery.

---

# 14. Email Forensic Tools

Several tools can assist with email investigations.

## 14.1 Autopsy

Autopsy is a digital forensics platform that can be used to examine forensic images and recover artifacts, including email-related evidence.

It is useful when email evidence is located on a seized computer or storage device.

---

## 14.2 FTK

Forensic Toolkit (FTK) is a commercial forensic investigation platform that supports evidence processing, searching, indexing, and analysis.

It can be useful when an investigation involves large quantities of email and other digital evidence.

---

## 14.3 EnCase

EnCase is a commercial digital forensic platform used for acquisition, examination, and analysis of digital evidence.

It can assist investigators in examining email artifacts alongside other evidence sources.

---

## 14.4 Email Header Analyzers

Header-analysis services and forensic tools can help investigators interpret technical email headers.

They may assist in identifying:

* Mail-server paths.
* IP addresses.
* Authentication results.
* Routing information.
* Potential spoofing indicators.

Investigators should preserve the original headers and not rely solely on the interpretation produced by an online analyzer.

---

## 14.5 Wireshark

Wireshark is a network protocol analyzer. It can be useful when investigators have captured network traffic related to email communication.

For encrypted email traffic, packet contents may not be directly readable, but metadata such as IP addresses, ports, protocols, and connection timing may still be useful.

---

# 15. Email Evidence Preservation

Email evidence must be preserved carefully to maintain its integrity.

A basic process is:

```text
Identify
   ↓
Preserve
   ↓
Acquire
   ↓
Hash
   ↓
Analyze
   ↓
Document
   ↓
Report
```

Investigators should:

* Preserve the original message.
* Export the complete message where possible.
* Preserve complete headers.
* Record acquisition date and time.
* Calculate cryptographic hashes for exported evidence where appropriate.
* Maintain chain-of-custody documentation.
* Work on forensic copies rather than originals.
* Record all analysis actions.
* Avoid altering timestamps or message content.
* Secure sensitive information.

---

# 16. Chain of Custody

Chain of custody documents the handling of evidence from collection to presentation.

A basic evidence record should include:

| Field              | Example                        |
| ------------------ | ------------------------------ |
| Evidence ID        | EMAIL-001                      |
| Description        | Suspected phishing email       |
| Source             | Corporate mailbox              |
| Acquisition date   | 28 August 2026                 |
| Acquired by        | Investigator                   |
| Hash               | SHA-256 value                  |
| Storage location   | Forensic evidence repository   |
| Transfers          | Documented                     |
| Analysis performed | Header and attachment analysis |

A properly maintained chain of custody helps demonstrate that evidence has not been improperly altered or mishandled.

---

# 17. Case Study: Tracing a Phishing Campaign

## Scenario

An employee at an organization receives an email claiming to be from the organization's IT department.

The message states:

> "Your account will be disabled today. Click the link below to verify your account."

The employee forwards the message to the security team.

## Step 1: Preserve the Email

The investigator obtains the original email, including its complete headers and attachments.

A forensic copy is created.

## Step 2: Analyze the Sender

The visible sender is:

```text
it-support@company-example.com
```

However, the investigator notices that the `Reply-To` address is different:

```text
account.verify@external-example.net
```

This is a potential indicator of phishing.

## Step 3: Examine Authentication Results

The investigator examines SPF, DKIM, and DMARC results.

Suppose the results indicate that the message failed authentication checks or that the authenticated domain does not align with the claimed sender.

This increases suspicion that the message is fraudulent.

## Step 4: Examine the Received Headers

The investigator reconstructs the mail-server path from the `Received` headers.

The investigation identifies the infrastructure that delivered the message to the organization's mail server.

The investigator does not immediately conclude that the identified IP belongs to the attacker. Instead, the IP is treated as an indicator requiring further investigation.

## Step 5: Analyze the URL

The message contains a link such as:

```text
https://account-verify.example.net/login
```

The investigator extracts the URL without visiting it from a normal workstation.

The domain is compared with the legitimate organization's domain.

## Step 6: Analyze the Landing Page

Using a controlled analysis environment, investigators determine that the page imitates the organization's login portal and attempts to collect user credentials.

## Step 7: Search for Related Messages

The security team searches other mailboxes for:

* Same sender.
* Same subject.
* Same Message-ID.
* Same URL.
* Same attachment hash.
* Similar domains.

The search reveals that several employees received similar messages.

## Step 8: Correlate Evidence

The investigators correlate:

* Email headers.
* URL information.
* Mail-server logs.
* DNS information.
* Endpoint logs.
* Proxy logs.
* User reports.

This helps establish the scope and timeline of the campaign.

## Step 9: Report Findings

The final report documents:

* How the email was obtained.
* Relevant header information.
* Authentication results.
* URLs and domains.
* Attachment analysis.
* Evidence hashes.
* Timeline.
* Affected users.
* Indicators of compromise.
* Investigative conclusions.

---

# 18. Email Forensic Timeline

Timeline analysis can help investigators understand the sequence of events.

For example:

```text
09:10 — Phishing email sent
09:11 — Email received by organization
09:15 — Employee opens message
09:16 — Employee clicks malicious link
09:17 — Browser connects to phishing domain
09:20 — Security team receives user report
09:30 — Investigation begins
09:45 — Similar emails identified
10:15 — Malicious domain blocked
```

The timeline can be correlated with network and endpoint logs to determine whether credentials were submitted or malware was downloaded.

---

# 19. Challenges in Email Forensics

Email investigations can be difficult because of:

### 19.1 Spoofing

Attackers can manipulate visible sender information.

### 19.2 Encryption

TLS and end-to-end encryption can limit access to message contents.

### 19.3 Cloud Email

Modern services may store evidence across cloud infrastructure rather than on a single local computer.

### 19.4 Deleted Messages

Messages may have been permanently deleted or removed through retention policies.

### 19.5 Large Volumes of Email

Organizations may have millions of messages, making manual examination impractical.

### 19.6 Time Zones

Different systems may record timestamps in different time zones.

### 19.7 Shared or Compromised Accounts

An email account may have been accessed by multiple users or attackers.

### 19.8 Privacy

Email often contains sensitive personal and organizational information. Investigators must ensure that collection and examination are legally authorized and appropriately limited.

---

# 20. Best Practices

Investigators should follow these practices:

1. Preserve the original email whenever possible.
2. Collect complete headers rather than relying on screenshots.
3. Work from forensic copies.
4. Calculate and document hashes where appropriate.
5. Maintain chain of custody.
6. Examine SPF, DKIM, and DMARC results.
7. Treat the `From` field as a claim rather than proof of origin.
8. Analyze `Received` headers carefully.
9. Do not open suspicious attachments on production systems.
10. Analyze malicious links in controlled environments.
11. Correlate email evidence with network and endpoint evidence.
12. Record timestamps and time zones consistently.
13. Protect confidential information.
14. Document every significant investigative action.
15. Use multiple sources of evidence before drawing conclusions.

---

# 21. Practical Exercise

## Exercise: Analyze a Suspicious Email

Students should be provided with a legally obtained sample email in `.eml` format.

### Tasks

1. Identify the sender and recipient.
2. Extract the complete email headers.
3. List all `Received` headers.
4. Identify the Message-ID.
5. Examine SPF, DKIM, and DMARC results.
6. Identify the Reply-To address.
7. Extract URLs from the message.
8. Identify and hash any attachments.
9. Determine whether the message contains phishing indicators.
10. Create a timeline of the email activity.
11. Document findings in an investigation report.

### Suggested Tools

* Autopsy
* Wireshark
* Linux command-line tools
* Python
* Text editors
* Email header analysis tools
* Hashing utilities such as `sha256sum`

---

# 22. Example Linux Analysis

For a legally obtained `.eml` file, basic command-line analysis can be performed using tools such as:

```bash
cat suspicious-email.eml
```

To calculate a SHA-256 hash:

```bash
sha256sum suspicious-email.eml
```

To search for important headers:

```bash
grep -Ei "From:|To:|Date:|Subject:|Message-ID:|Reply-To:|Received:|Authentication-Results:" suspicious-email.eml
```

To search for URLs:

```bash
grep -Eo 'https?://[^ ]+' suspicious-email.eml
```

These commands provide basic triage. More advanced investigations should use appropriate forensic tools and controlled analysis environments.

---

# 23. Chapter Summary

Email forensics is an important area of digital investigation because email messages can provide evidence about communication, phishing, fraud, malware distribution, account compromise, and data leakage. Investigators should understand email architecture and protocols such as SMTP, IMAP, and POP3 before examining evidence. Email headers, metadata, authentication results, attachments, URLs, and server logs can help establish the origin, routing, and purpose of suspicious messages.

A reliable email investigation requires careful preservation of evidence, complete header collection, hashing where appropriate, chain-of-custody documentation, and correlation with other digital evidence. Investigators should also recognize the limitations of email evidence: sender addresses can be spoofed, timestamps can vary between systems, and technical infrastructure may not directly identify the person responsible. Proper forensic conclusions should therefore be based on multiple corroborating sources.

---

# 24. Key Terms

| Term             | Definition                                                                             |
| ---------------- | -------------------------------------------------------------------------------------- |
| Email Forensics  | Examination of email evidence for investigative purposes                               |
| SMTP             | Protocol primarily used to send and transfer email                                     |
| IMAP             | Protocol used to access and synchronize email stored on servers                        |
| POP3             | Protocol used to retrieve email from a mail server                                     |
| Header           | Technical information attached to an email message                                     |
| Metadata         | Information describing an email and its properties                                     |
| Spoofing         | Manipulating information to make communication appear to originate from another source |
| Phishing         | Deceptive communication intended to trick users into performing harmful actions        |
| SPF              | Mechanism for identifying authorized sending servers for a domain                      |
| DKIM             | Cryptographic email authentication mechanism                                           |
| DMARC            | Email authentication and policy framework using SPF and DKIM                           |
| Message-ID       | Identifier associated with an email message                                            |
| Chain of Custody | Documentation of evidence handling from collection to presentation                     |
| Hash             | Fixed-length value used to help verify data integrity                                  |
| BEC              | Business Email Compromise                                                              |
| MTA              | Mail Transfer Agent                                                                    |
| MUA              | Mail User Agent                                                                        |
| MDA              | Mail Delivery Agent                                                                    |

---

# References

1. Klensin, J. (2008). *Simple Mail Transfer Protocol*. RFC 5321. Internet Engineering Task Force (IETF).

2. Resnick, P. (2008). *Internet Message Format*. RFC 5322. Internet Engineering Task Force (IETF).

3. Myers, J., & Rose, M. (1996). *Post Office Protocol – Version 3*. RFC 1939. Internet Engineering Task Force (IETF).

4. Melnikov, A., & Cridland, A. (2013). *Internet Message Access Protocol – Version 4rev1*. RFC 3501. Internet Engineering Task Force (IETF).

5. Kitterman, S. (2014). *Sender Policy Framework (SPF) for Authorizing Use of Domains in Email, Version 1*. RFC 7208. Internet Engineering Task Force (IETF).

6. Crocker, D., Hansen, T., & Kucherawy, M. (2011). *DomainKeys Identified Mail (DKIM) Signatures*. RFC 6376. Internet Engineering Task Force (IETF).

7. Kucherawy, M., & Zwicky, E. (2015). *Domain-based Message Authentication, Reporting, and Conformance (DMARC)*. RFC 7489. Internet Engineering Task Force (IETF).

8. National Institute of Standards and Technology (NIST). *Guide to Integrating Forensic Techniques into Incident Response*. NIST Special Publication 800-86.

9. National Institute of Standards and Technology (NIST). *Guidelines on Mobile Device Forensics*. NIST Special Publication 800-101.

10. Casey, E. (2011). *Digital Evidence and Computer Crime: Forensic Science, Computers, and the Internet* (3rd ed.). Academic Press.

11. Carrier, B. (2005). *File System Forensic Analysis*. Addison-Wesley.

12. Garfinkel, S. (2010). Digital forensics research: The next 10 years. *Digital Investigation, 7*, S64–S73.

13. MITRE. *ATT&CK: Phishing (T1566)*. MITRE ATT&CK knowledge base.

14. Anti-Phishing Working Group (APWG). *Phishing Activity Trends Reports*.

---

[⬅ Previous Chapter](../05-legal-and-ethical-considerations/README.md) | [Back to Course Home](../README.md) | [Next Chapter ➡](../07-network-forensics/README.md)
