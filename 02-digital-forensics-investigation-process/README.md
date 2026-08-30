# Chapter 2 — Digital Forensics Investigation Process

[⬅ Previous Chapter](../01-introduction-to-digital-forensics/README.md) | [Back to Course Home](../README.md) | [Next Chapter ➡](../03-cybercrime-fundamentals/README.md)


The digital forensics process is a structured sequence used to identify, preserve, acquire, examine, analyze, and report digital evidence.

**Key Stages:**

```
Identification → Preservation → Collection/Acquisition → Examination → Analysis → Reporting
```

---

## 1. Identification

Identification is the first stage of a digital forensic investigation. It involves determining what happened, what evidence may exist, and where that evidence can be found.

### Main Activities

The investigator:

- Identifies the suspected incident
- Determines the objectives and scope of the investigation
- Identifies potential sources of digital evidence
- Determines which devices, systems, accounts, or networks may be relevant
- Identifies the type of evidence that may be required

### Possible Sources of Evidence

Evidence may be found on:

- Computers and laptops
- Smartphones
- USB drives
- External hard drives
- Servers
- Network devices
- Cloud accounts
- Email systems
- Databases
- Application logs
- Browser data
- CCTV/DVR systems

### Example

If an organization suspects that an employee copied confidential files, the investigator might identify:

- The employee's computer
- USB devices
- File-server logs
- User login records
- Browser history
- Network logs

The investigator must determine which sources can help answer the investigative questions.

> **Key question:** What evidence exists, and where can it be found?

---

## 2. Preservation

After identifying potential evidence, the next step is preservation.

Preservation ensures that evidence is protected from alteration, destruction, contamination, or unauthorized access. Digital evidence is particularly fragile because simply opening a file, starting a computer, or connecting a device can potentially modify data.

### Main Activities

Investigators should:

- Secure the evidence
- Document the condition of devices
- Prevent unauthorized access
- Use appropriate evidence-handling procedures
- Establish a chain of custody
- Use write blockers where appropriate
- Record relevant information about the evidence

### Chain of Custody

The chain of custody documents the movement and handling of evidence throughout the investigation. It records information such as:

- Who collected the evidence
- When it was collected
- Where it was collected
- Who handled it
- Why it was transferred
- Where it was stored

### Example

An investigator collects a laptop and records:

| Field | Value |
|---|---|
| Evidence ID | LAP-001 |
| Description | Dell laptop |
| Collection time | 10:30 |
| Collector | Forensic Investigator |
| Location | Office 3 |
| Condition | Powered on |

This documentation helps establish confidence in the evidence.

> **Key question:** How can the evidence be protected from being changed or destroyed?

---

## 3. Collection / Acquisition

Collection or acquisition involves obtaining the digital evidence for forensic examination.

The investigator generally avoids performing extensive analysis directly on the original evidence. Instead, a forensic copy or image is created.

### Common Acquisition Methods

**Physical acquisition**

Attempts to obtain a complete representation of a storage device, including:

- Active files
- Deleted files
- Unallocated space
- File-system structures
- Slack space

**Logical acquisition**

Collects accessible files and directories, for example:

- Documents
- Photos
- Videos
- Contacts
- Application data

**Live acquisition**

Occurs while a computer or device is running. It may be necessary to capture volatile information such as:

- RAM
- Running processes
- Network connections
- Logged-in users
- Encryption keys

### Forensic Imaging

A forensic image is created from the original storage device so that investigators can examine the copy.

Common forensic image formats include:

- RAW/DD
- E01
- AFF/AFF4

### Hashing

Cryptographic hashes are used to verify evidence integrity.

```
Original Evidence
       ↓
   SHA-256
       ↓
Hash: ABC123...
```

After acquisition:

```
Forensic Image
       ↓
   SHA-256
       ↓
Hash: ABC123...
```

Matching hashes provide evidence that the acquired copy corresponds to the source at the time of verification.

Common hashing algorithms include:

- MD5
- SHA-1
- SHA-256
- SHA-512

For modern forensic integrity verification, SHA-256 or stronger algorithms are generally preferred.

> **Key question:** How can we obtain the evidence without compromising its integrity?

---

## 4. Examination

Examination involves processing the acquired evidence to locate and extract information that may be relevant to the investigation. This stage focuses primarily on **finding** evidence.

Investigators may examine:

- File systems
- Documents
- Images
- Videos
- Deleted files
- Browser history
- Emails
- System logs
- Application databases
- Registry artifacts
- Metadata
- Network information
- Memory dumps

### Example

An investigator examining a forensic image may discover:

- A deleted document
- USB connection records
- Browser history
- Recently accessed files
- User login records

At this stage, the investigator is primarily concerned with identifying and extracting these artifacts.

### Useful Tools

- Autopsy
- The Sleuth Kit
- FTK
- EnCase
- FTK Imager
- ExifTool
- Volatility
- Wireshark

> **Key question:** What relevant information can be extracted from the acquired evidence?

---

## 5. Analysis

Analysis is the stage where investigators interpret the evidence discovered during examination. This is where the investigator attempts to determine **what** happened, **how** it happened, **when** it happened, and potentially **who** was involved.

### Examination vs. Analysis

This distinction is important:

- **Examination:** "A USB device was found to have been connected to the computer."
- **Analysis:** "The USB connection occurred shortly before several confidential documents were accessed, suggesting a possible relationship between the device connection and the file activity."

Therefore: *examination finds evidence; analysis determines what the evidence means.*

### Timeline Analysis

One of the most important analytical techniques is creating a timeline.

```
09:00 ─ User logs into computer
   ↓
09:15 ─ USB device connected
   ↓
09:18 ─ Confidential file accessed
   ↓
09:20 ─ File copied
   ↓
09:25 ─ USB device removed
   ↓
09:30 ─ User logs out
```

The investigator can correlate these events with:

- File-system timestamps
- Windows Event Logs
- USB artifacts
- Network logs
- Authentication records
- Application logs

This helps reconstruct the sequence of events.

### Evidence Correlation

Investigators should avoid relying on a single artifact whenever possible. For example, combining:

- USB artifact
- File access record
- User login record
- File-server log
- Network evidence

...can provide a much stronger explanation of an incident than any single piece of evidence.

> **Key question:** What does the evidence tell us about what actually happened?

---

## 6. Reporting

Reporting is the final stage from identification through reporting. The investigator documents the methods, evidence, findings, analysis, conclusions, and limitations of the investigation.

A forensic report should be:

- Accurate
- Objective
- Clear
- Detailed enough to be understood
- Evidence-based
- Reproducible where practical
- Free from unsupported assumptions

---

## Chapter Summary

In short, the digital forensics process moves from **finding out what evidence might exist** (Identification), to **protecting it** (Preservation), **obtaining it safely** (Collection/Acquisition), **locating relevant artifacts within it** (Examination), **interpreting what those artifacts mean** (Analysis), and finally **communicating the findings** (Reporting). Each stage depends on the integrity of the one before it — for example, weak preservation undermines acquisition, and poor examination limits the depth of analysis possible.

---

## References

1. National Institute of Standards and Technology (NIST). *Guide to Integrating Forensic Techniques into Incident Response*, SP 800-86.
2. National Institute of Standards and Technology (NIST). *Computer Forensics Tool Testing (CFTT) Program*.
3. ISO/IEC 27037:2012 — *Guidelines for identification, collection, acquisition, and preservation of digital evidence*.
4. Scientific Working Group on Digital Evidence (SWGDE). *SWGDE Best Practices for Computer Forensic Acquisitions*.
5. Casey, E. *Digital Evidence and Computer Crime: Forensic Science, Computers, and the Internet*.
6. Carrier, B. *File System Forensic Analysis*.
7. Autopsy / The Sleuth Kit Documentation — https://www.sleuthkit.org/
8. Volatility Foundation Documentation — https://www.volatilityfoundation.org/
9. Wireshark Documentation — https://www.wireshark.org/docs/


 [Next Chapter ➡](../03-cybercrime-fundamentals/README.md)
