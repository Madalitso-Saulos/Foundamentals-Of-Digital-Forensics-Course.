# Chapter 7 — Network Forensics

[⬅ Previous](../06-email-forensics/README.md) | [Back to Course Home](../README.md) | [Next Chapter ➡](../08-windows-forensics/README.md)

---


## 1. Introduction to Network Forensics

Network forensics is a branch of digital forensics concerned with the **collection, preservation, examination, analysis, and reporting of network-based evidence**.

Unlike traditional disk forensics, where investigators examine persistent storage such as hard drives and USB devices, network forensics focuses primarily on communications occurring between systems.

Network evidence can help investigators answer questions such as:

* Who communicated with the compromised system?
* What IP addresses and ports were involved?
* When did the suspicious activity begin?
* Which protocols were used?
* What commands or requests were sent?
* Was data transferred outside the organization?
* Which systems were contacted after the initial compromise?
* Can the sequence of events be reconstructed?

Network forensics is particularly important during **incident response, intrusion investigations, malware analysis, data breach investigations, and cybercrime investigations**.

---

## 2. Network Forensics and Digital Forensics

Network forensics is not an isolated activity. It forms part of the broader digital forensic investigation process.

A typical investigation may involve evidence from:

```text
                    Digital Investigation
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
     Disk/File          Memory Forensics    Network Forensics
        │                   │                   │
   File systems        Processes          Packets
   Documents           Connections        Sessions
   Metadata            Credentials        DNS
   Logs                Malware            HTTP/HTTPS
        │                   │                   │
        └───────────────────┼───────────────────┘
                            │
                    Evidence Correlation
                            │
                       Timeline
                            │
                         Findings
                            │
                         Report
```

Network evidence can therefore confirm or contradict findings discovered on endpoints, servers, mobile devices, or cloud systems.

For example, an investigator may discover malware on a workstation and then use network evidence to determine whether that malware communicated with an external command-and-control server.

---

# 3. Sources of Network Forensic Evidence

Network evidence can come from several different sources.

## 3.1 Packet Captures

A packet capture contains network packets observed on a network interface.

Common packet capture formats include:

* `.pcap`
* `.pcapng`

Packet captures may contain:

* Source and destination IP addresses
* Source and destination ports
* Protocol information
* Packet timestamps
* TCP flags
* DNS requests
* HTTP requests
* TLS metadata
* ARP traffic
* ICMP traffic
* Application data where it is not encrypted

Packet captures are particularly valuable because they provide detailed evidence about network communications.

---

## 3.2 Firewall Logs

Firewalls record traffic that is allowed, blocked, or otherwise processed by security policies.

Typical information includes:

```text
Timestamp
Source IP
Destination IP
Source Port
Destination Port
Protocol
Action
Rule ID
Interface
```

For example:

```text
2026-08-29 18:42:11
SRC=192.168.1.25
DST=203.0.113.50
SPT=49152
DPT=443
PROTO=TCP
ACTION=ALLOW
```

Firewall logs can help establish whether a suspected system communicated with an external host.

---

## 3.3 Router and Switch Logs

Network infrastructure devices can provide evidence about:

* Routing changes
* Interface activity
* MAC addresses
* DHCP assignments
* Authentication events
* Configuration changes
* Network failures
* Administrative access

These records can help investigators determine how devices were connected and whether network infrastructure was modified during an incident.

---

## 3.4 IDS/IPS Logs

Intrusion Detection Systems (IDS) and Intrusion Prevention Systems (IPS) monitor network activity for suspicious behavior.

Examples of alerts include:

* Port scanning
* Brute-force attempts
* Exploit attempts
* Malware traffic
* Suspicious DNS activity
* Known malicious IP addresses
* Command-and-control communication

IDS/IPS alerts should not automatically be treated as proof of compromise. They should be correlated with other evidence.

---

## 3.5 DNS Logs

DNS evidence is extremely useful during investigations because malware and attackers frequently use DNS to locate services or communicate with external infrastructure.

Investigators may examine:

* Requested domains
* Query timestamps
* Response addresses
* Query types
* Unusual domain patterns
* High-volume DNS requests
* Newly observed domains

Suspicious DNS behavior can include unusually long domain names, random-looking subdomains, frequent queries, or communication with known malicious infrastructure.

---

## 3.6 DHCP Logs

DHCP logs can associate IP addresses with devices.

For example:

```text
Timestamp              IP Address       MAC Address
-----------------------------------------------------
08:10:01               192.168.1.20     AA:BB:CC:11:22:33
08:12:44               192.168.1.21     AA:BB:CC:44:55:66
```

This can be important because an IP address alone may not identify the physical device or user responsible for network activity.

---

# 4. Network Traffic Fundamentals

To analyze network evidence effectively, an investigator needs a basic understanding of network communication.

## 4.1 IP Addresses

An IP address identifies a network interface participating in IP communication.

Examples:

```text
IPv4:
192.168.1.10
10.0.0.25

IPv6:
2001:db8::10
```

Investigators should distinguish between:

* Private IP addresses
* Public IP addresses
* Loopback addresses
* Multicast addresses
* Broadcast addresses

Private IPv4 ranges include:

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

---

## 4.2 MAC Addresses

A MAC address identifies a network interface at the data-link layer.

Example:

```text
00:1A:2B:3C:4D:5E
```

MAC addresses can be useful for correlating network activity with specific devices on local networks.

---

## 4.3 Ports

Ports identify network services or applications.

Common examples include:

|  Port | Protocol/Service |
| ----: | ---------------- |
| 20/21 | FTP              |
|    22 | SSH              |
|    23 | Telnet           |
|    25 | SMTP             |
|    53 | DNS              |
|    80 | HTTP             |
|   110 | POP3             |
|   143 | IMAP             |
|   443 | HTTPS            |
|   445 | SMB              |
|  3389 | RDP              |

A port number alone does not prove which application is running because services can operate on non-standard ports.

---

# 5. Packet Capture

Packet capture is one of the most important techniques in network forensics.

A packet capture records network traffic observed by a monitoring interface.

Common tools include:

* Wireshark
* tcpdump
* TShark
* dumpcap

### Example: tcpdump

```bash
sudo tcpdump -i eth0
```

Capture traffic to a file:

```bash
sudo tcpdump -i eth0 -w investigation.pcap
```

Capture only TCP traffic:

```bash
sudo tcpdump -i eth0 tcp -w tcp-traffic.pcap
```

Capture traffic involving a specific host:

```bash
sudo tcpdump -i eth0 host 192.168.1.50
```

Capture traffic involving a specific port:

```bash
sudo tcpdump -i eth0 port 443
```

The resulting capture can be examined later using Wireshark or TShark.

---

# 6. Wireshark

[Wireshark](https://www.wireshark.org/) is one of the most widely used tools for network traffic analysis.

It provides a graphical interface for examining packet captures and supports a large number of network protocols.

A typical Wireshark workflow is:

```text
Obtain Capture
      ↓
Open PCAP/PCAPNG
      ↓
Identify Hosts
      ↓
Identify Protocols
      ↓
Apply Filters
      ↓
Follow Conversations
      ↓
Extract Evidence
      ↓
Correlate Findings
      ↓
Document Results
```

---

## 6.1 Useful Wireshark Filters

Display traffic from a specific IP:

```text
ip.addr == 192.168.1.50
```

Source IP:

```text
ip.src == 192.168.1.50
```

Destination IP:

```text
ip.dst == 192.168.1.50
```

TCP traffic:

```text
tcp
```

UDP traffic:

```text
udp
```

DNS traffic:

```text
dns
```

HTTP traffic:

```text
http
```

TLS traffic:

```text
tls
```

SSH traffic:

```text
ssh
```

Traffic on a specific port:

```text
tcp.port == 443
```

Traffic between two hosts:

```text
ip.addr == 192.168.1.10 && ip.addr == 192.168.1.20
```

Packets containing a specific string:

```text
tcp contains "password"
```

> **Note:** Searching packet contents is only useful when the relevant payload is visible. Encryption can prevent investigators from viewing application data.

---

# 7. Protocol Analysis

Understanding protocols is essential for interpreting network evidence.

## 7.1 TCP

Transmission Control Protocol provides reliable, connection-oriented communication.

Important TCP fields include:

* Source port
* Destination port
* Sequence number
* Acknowledgment number
* Flags
* Window size

Important TCP flags include:

| Flag | Meaning                          |
| ---- | -------------------------------- |
| SYN  | Initiates a connection           |
| ACK  | Acknowledges data                |
| FIN  | Gracefully terminates connection |
| RST  | Resets connection                |
| PSH  | Pushes data to application       |
| URG  | Urgent data                      |

A common TCP connection begins with the three-way handshake:

```text
Client                  Server
  │                       │
  │────── SYN ───────────>│
  │<──── SYN + ACK ───────│
  │────── ACK ───────────>│
  │                       │
  │    Data Transfer      │
```

Investigators can use this information to reconstruct communication between systems.

---

## 7.2 UDP

UDP is connectionless and does not provide the same delivery guarantees as TCP.

It is commonly used by:

* DNS
* DHCP
* VoIP
* Streaming applications
* Some VPN and tunneling technologies

Because UDP does not establish a traditional connection, analysis often focuses on individual flows and request/response patterns.

---

## 7.3 DNS

DNS converts domain names into IP addresses.

Example:

```text
User → DNS Query → example.com
DNS Server → Response → 93.184.216.34
```

Investigators can use DNS traffic to identify:

* Domains contacted by a device
* Malware infrastructure
* Suspicious domains
* Potential command-and-control channels
* Data exfiltration through DNS

---

## 7.4 HTTP and HTTPS

HTTP traffic can contain valuable information such as:

* URLs
* HTTP methods
* Host headers
* User-Agent values
* Cookies
* Parameters
* Server responses

Common HTTP methods include:

```text
GET
POST
PUT
DELETE
HEAD
```

HTTPS encrypts HTTP traffic using TLS, which significantly limits visibility into application-layer content without appropriate decryption keys or other authorized visibility mechanisms.

---

## 7.5 ARP

Address Resolution Protocol maps IPv4 addresses to MAC addresses on local networks.

Example:

```text
Who has 192.168.1.1?
192.168.1.1 is at AA:BB:CC:DD:EE:FF
```

Unusual ARP activity can be relevant when investigating:

* ARP spoofing
* Man-in-the-middle attacks
* Network impersonation
* Rogue devices

---

# 8. Network Flow Analysis

Full packet capture is not always available.

Network flow records provide summarized information about communication.

A flow may contain:

```text
Source IP
Destination IP
Source Port
Destination Port
Protocol
Start Time
End Time
Bytes
Packets
```

Examples of flow technologies include:

* NetFlow
* IPFIX
* sFlow

Flow data generally contains less detail than full packet captures but can be much easier to store and analyze across large networks.

---

# 9. Identifying Indicators of Compromise

Network forensic investigators look for **Indicators of Compromise (IOCs)** and suspicious behavioral patterns.

Examples include:

* Communication with known malicious IP addresses
* Connections to suspicious domains
* Unexpected external connections
* Repeated failed connections
* Unusual destination ports
* Large outbound data transfers
* Periodic beaconing
* Suspicious DNS requests
* Port scanning
* Unexpected remote administration
* Lateral movement between internal systems

An IOC should be evaluated in context. A single suspicious connection does not necessarily establish that a system is compromised.

---

# 10. Detecting Network Scanning

Attackers may scan networks to discover available systems and services.

Common scanning behavior includes:

```text
Host A
  │
  ├── SYN → Host B:22
  ├── SYN → Host B:80
  ├── SYN → Host B:443
  ├── SYN → Host B:445
  └── SYN → Host B:3389
```

Indicators of scanning may include:

* One source contacting many ports
* One source contacting many hosts
* Large numbers of connection attempts
* Repeated SYN packets
* Many failed connections

Tools such as Wireshark can help investigators identify these patterns in packet captures.

---

# 11. Detecting Brute-Force Activity

Brute-force attacks often generate repeated authentication attempts.

Examples include attempts against:

* SSH
* FTP
* RDP
* Web applications
* VPN services
* Email services

Investigators may look for:

```text
Many connections
        ↓
Same destination
        ↓
Same service
        ↓
Repeated authentication attempts
        ↓
Possible successful login
```

Network evidence should be correlated with authentication logs to determine whether an account was actually compromised.

---

# 12. Detecting Lateral Movement

Lateral movement occurs when an attacker moves from one compromised system to other systems within a network.

For example:

```text
Internet
   │
   ↓
Web Server
   │
   ↓
Application Server
   │
   ↓
Database Server
```

Network forensic evidence may reveal:

* SMB connections
* RDP sessions
* SSH connections
* Remote administration
* Internal scanning
* Authentication attempts
* Unexpected host-to-host communication

Investigators should construct a timeline showing how communication progressed between systems.

---

# 13. Session Reconstruction

One of the most useful capabilities of network forensic analysis is reconstructing conversations.

In Wireshark, investigators can use:

**Analyze → Follow → TCP Stream**

This can reconstruct data exchanged during a TCP session when the relevant content is available.

Session reconstruction may reveal:

* HTTP requests
* Commands
* Responses
* Authentication information
* Transferred content
* Malware communication

However, reconstruction is limited when traffic is encrypted, incomplete, or captured only partially.

---

# 14. File Extraction from Network Traffic

Some packet captures contain files transferred over the network.

Depending on the protocol and capture contents, investigators may be able to identify or extract:

* Images
* Documents
* Archives
* Executables
* Scripts
* Other transferred files

Before analyzing an extracted file, investigators should preserve the original evidence and calculate appropriate hashes.

Example:

```bash
sha256sum extracted_file.bin
```

The resulting hash can be recorded in the investigation documentation.

---

# 15. Network Timeline Reconstruction

A network timeline combines timestamps from multiple sources to reconstruct events.

Example:

| Time     | Event                           |
| -------- | ------------------------------- |
| 09:12:04 | DNS query for suspicious domain |
| 09:12:07 | TCP connection established      |
| 09:12:10 | HTTP request sent               |
| 09:13:22 | Large outbound transfer         |
| 09:14:01 | Internal connection to server   |
| 09:15:32 | Authentication attempt          |
| 09:17:08 | Connection terminated           |

Timeline analysis can help investigators determine:

1. Initial communication
2. Possible compromise
3. Command execution
4. Lateral movement
5. Data collection
6. Exfiltration
7. Attacker exit or connection termination

---

# 16. Log Correlation

Network evidence becomes more valuable when correlated with other sources.

For example:

```text
PCAP
 │
 ├── Source IP
 ├── Destination IP
 └── Timestamp
        │
        ↓
Firewall Logs
        │
        ├── Connection allowed
        └── Connection blocked
        │
        ↓
Authentication Logs
        │
        └── Successful login
        │
        ↓
Endpoint Logs
        │
        └── Suspicious process
        │
        ↓
Investigation Timeline
```

Correlation can reduce false positives and provide stronger evidence about what happened.

---

# 17. Common Network Forensic Tools

| Tool             | Primary Use                                       |
| ---------------- | ------------------------------------------------- |
| **Wireshark**    | Graphical packet analysis                         |
| **TShark**       | Command-line packet analysis                      |
| **tcpdump**      | Packet capture and filtering                      |
| **Zeek**         | Network security monitoring and protocol analysis |
| **Suricata**     | IDS/IPS and network threat detection              |
| **Snort**        | Network intrusion detection                       |
| **NetworkMiner** | Network forensic analysis and artifact extraction |
| **Arkime**       | Large-scale packet capture and session analysis   |
| **ngrep**        | Searching network traffic                         |
| **tshark**       | Command-line Wireshark analysis                   |

Tools should be selected according to the investigation requirements, available evidence, network architecture, and legal authority.

---

# 18. Practical TShark Examples

List interfaces:

```bash
tshark -D
```

Read a capture:

```bash
tshark -r investigation.pcap
```

Display only DNS packets:

```bash
tshark -r investigation.pcap -Y dns
```

Display HTTP traffic:

```bash
tshark -r investigation.pcap -Y http
```

Extract source and destination IP addresses:

```bash
tshark -r investigation.pcap \
-T fields \
-e frame.time \
-e ip.src \
-e ip.dst
```

Count DNS packets:

```bash
tshark -r investigation.pcap -Y dns | wc -l
```

These commands are useful when analyzing large captures where manually examining packets in a graphical interface would be inefficient.

---

# 19. Network Forensics Investigation Workflow

A structured network forensic investigation can follow these stages:

## Step 1 — Preparation

Determine:

* Investigation scope
* Systems involved
* Available network evidence
* Legal authority
* Collection requirements
* Required tools

---

## Step 2 — Evidence Identification

Identify available evidence sources:

```text
PCAP
Firewall Logs
IDS/IPS Logs
DNS Logs
DHCP Logs
Router Logs
Proxy Logs
VPN Logs
Flow Records
Endpoint Logs
```

---

## Step 3 — Evidence Collection

Collect evidence using appropriate and authorized methods.

For packet captures:

```bash
tcpdump -i eth0 -w evidence.pcap
```

The original evidence should be protected from unnecessary modification.

---

## Step 4 — Preservation

Preserve collected evidence and document:

* Date and time
* Collection method
* Investigator
* Source system
* File name
* Hash value
* Storage location

Example:

```bash
sha256sum evidence.pcap
```

---

## Step 5 — Examination

Identify:

* Hosts
* Protocols
* Ports
* Conversations
* DNS queries
* Suspicious connections
* Large transfers
* Authentication attempts

---

## Step 6 — Analysis

Determine:

* What happened?
* When did it happen?
* Which systems were involved?
* How did the attacker communicate?
* What systems were accessed?
* Was data transferred?
* What indicators of compromise were observed?

---

## Step 7 — Correlation

Compare network evidence with:

* System logs
* Authentication records
* Disk artifacts
* Memory evidence
* Email evidence
* Malware analysis
* Firewall records

---

## Step 8 — Reporting

The final report should clearly document:

* Investigation scope
* Evidence collected
* Tools used
* Methodology
* Findings
* Timeline
* Indicators of compromise
* Limitations
* Conclusions
* Recommendations

---

# 20. Challenges in Network Forensics

## 20.1 Encryption

Modern networks rely heavily on encryption.

Examples include:

* HTTPS
* SSH
* TLS
* VPNs
* Encrypted messaging

Encryption can prevent investigators from directly viewing application-layer content.

However, useful metadata may still be available, including:

* IP addresses
* Ports
* Timestamps
* Packet sizes
* Connection duration
* TLS metadata
* DNS activity

---

## 20.2 Large Data Volumes

Enterprise networks can generate enormous quantities of traffic.

Investigators may need to process:

```text
Gigabytes → Terabytes → Petabytes
```

Efficient filtering, indexing, flow analysis, and automated processing are therefore important.

---

## 20.3 Volatile Evidence

Network traffic can disappear quickly.

Unlike a disk image, network traffic may not remain available after the event unless it was captured or logged.

This makes timely collection extremely important.

---

## 20.4 Network Address Translation

NAT can make attribution more difficult because multiple internal systems may appear to use the same public IP address.

Investigators may need to correlate:

```text
Public IP
     ↓
NAT Logs
     ↓
Internal IP
     ↓
DHCP Logs
     ↓
MAC Address
     ↓
Device
```

---

## 20.5 Missing or Incomplete Captures

A packet capture may begin after an incident has already started or may capture only part of a communication.

This can make it impossible to reconstruct the complete sequence of events.

Investigators should therefore identify limitations explicitly in their reports.

---

## 20.6 Spoofed Information

Attackers may manipulate or spoof certain network information.

Examples include:

* Spoofed IP addresses
* Forged DNS information
* Manipulated HTTP headers
* Spoofed MAC addresses

Network evidence should therefore be correlated with independent sources wherever possible.

---

# 21. Best Practices

When conducting network forensic investigations:

1. Obtain appropriate authorization before collecting traffic.
2. Define the scope of collection.
3. Minimize unnecessary collection of unrelated data.
4. Preserve original evidence.
5. Calculate cryptographic hashes where appropriate.
6. Record timestamps and time zones.
7. Document collection methods.
8. Use validated forensic tools.
9. Correlate multiple evidence sources.
10. Distinguish observations from conclusions.
11. Record limitations and uncertainty.
12. Maintain chain of custody.
13. Protect sensitive network data.
14. Keep detailed investigation notes.
15. Produce reproducible findings where possible.

---

# 22. Example Investigation Scenario

Consider an organization that discovers that a workstation has been compromised.

The security team provides a packet capture from the period surrounding the suspected compromise.

### Initial observation

The investigator identifies:

```text
Internal Host:
192.168.10.25

External Host:
203.0.113.50
```

The workstation repeatedly communicates with the external IP over TCP.

### Investigation

The investigator examines:

```text
DNS queries
     ↓
TCP connections
     ↓
TLS metadata
     ↓
Connection frequency
     ↓
Outbound data volume
     ↓
Firewall logs
     ↓
Endpoint logs
```

The analysis reveals that the workstation first resolved a suspicious domain and subsequently established repeated connections to an external server.

The investigator then correlates timestamps with endpoint logs and discovers that a suspicious executable was launched shortly before the first external connection.

### Result

Network evidence alone may not prove the complete attack chain. However, when combined with endpoint and authentication evidence, it can support a stronger reconstruction:

```text
Malicious File
      ↓
Execution
      ↓
DNS Resolution
      ↓
External Connection
      ↓
Command-and-Control
      ↓
Internal Reconnaissance
      ↓
Possible Lateral Movement
```

This demonstrates why **evidence correlation** is central to network forensics.

---

# 23. Legal and Ethical Considerations

Network traffic can contain highly sensitive information, including:

* Personal communications
* Credentials
* Financial information
* Customer data
* Internal business information
* Medical or confidential records

Investigators must therefore ensure that network monitoring and evidence collection are performed under appropriate legal authority and organizational policy.

Investigators should also avoid collecting unnecessary information and should protect evidence from unauthorized disclosure.

The principles discussed in the chapter on [Legal and Ethical Considerations](../05-legal-and-ethical-considerations/README.md) should be applied throughout network forensic investigations.


---

# 26. Chapter Summary

Network forensics focuses on the collection and analysis of network-based evidence to reconstruct communications and investigate security incidents. Packet captures, firewall logs, DNS records, DHCP records, router logs, IDS/IPS alerts, and network flow data can provide valuable information about systems, users, connections, protocols, and attack activity.

Tools such as Wireshark, tcpdump, TShark, Zeek, Suricata, and Snort can help investigators identify suspicious traffic, reconstruct sessions, detect indicators of compromise, and develop incident timelines.

A successful network forensic investigation requires more than simply identifying suspicious packets. Investigators must preserve evidence, understand network protocols, correlate multiple sources, document their methodology, and clearly distinguish evidence-based conclusions from assumptions.

The major challenges include encryption, large traffic volumes, volatile evidence, NAT, incomplete captures, and attribution difficulties. Despite these challenges, network forensics remains an important component of modern digital investigations because network evidence can reveal how systems communicated and how an attack progressed across an environment.

---

# 27. Key Terms

| Term                       | Definition                                                                       |
| -------------------------- | -------------------------------------------------------------------------------- |
| **Network Forensics**      | Examination of network communications for investigative purposes.                |
| **Packet**                 | A unit of data transmitted across a network.                                     |
| **PCAP**                   | A file containing captured network packets.                                      |
| **Flow**                   | A summarized record of communication between network endpoints.                  |
| **Wireshark**              | Graphical network protocol analyzer.                                             |
| **tcpdump**                | Command-line packet capture and analysis tool.                                   |
| **TShark**                 | Command-line version of Wireshark's analysis capabilities.                       |
| **IDS**                    | Intrusion Detection System.                                                      |
| **IPS**                    | Intrusion Prevention System.                                                     |
| **IOC**                    | Indicator of Compromise.                                                         |
| **TCP**                    | Transmission Control Protocol.                                                   |
| **UDP**                    | User Datagram Protocol.                                                          |
| **DNS**                    | Domain Name System.                                                              |
| **ARP**                    | Address Resolution Protocol.                                                     |
| **NAT**                    | Network Address Translation.                                                     |
| **C2**                     | Command-and-Control infrastructure used to communicate with compromised systems. |
| **Lateral Movement**       | Movement from one compromised system to other systems within a network.          |
| **Data Exfiltration**      | Unauthorized transfer of data from an environment.                               |
| **Session Reconstruction** | Reassembling network communications to understand a conversation or transaction. |

---

# 28. Further Reading and Resources

### Network Analysis Tools

* [Wireshark](https://www.wireshark.org/) — Network protocol analyzer and packet analysis tool.
* [tcpdump](https://www.tcpdump.org/) — Command-line packet capture utility.
* [Zeek](https://zeek.org/) — Network security monitor and traffic analysis platform.
* [Suricata](https://suricata.io/) — Open-source IDS/IPS and network security engine.
* [Snort](https://www.snort.org/) — Network intrusion detection and prevention system.

### Recommended Topics for Further Study

* TCP/IP protocol analysis
* Network intrusion detection
* Malware network traffic analysis
* DNS forensics
* TLS and encrypted traffic analysis
* Network flow analysis
* Security Information and Event Management (SIEM)
* Incident response
* Threat hunting
* Network-based indicators of compromise
* Cloud network forensics

See the [main References section](../../README.md#references) in the course README for additional digital forensics resources.

---
## 29. References

1. Kent, K., Chevalier, S., Grance, T., & Dang, H. (2006). *Guide to Integrating Forensic Techniques into Incident Response*. NIST Special Publication 800-86. National Institute of Standards and Technology.
   [NIST SP 800-86 — Official Publication](https://csrc.nist.gov/pubs/sp/800/86/final?utm_source=chatgpt.com)

2. National Institute of Standards and Technology (NIST). (2006). *Guide to Integrating Forensic Techniques into Incident Response*. The guide provides practical guidance for computer and network forensics, including evidence collection, examination, analysis, and reporting.
[NIST Forensics Guide](https://www.nist.gov/publications/guide-integrating-forensic-techniques-incident-response?utm_source=chatgpt.com)

3. Sharpe, R., Warnicke, E., & Lamping, U. *Wireshark User's Guide*. Wireshark Foundation. The guide documents packet capture, protocol analysis, display filters, conversations, endpoints, statistics, and related network analysis features.
   [Wireshark User's Guide](https://www.wireshark.org/docs/wsug_html/?utm_source=chatgpt.com)

4. Wireshark Foundation. *Wireshark Documentation*. Documentation covering Wireshark, TShark, dumpcap, tcpdump integration, capture files, display filters, and other network analysis utilities.
   [Wireshark Documentation](https://www.wireshark.org/docs/?utm_source=chatgpt.com)

5. Mockapetris, P. (1987). *Domain Names — Concepts and Facilities*. RFC 1034. Internet Engineering Task Force (IETF).
   [RFC 1034 — DNS Concepts and Facilities](https://www.rfc-editor.org/rfc/rfc1034?utm_source=chatgpt.com)

6. Mockapetris, P. (1987). *Domain Names — Implementation and Specification*. RFC 1035. Internet Engineering Task Force (IETF).
   [RFC 1035 — DNS Implementation and Specification](https://www.rfc-editor.org/rfc/rfc1035?utm_source=chatgpt.com)

7. National Institute of Standards and Technology (NIST). *Computer Security Incident Handling Guide*. NIST Special Publication 800-61. This provides additional guidance for incident handling and can be used alongside network forensic procedures.
   [NIST SP 800-61](https://csrc.nist.gov/pubs/sp/800/61?utm_source=chatgpt.com)



[Next Chapter ➡](../08-windows-forensics/README.md)
