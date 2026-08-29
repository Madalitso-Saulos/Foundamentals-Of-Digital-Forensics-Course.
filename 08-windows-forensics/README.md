# Chapter 8 — Windows Forensics

[⬅ Previous](../07-network-forensics/README.md) | [Back to Course Home](../README.md) | [Next Chapter ➡](../09-malware-forensics/README.md)

---


## 1. Introduction to Windows Forensics

Windows forensics is the process of identifying, acquiring, preserving, examining, analyzing, and reporting digital evidence from computers running Microsoft Windows operating systems.

Windows systems generate large amounts of forensic evidence during normal operation. This evidence can reveal:

* Which users accessed a computer.
* When users logged in or logged out.
* Which applications were executed.
* Which files were opened or modified.
* Which USB devices were connected.
* Which websites and resources were accessed.
* What programs started automatically.
* How the system was configured.
* Evidence of persistence or malicious activity.
* Changes made to the Windows operating system.

Windows forensics is therefore an important part of investigations involving unauthorized access, insider threats, malware infections, data theft, intellectual property theft, and other cybercrime investigations.

A Windows forensic investigation should maintain the same fundamental principles used throughout digital forensics: **preservation, integrity, repeatability, documentation, and proper chain of custody**.

---

## 2. Windows Forensic Investigation Process

A typical Windows forensic investigation can be divided into several stages.

### 2.1 Identification

The investigator determines:

* Which computers are relevant.
* Which users are associated with the systems.
* What operating system and version are involved.
* Whether volatile evidence such as RAM is available.
* What potential evidence sources should be collected.

### 2.2 Preservation

Evidence must be protected from modification or destruction.

Investigators should:

* Document the condition of the system.
* Avoid unnecessary interaction with the original evidence.
* Use write-blocking technologies where appropriate.
* Work from forensic copies rather than the original media.
* Calculate cryptographic hashes of acquired evidence.

Common hashing algorithms include SHA-256 and SHA-512.

### 2.3 Acquisition

Relevant evidence may include:

* Hard drives and SSDs.
* External storage devices.
* RAM.
* Windows Registry hives.
* Event Logs.
* Browser data.
* Application artifacts.
* User profiles.
* Network configuration data.

A forensic disk image should be created whenever possible rather than conducting analysis directly on the original storage device.

### 2.4 Examination

The investigator extracts relevant artifacts from the acquired evidence.

Examples include:

* Registry keys.
* Event Log records.
* File system metadata.
* Browser history.
* Prefetch files.
* LNK files.
* Jump Lists.
* Windows Search artifacts.
* USB device information.

### 2.5 Analysis

The extracted artifacts are correlated to reconstruct events.

For example:

> A Windows Event Log may show that a user logged in at 08:30, while Prefetch and LNK artifacts can provide additional evidence that a particular application or file was accessed shortly afterward.

No single artifact should normally be treated as definitive evidence without considering its context and corroborating evidence.

### 2.6 Reporting

The investigator documents:

* Evidence collected.
* Acquisition methods.
* Tools used.
* Hash values.
* Findings.
* Timeline events.
* Limitations.
* Conclusions.

Reports should clearly distinguish between **observed evidence** and **investigator interpretation**.

---

# 3. Windows File Systems

Windows systems commonly use **NTFS** and, in some environments, **FAT-based file systems**.

## 3.1 NTFS

NTFS (New Technology File System) is the primary file system used by modern Windows systems.

Important NTFS structures include:

* Master File Table (MFT)
* File records
* File attributes
* `$STANDARD_INFORMATION`
* `$FILE_NAME`
* `$DATA`
* `$UsnJrnl`
* `$LogFile`
* `$Bitmap`
* `$Secure`

### Master File Table (MFT)

The MFT contains records describing files and directories on an NTFS volume.

Forensic examination of the MFT can provide information such as:

* File names.
* File paths.
* File sizes.
* File timestamps.
* File allocation information.
* Deleted file records.

The MFT is particularly useful for reconstructing file system activity.

### NTFS Timestamps

Windows file systems contain several timestamps that can assist investigators.

Common timestamps include:

* Created
* Modified
* Accessed
* Metadata changed

Investigators should be careful when interpreting timestamps because they can be affected by system configuration, copying operations, application behavior, time zones, and anti-forensic techniques.

### USN Journal

The NTFS Update Sequence Number Journal (`$UsnJrnl`) records changes made to files and directories.

It may provide evidence of events such as:

* File creation.
* File deletion.
* File renaming.
* File modification.
* Directory changes.

The USN Journal can therefore help establish a sequence of file system activity.

---

# 4. Windows Registry Forensics

The Windows Registry is a hierarchical database used by Windows to store configuration information.

From a forensic perspective, the Registry is extremely valuable because it contains information about:

* Users.
* Installed software.
* Operating system configuration.
* Recently accessed files.
* Network configuration.
* Connected devices.
* Application execution.
* Startup programs.

## 4.1 Major Registry Hives

Important Registry hives include:

| Hive           | Description                                    |
| -------------- | ---------------------------------------------- |
| `SYSTEM`       | System configuration and hardware information  |
| `SOFTWARE`     | Installed software and Windows configuration   |
| `SAM`          | Local user account information                 |
| `SECURITY`     | Security policy and configuration              |
| `NTUSER.DAT`   | User-specific settings and activity            |
| `USRCLASS.DAT` | User shell and application-related information |

### NTUSER.DAT

`NTUSER.DAT` is associated with an individual Windows user profile.

It can contain evidence relating to:

* User preferences.
* Recently accessed resources.
* Application activity.
* Shell activity.
* User-specific configuration.

### SYSTEM Hive

The SYSTEM hive contains information relating to:

* Hardware.
* Services.
* Drivers.
* System configuration.
* Control sets.
* Time zone configuration.

It can also assist in identifying connected devices and system startup configuration.

### Registry Examination

Investigators should preserve Registry hives before analysis and use forensic tools capable of parsing Registry structures.

Useful tools include:

* Registry Explorer
* RegRipper
* Autopsy
* FTK
* Magnet AXIOM

---

# 5. Windows Event Logs

Windows Event Logs provide valuable information about operating system and application activity.

Modern Windows systems commonly store logs in:

`C:\Windows\System32\winevt\Logs\`

Important logs include:

* `Security.evtx`
* `System.evtx`
* `Application.evtx`

Additional logs may be available under:

`Applications and Services Logs`

## 5.1 Security Logs

Security logs can provide evidence of:

* Successful logons.
* Failed logons.
* Account creation.
* Account changes.
* Privilege-related activity.
* Security policy changes.

Examples of useful Windows Event IDs include:

| Event ID | Typical Significance                       |
| -------- | ------------------------------------------ |
| 4624     | Successful account logon                   |
| 4625     | Failed account logon                       |
| 4634     | Account logoff                             |
| 4648     | Logon using explicit credentials           |
| 4672     | Special privileges assigned to a new logon |
| 4688     | Process creation                           |
| 4720     | User account created                       |
| 4722     | User account enabled                       |
| 4725     | User account disabled                      |
| 4726     | User account deleted                       |

Event IDs should always be interpreted in context because their meaning and availability depend on Windows version and audit configuration.

## 5.2 System Logs

System logs may reveal:

* Service activity.
* Driver events.
* Startup and shutdown.
* Hardware problems.
* System errors.
* Configuration changes.

## 5.3 Application Logs

Application logs can provide evidence about:

* Application failures.
* Application activity.
* Software installations.
* Application-specific events.

---

# 6. Windows User Activity Artifacts

Windows creates several artifacts that can help reconstruct user behavior.

## 6.1 Prefetch

Windows Prefetch files are designed to improve application startup performance.

They can also provide forensic evidence that an executable was executed.

Prefetch files are commonly located at:

`C:\Windows\Prefetch\`

Investigators may examine:

* Executable name.
* Execution-related information.
* Referenced files.
* Referenced directories.
* Execution timestamps, subject to Windows version and artifact limitations.

Prefetch should not be interpreted as absolute proof of a user's intentional execution of a program.

---

## 6.2 LNK Files

Windows shortcut files, commonly known as LNK files, can contain information about referenced files.

They may provide:

* File name.
* Original path.
* Volume information.
* Timestamps.
* File size.
* Machine-related information.

LNK artifacts can therefore help determine whether a file or location was accessed through Windows shell activity.

---

## 6.3 Jump Lists

Jump Lists provide shortcuts to recently or frequently accessed files and application resources.

They can contain evidence relating to:

* Recently accessed documents.
* Application activity.
* File paths.
* User interaction with applications.

Jump Lists are particularly useful when combined with other artifacts such as LNK files and ShellBags.

---

## 6.4 ShellBags

ShellBags record information about folders viewed through the Windows graphical shell.

They can provide evidence of:

* Folder access.
* Folder structure.
* External storage locations.
* Previously accessed directories.

ShellBags can remain available even after a folder is no longer accessible, making them potentially valuable during forensic investigations.

---

## 6.5 Recent Files

Windows maintains several artifacts relating to recently accessed files and locations.

These artifacts can help investigators determine:

* Which documents were recently accessed.
* Which directories were viewed.
* Which applications were involved.

---

# 7. USB and External Device Forensics

USB devices can provide important evidence in investigations involving data theft or unauthorized transfer.

Windows may retain information about previously connected devices.

Potential artifacts can reveal:

* Device manufacturer.
* Device model.
* Serial number.
* Device identifiers.
* Connection information.
* Associated drive letters.

Relevant Registry locations include areas under the SYSTEM hive associated with USB and storage device configuration.

USB evidence can be correlated with:

* File system timestamps.
* LNK files.
* Event Logs.
* User activity.
* File access records.

For example, an investigation might correlate a USB device's presence with the creation or copying of sensitive documents.

---

# 8. Windows User Account Forensics

User account analysis can help determine who had access to a Windows system.

Investigators may examine:

* Local accounts.
* Account creation.
* Account modification.
* Account deletion.
* Group membership.
* Administrative privileges.
* Login activity.
* Failed authentication attempts.

Important evidence may be found in:

* SAM Registry hive.
* Security Event Logs.
* SYSTEM hive.
* User profile directories.

Investigators should distinguish between **account ownership** and **actual user activity**. The existence of an account does not by itself prove that the account holder performed a specific action.

---

# 9. Browser and Internet Artifacts

Web browsers can contain valuable evidence.

Depending on the browser, investigators may examine:

* Browsing history.
* Downloads.
* Cookies.
* Cached resources.
* Bookmarks.
* Saved sessions.
* Form information.
* Browser extensions.

Browser artifacts can help establish:

* Websites visited.
* Approximate access times.
* Files downloaded.
* Online services accessed.

Browser evidence should be correlated with network, file system, and application artifacts whenever possible.

---

# 10. Windows Memory Forensics

Memory forensics involves analyzing the contents of RAM.

Unlike disk evidence, RAM is volatile and can disappear when a computer is powered off.

Memory may contain:

* Running processes.
* Network connections.
* Loaded modules.
* Command history.
* Credentials or authentication material.
* Malware.
* Encryption keys.
* Open files.
* Injected code.

## 10.1 Memory Acquisition

A memory image should ideally be acquired while the system is running when volatile evidence is important.

Common tools include:

* WinPmem
* Magnet RAM Capture
* DumpIt
* Belkasoft RAM Capturer

The acquisition process should be carefully documented because interacting with a live system can alter its state.

## 10.2 Memory Analysis

Tools such as **Volatility** can be used to analyze memory images.

Common analysis tasks include:

* Process enumeration.
* Network connection analysis.
* DLL/module analysis.
* Command-line investigation.
* Process tree reconstruction.
* Detection of suspicious processes.

Memory analysis can be especially useful during malware investigations because malicious code may exist only in memory.

---

# 11. Windows Timeline Analysis

Timeline analysis combines evidence from multiple artifacts to reconstruct events in chronological order.

A timeline may include:

```text
08:30:12  User successfully logs in
08:34:05  USB storage device connected
08:35:17  Document accessed
08:36:42  File copied to external media
08:40:10  USB device disconnected
08:45:33  User logs out
```

The exact events and timestamps must be derived from available evidence rather than assumed.

Useful timeline sources include:

* Event Logs.
* MFT records.
* USN Journal.
* Prefetch.
* LNK files.
* Browser history.
* Registry artifacts.
* USB artifacts.
* Application logs.

Timeline analysis is most effective when multiple independent artifacts support the same sequence of events.

---

# 12. Windows Persistence Artifacts

Attackers frequently attempt to maintain access to compromised Windows systems.

Investigators should examine common persistence mechanisms such as:

* Registry Run keys.
* Startup folders.
* Scheduled Tasks.
* Windows Services.
* WMI persistence.
* Browser extensions.
* DLL search-order abuse.
* Startup scripts.

Evidence of persistence can help explain how malware or unauthorized software survived system restarts.

Investigators should document:

1. The persistence mechanism.
2. The executable or script involved.
3. The associated user or account.
4. Relevant timestamps.
5. The relationship to other evidence.

---

# 13. Common Windows Forensic Tools

| Tool                   | Primary Use                                          |
| ---------------------- | ---------------------------------------------------- |
| Autopsy                | Disk and forensic artifact analysis                  |
| FTK                    | Forensic examination and analysis                    |
| Magnet AXIOM           | Digital evidence analysis                            |
| Volatility             | Memory forensics                                     |
| RegRipper              | Windows Registry analysis                            |
| Registry Explorer      | Registry examination                                 |
| KAPE                   | Rapid collection and processing of Windows artifacts |
| Eric Zimmerman's tools | Windows artifact analysis                            |
| Plaso/log2timeline     | Timeline generation                                  |
| EvtxECmd               | Windows Event Log analysis                           |
| PECmd                  | Prefetch analysis                                    |
| MFTECmd                | MFT analysis                                         |

Tool output should always be validated and interpreted within the context of the investigation.

---

# 14. Practical Investigation Workflow

A simplified Windows forensic workflow can be represented as:

```text
Identify System
      ↓
Document Evidence
      ↓
Acquire Disk / RAM
      ↓
Calculate Hashes
      ↓
Extract Windows Artifacts
      ↓
Analyze Registry
      ↓
Analyze Event Logs
      ↓
Analyze File System
      ↓
Analyze User Activity
      ↓
Build Timeline
      ↓
Correlate Evidence
      ↓
Document Findings
      ↓
Produce Forensic Report
```

The workflow may vary depending on the incident and available evidence.

---

# 15. Evidence Integrity and Chain of Custody

Windows forensic evidence must be handled in a manner that preserves its integrity.

Important practices include:

* Use forensic images rather than originals whenever possible.
* Calculate cryptographic hashes.
* Record evidence identifiers.
* Document acquisition dates and times.
* Record investigator actions.
* Maintain chain-of-custody documentation.
* Store evidence securely.
* Use validated forensic tools.
* Preserve original evidence as read-only.

A forensic conclusion is significantly stronger when another investigator can independently reproduce the analysis.

---

# 16. Challenges in Windows Forensics

Windows investigations present several challenges.

### Large Amounts of Evidence

Modern Windows systems generate enormous quantities of logs and artifacts. Investigators must identify the most relevant evidence.

### Encryption

Full-disk encryption and encrypted application data may limit access to evidence.

### Anti-Forensics

Attackers may attempt to:

* Delete logs.
* Modify timestamps.
* Clear Registry artifacts.
* Delete files.
* Disable logging.
* Use secure deletion tools.

### Different Windows Versions

Artifact locations, structures, and behavior can differ between Windows versions.

### Cloud and Remote Services

Modern Windows systems increasingly interact with:

* Cloud storage.
* Microsoft accounts.
* Remote management systems.
* SaaS applications.

Important evidence may therefore exist outside the local computer.

### SSD and TRIM

Solid-state drives can complicate deleted-file recovery because of technologies such as TRIM and garbage collection.


---

# 19. Chapter Summary

Windows forensics focuses on collecting and analyzing evidence generated by Windows operating systems. Important evidence sources include the NTFS file system, Windows Registry, Event Logs, Prefetch, LNK files, Jump Lists, USB artifacts, browser data, user accounts, and RAM.

The strongest investigations correlate multiple independent artifacts rather than relying on a single source. For example, a Registry artifact may identify a USB device, while Event Logs, file system metadata, and user activity artifacts can provide additional context about when and how the system was used.

Windows forensic investigations must also prioritize evidence integrity, proper documentation, chain of custody, and repeatable analysis. As Windows systems continue to incorporate cloud services, encryption, virtualization, and advanced security features, investigators must continually adapt their techniques and tools.

---

## References

1. **Microsoft.** *Windows Security Auditing Documentation.* Microsoft Learn.
   https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/

2. **Microsoft.** *Windows Event Log.* Microsoft Learn.
   https://learn.microsoft.com/en-us/windows/win32/wes/windows-event-log

3. **Microsoft.** *NTFS Technical Reference.* Microsoft Learn.
   https://learn.microsoft.com/en-us/windows-server/storage/file-server/ntfs-overview

4. **Casey, E.** *Digital Evidence and Computer Crime: Forensic Science, Computers and the Internet.* Academic Press.

5. **Nelson, B., Phillips, A., & Steuart, C.** *Guide to Computer Forensics and Investigations.* Cengage Learning.


---

[Next Chapter ➡](../09-malware-forensics/README.md)
