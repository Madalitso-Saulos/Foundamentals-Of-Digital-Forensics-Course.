# Chapter 3 — Cybercrime Fundamentals

[⬅ Previous Chapter](../02-digital-forensics-investigation-process/README.md) | [Back to Course Home](../README.md) | [Next Chapter ➡](../04-digital-evidence/README.md)


##  Understanding Cybercrime

Cybercrime refers to criminal activity in which computers, computer networks, digital devices, or online services are used as a **target, tool, or environment** for committing an offence.

The growth of the Internet, smartphones, cloud computing, social media, online banking, cryptocurrencies, and interconnected systems has created significant opportunities for legitimate activities. At the same time, these technologies have created new opportunities for criminals.

Cybercrime can range from relatively simple activities such as unauthorized access to an account to highly organized attacks involving ransomware, financial fraud, malware campaigns, data breaches, and attacks against critical infrastructure.

For digital forensic investigators, cybercrime is particularly important because many modern crimes leave behind **digital traces**. These traces may exist on computers, smartphones, servers, cloud platforms, network devices, databases, applications, removable storage, and online services.

Digital forensics therefore plays an important role in reconstructing what happened, identifying relevant evidence, establishing timelines, determining how systems were compromised, and supporting legal or organizational decision-making.

---

##  Definition of Cybercrime

There is no single universally accepted definition of cybercrime. In general, however, cybercrime can be understood as criminal activity involving information and communication technologies.

A useful way of understanding cybercrime is to consider three roles that technology can play:

### 1. Computer as the Target

The computer or information system itself is attacked.

Examples include:

* Unauthorized access.
* Denial-of-service attacks.
* Malware infections.
* Data destruction.
* Website defacement.
* Exploitation of software vulnerabilities.

### 2. Computer as the Tool

Technology is used to facilitate another crime.

Examples include:

* Online fraud.
* Phishing.
* Identity theft.
* Cyberstalking.
* Online extortion.
* Distribution of illegal content.

### 3. Computer as the Environment

The digital environment provides the location or platform where criminal activity occurs.

Examples include:

* Criminal marketplaces.
* Online scams.
* Cryptocurrency-related crimes.
* Social media-based fraud.
* Illegal file-sharing communities.

This distinction is useful because it helps investigators determine what systems, devices, accounts, logs, and other sources may contain relevant evidence.

---

# 3. Characteristics of Cybercrime

Cybercrime differs from many traditional crimes because of the characteristics of digital technology. These include

## 3.1 Borderless Nature

A cybercriminal may operate in one country, use infrastructure hosted in another country, target victims in several countries, and route traffic through additional jurisdictions.

This creates significant challenges involving:

* Jurisdiction.
* International cooperation.
* Evidence acquisition.
* Data protection.
* Legal authority.

## 3.2 Speed

Digital attacks can occur extremely quickly. A malicious program can infect thousands of systems within a short period of time.

Consequently, investigators may need to preserve volatile evidence before it disappears.

## 3.3 Anonymity and Pseudonymity

Cybercriminals may attempt to hide their identities through:

* Proxy servers.
* VPN services.
* Tor.
* Compromised systems.
* Disposable accounts.
* Cryptocurrency.
* Multiple online identities.

Anonymity does not necessarily mean that an attacker cannot be identified. Investigators can potentially correlate multiple digital traces to establish attribution.

## 3.4 Scalability

Traditional crimes may require significant physical resources to affect many victims. Cybercrime can be automated and scaled considerably.

For example, a phishing campaign can send thousands or millions of messages automatically.

## 3.5 Volatility of Evidence

Digital evidence can be changed, deleted, overwritten, or automatically expired.

Examples include:

* RAM contents.
* Network connections.
* Temporary files.
* Authentication logs.
* Browser sessions.
* Cloud logs.
* Application data.

This makes timely evidence preservation particularly important.

## 3.6 Automation

Cybercriminals frequently automate attacks using scripts, bots, malware, and automated exploitation tools.

Automation allows attackers to:

* Scan networks.
* Identify vulnerable systems.
* Send phishing messages.
* Attempt passwords.
* Deploy malware.
* Collect information.

---

# 4. Classification of Cybercrime

Cybercrime can be classified in several ways. One useful classification divides cybercrime according to the primary victim or target.

## 4.1 Crimes Against Individuals

These crimes target individual users.

Examples include:

* Identity theft.
* Cyberstalking.
* Online harassment.
* Phishing.
* Account takeover.
* Cyberbullying.
* Online impersonation.
* Romance scams.

Digital evidence may include:

* Emails.
* Social media messages.
* Chat histories.
* Browser history.
* Login records.
* IP addresses.
* Mobile-device artefacts.

---

## 4.2 Crimes Against Property

These crimes involve the theft, destruction, alteration, or unauthorized use of digital or physical property.

Examples include:

* Intellectual property theft.
* Data theft.
* Ransomware.
* Software piracy.
* Unauthorized modification of databases.
* Destruction of computer systems.

Investigators may examine:

* File systems.
* Database records.
* Malware.
* Access logs.
* Network traffic.
* File metadata.
* Cloud storage.

---

## 4.3 Crimes Against Organizations

Organizations can be targeted because they possess valuable information, financial resources, intellectual property, and infrastructure.

Examples include:

* Data breaches.
* Business email compromise.
* Ransomware attacks.
* Insider threats.
* Network intrusion.
* Supply-chain attacks.
* Website attacks.
* Denial-of-service attacks.

Organizations may need digital forensic investigations to determine:

* How the attacker gained access.
* Which systems were compromised.
* What information was accessed.
* Whether data was exfiltrated.
* How long the attacker remained in the environment.
* What actions the attacker performed.

---

## 4.4 Crimes Against Government

Government institutions can be targeted for financial, political, intelligence, or disruptive purposes.

Examples include:

* Cyber espionage.
* Attacks against government websites.
* Data breaches.
* Disruption of public services.
* Unauthorized access to government systems.

Attacks against government infrastructure can have significant consequences because government systems may contain sensitive personal, financial, security, or administrative information.

---

## 4.5 Crimes Against Society

Some cybercrimes affect society more broadly.

Examples include:

* Online distribution of illegal content.
* Large-scale financial fraud.
* Cyberterrorism.
* Attacks against critical infrastructure.
* Coordinated misinformation campaigns.
* Criminal marketplaces.

These cases can involve large quantities of digital evidence distributed across multiple systems and jurisdictions.

---

# 5. Common Types of Cybercrime

## 5.1 Hacking and Unauthorized Access

Unauthorized access occurs when an individual gains access to a computer system, account, network, or service without permission.

Attackers may obtain access through:

* Stolen credentials.
* Weak passwords.
* Vulnerable applications.
* Social engineering.
* Malware.
* Exploitation of security vulnerabilities.

From a forensic perspective, investigators may examine authentication logs, system logs, network traffic, account activity, and file-system changes.

---

## 5.2 Phishing

Phishing is a form of social engineering in which attackers attempt to trick victims into revealing sensitive information or performing an action.

A phishing message may attempt to obtain:

* Usernames.
* Passwords.
* Banking information.
* Authentication codes.
* Personal information.

Phishing investigations can involve examination of email headers, URLs, domains, attachments, browser history, and communication records.

---

## 5.3 Malware

Malware is malicious software designed to perform unauthorized or harmful activities.

Common categories include:

### Virus

Malicious code capable of replicating by attaching itself to other files or programs.

### Worm

Malware capable of spreading between systems, often without requiring a user to manually execute an infected file.

### Trojan

Malicious software disguised as legitimate software or files.

### Spyware

Software designed to monitor or collect information about users or systems.

### Rootkit

Software designed to maintain privileged access while attempting to hide its presence.

### Ransomware

Malware that prevents access to systems or data, commonly by encrypting files, and demands payment from victims.

Malware investigations may involve:

* Malware samples.
* File hashes.
* Persistence mechanisms.
* Registry artefacts.
* Process information.
* Network connections.
* Command-and-control communication.

---

# 6. Identity Theft

Identity theft occurs when an attacker obtains and uses another person's identity or personal information without authorization.

Stolen information may include:

* Names.
* Identification information.
* Passwords.
* Financial information.
* Email accounts.
* Social media credentials.

Cybercriminals may use stolen identities to commit financial fraud, create fraudulent accounts, impersonate victims, or conduct additional crimes.

---

# 7. Online Fraud

Online fraud involves using digital technologies to deceive victims for financial or other benefits.

Examples include:

* Online shopping scams.
* Investment scams.
* Romance scams.
* Lottery scams.
* Banking fraud.
* Business email compromise.
* Payment fraud.

Investigators may need to correlate:

* Communication records.
* Bank transactions.
* Device artefacts.
* IP addresses.
* Email accounts.
* Social media accounts.
* Cryptocurrency transactions.

---

# 8. Denial-of-Service and Distributed Denial-of-Service

A **Denial-of-Service (DoS)** attack attempts to make a system, service, or network unavailable.

A **Distributed Denial-of-Service (DDoS)** attack uses multiple systems or devices to generate traffic or requests against a target.

Potential targets include:

* Websites.
* Online applications.
* DNS infrastructure.
* Gaming services.
* Government portals.
* Financial services.

Forensic analysis may involve:

* Firewall logs.
* Web server logs.
* Network-flow records.
* Packet captures.
* Authentication logs.
* Traffic patterns.

---

# 9. Cyberstalking and Online Harassment

Cyberstalking involves using digital technologies to repeatedly monitor, threaten, intimidate, or harass an individual.

Evidence can include:

* Emails.
* Social media messages.
* SMS messages.
* Phone records.
* Chat applications.
* Location information.
* Browser history.
* Account login records.

Investigators should preserve evidence carefully because online communications can be deleted or modified.

---

# 10. Data Breaches and Data Theft

A data breach occurs when unauthorized individuals gain access to protected or sensitive information.

Stolen information can include:

* Customer records.
* Password databases.
* Financial information.
* Medical records.
* Intellectual property.
* Government information.

A forensic investigation attempts to determine the:

1. Initial attack vector.
2. Compromised accounts.
3. Compromised systems.
4. Data accessed.
5. Data exfiltrated.
6. Attacker activities.
7. Duration of compromise.

---

# 11. Cyber Extortion

Cyber extortion occurs when criminals use digital means to threaten victims in order to obtain money or another benefit.

A common example is ransomware.

Other forms include:

* Threatening to publish stolen information.
* Threatening to disrupt services.
* Threatening to release private communications.
* Blackmail involving stolen digital material.

Cyber extortion investigations require careful preservation of communications, payment information, system logs, and affected files.

---

# 12. Cybercrime Motivations

Cybercriminals may have different motivations.

## 12.1 Financial Gain

Financial gain is one of the most common motivations.

Examples include:

* Ransomware.
* Banking fraud.
* Credential theft.
* Cryptocurrency theft.
* Online scams.

## 12.2 Espionage

Attackers may steal confidential information for intelligence or competitive advantage.

Targets can include:

* Governments.
* Military organizations.
* Corporations.
* Research institutions.

## 12.3 Political or Ideological Motivation

Some attacks are motivated by political or ideological beliefs.

Examples include:

* Website defacement.
* DDoS attacks.
* Data leaks.
* Hacktivism.

## 12.4 Revenge

Employees, former employees, or individuals with personal grievances may attack systems to cause damage or disruption.

## 12.5 Recognition and Challenge

Some attackers may be motivated by curiosity, technical challenge, status, or recognition within an online community.

## 12.6 Disruption

Some attackers seek to disrupt organizations, infrastructure, or services rather than obtain direct financial benefits.

---

# 13. Cybercriminal Profiles and Threat Actors

Cybercriminals are not a single homogeneous group. Different threat actors have different capabilities, resources, motivations, and objectives.

## 13.1 Script Kiddies

Individuals who use existing tools and publicly available scripts without necessarily understanding the underlying techniques.

Their activities can still cause significant damage.

## 13.2 Organized Cybercriminal Groups

Professionally organized groups may operate cybercrime as a business.

Activities can include:

* Ransomware.
* Credential theft.
* Financial fraud.
* Malware distribution.
* Data extortion.

## 13.3 Insider Threats

Insiders are individuals who already have legitimate access to an organization's systems.

They may intentionally or unintentionally:

* Steal information.
* Leak sensitive data.
* Introduce malware.
* Abuse privileges.
* Destroy information.

Insider investigations require careful analysis of user activity and access patterns.

## 13.4 Hacktivists

Hacktivists use cyber activities to promote political or social causes.

Common activities include:

* Website defacement.
* Data leaks.
* DDoS attacks.

## 13.5 Nation-State Actors

Nation-state-linked actors may conduct sophisticated cyber operations for:

* Espionage.
* Intelligence gathering.
* Strategic advantage.
* Military objectives.
* Disruption.

These operations may involve significant resources and advanced techniques.

## 13.6 Cyberterrorists

Cyberterrorism refers to the use of cyber capabilities in activities intended to cause serious disruption or fear in pursuit of ideological or political objectives.

---

# 14. Cybercrime Attack Lifecycle

A cybercrime incident can often be understood as a sequence of activities.

A simplified lifecycle is:

```text
Reconnaissance
      ↓
Initial Access
      ↓
Execution
      ↓
Persistence
      ↓
Privilege Escalation
      ↓
Defense Evasion
      ↓
Credential/Data Access
      ↓
Lateral Movement
      ↓
Data Collection
      ↓
Exfiltration / Impact
```

Not every cybercrime follows this exact sequence. However, the model helps investigators organize evidence and reconstruct attacker behaviour.

For example, investigators may ask:

* How did the attacker initially gain access?
* What account was compromised?
* What software was executed?
* How did the attacker maintain access?
* What privileges were obtained?
* Which systems were accessed?
* What information was collected?
* Was information transferred outside the organization?
* What impact did the attacker cause?

---

# 15. Cybercrime and Digital Forensics

Cybercrime and digital forensics are closely connected.

Cybercrime represents the **criminal activity**, while digital forensics provides methods for identifying, preserving, acquiring, examining, analyzing, and reporting digital evidence associated with that activity.

A simplified relationship is:

```text
Cybercrime
    ↓
Digital Trace / Evidence
    ↓
Identification
    ↓
Preservation
    ↓
Acquisition
    ↓
Examination
    ↓
Analysis
    ↓
Reporting
    ↓
Investigation Findings
```

Digital forensic investigators may examine:

* Computers.
* Smartphones.
* Servers.
* Network devices.
* Cloud environments.
* Databases.
* Email systems.
* Social media.
* Removable media.
* Application data.

Digital forensics therefore provides the technical foundation for reconstructing cybercrime incidents and presenting findings in a defensible manner.

---

# 16. Digital Evidence Generated by Cybercrime

Different cybercrimes generate different forms of digital evidence.

| Cybercrime       | Possible Evidence                                |
| ---------------- | ------------------------------------------------ |
| Phishing         | Emails, headers, URLs, browser history           |
| Malware          | Samples, hashes, logs, processes                 |
| Ransomware       | Encrypted files, ransom notes, logs              |
| Account takeover | Login records, IP addresses, authentication logs |
| Data breach      | Database records, access logs, network traffic   |
| Online fraud     | Emails, messages, transactions, device data      |
| Cyberstalking    | Messages, account records, timestamps            |
| DDoS             | Firewall logs, flow data, packet captures        |
| Insider threat   | User activity, access logs, file operations      |

The investigator must determine which evidence sources are relevant to the specific incident.

---

# 17. Cybercrime Investigation Challenges

## 17.1 Attribution

Determining who actually conducted an attack can be difficult.

Attackers may use:

* Compromised infrastructure.
* VPNs.
* Proxies.
* Tor.
* Stolen accounts.
* Botnets.

Therefore, an IP address alone should not automatically be treated as proof of the attacker's identity.

## 17.2 Encryption

Encryption can protect legitimate users but can also make forensic examination more difficult.

Investigators may encounter:

* Encrypted disks.
* Encrypted messaging applications.
* Encrypted archives.
* Encrypted network traffic.

## 17.3 Anti-Forensics

Attackers may deliberately attempt to hide or destroy evidence.

Examples include:

* Log deletion.
* File deletion.
* Timestamp manipulation.
* Secure wiping.
* Encryption.
* Obfuscation.
* Log manipulation.

## 17.4 Cloud Computing

Cloud environments introduce challenges because data may be distributed across different systems and geographical locations. Investigators may have limited access to the underlying physical infrastructure.

Cloud investigations may therefore depend heavily on:

* Provider logs.
* Audit trails.
* Snapshots.
* Virtual machine data.
* API records.
* Network telemetry.

## 17.5 Large Volumes of Data

Modern investigations can involve terabytes or even petabytes of information.

Investigators therefore need effective:

* Filtering.
* Searching.
* Indexing.
* Timeline analysis.
* Automated processing.

AI and machine learning can assist investigators in identifying patterns within large datasets, although forensic conclusions must remain explainable and subject to human review.

---

# 18. Best Practices for Cybercrime Investigations

Investigators should follow established forensic principles.

### Preserve Evidence

Evidence should be protected from unnecessary modification.

### Maintain Integrity

Hashing and other integrity mechanisms can help demonstrate that acquired evidence has not been altered.

### Maintain Chain of Custody

Every transfer or handling of evidence should be documented.

### Use Validated Tools

Forensic tools should be appropriately tested and their capabilities understood.

### Document Investigative Actions

Investigators should record:

* What was done.
* When it was done.
* Who performed it.
* Which tools were used.
* What results were obtained.

### Minimize Evidence Contamination

Investigators should avoid unnecessary interaction with original evidence.

### Follow Legal Authority

Investigations should comply with applicable laws, warrants, organizational policies, and evidence-handling requirements.

### Maintain Reproducibility

Another qualified investigator should be able to understand and, where possible, reproduce the investigative process.

---

# 20. Ethical and Legal Considerations

Cybercrime investigations must balance investigative objectives with legal and ethical responsibilities.

Investigators should consider:

* Privacy.
* Authorization.
* Data protection.
* Evidence admissibility.
* Search and seizure requirements.
* Jurisdiction.
* Confidentiality.
* Minimization of unnecessary data collection.

Investigators should only access systems and information when they have appropriate legal or organizational authority.

For educational and laboratory activities, techniques should be practiced only on systems that the investigator owns, has explicit permission to test, or that are intentionally provided for security training.


---

# 22. Key Terms

| Term                 | Meaning                                                                                                 |
| -------------------- | ------------------------------------------------------------------------------------------------------- |
| **Cybercrime**       | Criminal activity involving computers, networks, digital devices, or online services                    |
| **Threat Actor**     | Person or group responsible for or associated with malicious activity                                   |
| **Malware**          | Software designed to perform unauthorized or harmful actions                                            |
| **Phishing**         | Social engineering technique used to deceive victims into revealing information or performing an action |
| **Ransomware**       | Malware that restricts access to data or systems, commonly to demand payment                            |
| **Botnet**           | Network of compromised devices controlled by an attacker                                                |
| **Data Breach**      | Unauthorized access to protected or sensitive information                                               |
| **Identity Theft**   | Unauthorized acquisition or use of another person's identity information                                |
| **Cyber Extortion**  | Use of digital threats or attacks to obtain money or another benefit                                    |
| **Digital Evidence** | Information stored or transmitted in digital form that may be relevant to an investigation              |
| **Attribution**      | Process of determining who or what was responsible for an activity                                      |
| **Anti-Forensics**   | Techniques intended to hide, alter, destroy, or obstruct digital evidence                               |

---

# 23. Chapter Summary

Cybercrime is criminal activity involving digital technologies and can target individuals, organizations, governments, property, or society.

Common forms include hacking, phishing, malware, ransomware, identity theft, online fraud, cyberstalking, DDoS attacks, data breaches, and cyber extortion.

Cybercriminals have different motivations and capabilities. They may include individual attackers, organized criminal groups, insiders, hacktivists, nation-state actors, and other threat actors.

Cybercrime investigations rely heavily on digital evidence. Computers, smartphones, networks, cloud platforms, databases, applications, and online accounts can all contain evidence that helps investigators reconstruct events.

The major challenges include attribution, encryption, anti-forensics, cloud environments, large volumes of data, jurisdictional issues, and rapidly changing technologies.

Successful cybercrime investigations therefore require a combination of **technical knowledge, forensic methodology, critical thinking, documentation, legal awareness, and ethical conduct**.

---

# 25. References

1. Casey, E. (2011). *Digital Evidence and Computer Crime: Forensic Science, Computers, and the Internet* (3rd ed.). Academic Press.

2. Carrier, B. (2005). *File System Forensic Analysis*. Addison-Wesley.

3. National Institute of Standards and Technology (NIST). (2006). *Guide to Integrating Forensic Techniques into Incident Response*. Special Publication 800-86.

4. National Institute of Standards and Technology (NIST). (2014). *Guide to Integrating Forensic Techniques into Incident Response*. NIST Computer Security Resource Center.

5. National Institute of Standards and Technology (NIST). (2012). *Computer Security Incident Handling Guide*. Special Publication 800-61 Revision 2.

6. International Organization for Standardization (ISO). *ISO/IEC 27037 — Information technology — Security techniques — Guidelines for identification, collection, acquisition and preservation of digital evidence*.

7. International Organization for Standardization (ISO). *ISO/IEC 27041 — Information technology — Security techniques — Guidance on assuring suitability and adequacy of incident investigative method*.

8. International Organization for Standardization (ISO). *ISO/IEC 27042 — Information technology — Security techniques — Guidelines for the analysis and interpretation of digital evidence*.

9. MITRE. *MITRE ATT&CK®*. Knowledge base of adversary tactics, techniques, and procedures.

10. INTERPOL. *Digital Forensics*. Guidance and resources relating to digital evidence and forensic investigation.

11. Europol. *Internet Organised Crime Threat Assessment (IOCTA)*. Reports concerning emerging cybercrime threats and trends.

12. United Nations Office on Drugs and Crime (UNODC). *Comprehensive Study on Cybercrime*. United Nations resources on cybercrime and criminal justice.

---

 [Next Chapter ➡](../04-digital-evidence/README.md)

