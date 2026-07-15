# Server Message Block (SMB)

> A practical guide to understanding SMB from a Security Operations Center (SOC) Analyst's perspective.

---

## Table of Contents

- [Overview](#overview)
- [Why SMB Matters](#why-smb-matters)
- [How SMB Works](#how-smb-works)
- [Ports and Protocols](#ports-and-protocols)
- [SMB in Enterprise Environments](#smb-in-enterprise-environments)
- [Why SMB Matters in SOC](#why-smb-matters-in-soc)
- [Common SMB-Based Attacks](#common-smb-based-attacks)
- [SOC Investigation Workflow](#soc-investigation-workflow)
- [Common SIEM Alerts](#common-siem-alerts)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Interview Questions](#interview-questions)
- [Key Takeaways](#key-takeaways)

---

# Overview

**Server Message Block (SMB)** is a Windows network protocol used to share files, folders, printers, and other resources between systems over a network.

Instead of storing duplicate files on every computer, organizations use SMB to provide centralized access to shared resources hosted on file servers.

SMB primarily communicates over **TCP Port 445**.

---

# Why SMB Matters

SMB enables users and systems to securely access shared resources across a network.

Organizations rely on SMB for:

- Centralized file sharing
- Shared printers
- Software deployment
- Backup operations
- Windows administration
- Active Directory environments

Without SMB, managing files and shared resources across hundreds of systems would be significantly more complex.

---

# How SMB Works

```text
User
   │
   ▼
Connects to File Server
   │
   ▼
Authentication
   │
   ▼
Permission Check
   │
   ▼
Access Shared Resource
   │
   ▼
Read / Write File
   │
   ▼
Changes Saved on Server
```

Typical workflow:

1. User connects to a shared resource.
2. Windows authenticates the user.
3. Server verifies permissions.
4. Access is granted or denied.
5. Files are read or modified directly on the server.

---

# Ports and Protocols

| Port | Protocol | Purpose |
|------|----------|---------|
| 445 | TCP | SMB file and resource sharing |

### Why TCP?

SMB uses TCP because file transfers require:

- Reliable communication
- Ordered packet delivery
- Error detection
- Packet retransmission

These features ensure files are transferred without corruption or loss.

---

# SMB in Enterprise Environments

SMB is widely used across Windows enterprise networks.

Common use cases include:

- File servers
- Departmental shared folders
- Network printers
- Software distribution
- Backup servers
- Administrative shares
- Windows server management

SMB is a core component of most Active Directory environments.

---

# Why SMB Matters in SOC

SMB is one of the most abused protocols during post-compromise activity.

After gaining initial access, attackers frequently use SMB for:

- Lateral movement
- Remote administration
- Credential abuse
- File theft
- Malware propagation
- Ransomware deployment

Monitoring SMB activity helps analysts identify suspicious movement inside enterprise networks.

---

# Common SMB-Based Attacks

## EternalBlue

Exploits a vulnerability in SMB to achieve remote code execution on vulnerable Windows systems.

---

## WannaCry

A ransomware campaign that leveraged the EternalBlue vulnerability to spread automatically across Windows networks.

---

## Pass-the-Hash (PtH)

Uses stolen NTLM password hashes to authenticate over SMB without knowing the user's actual password.

### Indicators

- NTLM authentication anomalies
- Administrative share access
- Lateral movement between hosts

---

## SMB Enumeration

Attackers discover available systems and shared resources before launching further attacks.

Common objectives include:

- Identifying shared folders
- Enumerating accessible hosts
- Discovering sensitive resources

---

## PsExec Abuse

Attackers misuse the legitimate **PsExec** utility to execute commands remotely through SMB after obtaining administrative credentials.

---

## SMB Brute Force

Repeated authentication attempts against SMB services to discover valid credentials.

### Indicators

- Multiple failed logins
- Repeated authentication attempts
- Account lockouts
- Excessive SMB connections

---

# SOC Investigation Workflow

When investigating suspicious SMB activity, verify:

- Source endpoint
- Destination endpoint
- Logged-in user
- Authentication success or failure
- User privileges
- Number of systems contacted
- Administrative share access (C$, ADMIN$)
- PsExec or remote execution activity
- File access or modification
- EDR detections
- Signs of ransomware behavior

---

# Common SIEM Alerts

Examples include:

- Suspicious SMB Connection
- SMB Scan Detected
- Lateral Movement Detected
- Pass-the-Hash Activity
- PsExec Execution
- Excessive SMB Connections
- SMB Brute Force
- Access to Sensitive File Share
- Possible EternalBlue Exploitation
- Administrative Share Access

---

# MITRE ATT&CK Mapping

| Tactic | SMB Usage |
|---------|-----------|
| Discovery | Enumerating hosts and shared resources |
| Credential Access | Pass-the-Hash and credential abuse |
| Lateral Movement | Moving between systems using SMB |
| Execution | Remote command execution with PsExec |
| Collection | Accessing and copying shared files |

---

# Interview Questions

### What is SMB?

SMB (Server Message Block) is a Windows file-sharing protocol used to share files, folders, printers, and other resources across a network. It primarily uses **TCP Port 445**.

---

### Which port does SMB use?

SMB primarily communicates over **TCP Port 445**.

---

### Why does SMB use TCP?

TCP provides reliable and ordered communication, ensuring file transfers occur without corruption or packet loss.

---

### Why is SMB important for SOC Analysts?

SMB is heavily used in Windows environments and is commonly abused for lateral movement, ransomware propagation, credential abuse, and unauthorized access to shared resources.

---

### Name common SMB attacks.

- EternalBlue
- WannaCry
- Pass-the-Hash
- SMB Enumeration
- PsExec Abuse
- SMB Brute Force

---

### How would you investigate a suspicious SMB alert?

A typical investigation includes:

- Identifying the source and destination systems
- Reviewing the user account involved
- Verifying authentication activity
- Checking the number of hosts contacted
- Looking for PsExec or remote execution
- Correlating with EDR alerts
- Determining whether the activity indicates lateral movement or ransomware

---

# Key Takeaways

| Topic | Summary |
|--------|---------|
| Purpose | Shares files, folders, printers, and network resources |
| Default Port | 445 |
| Protocol | TCP |
| Enterprise Usage | File sharing, printers, backups, software deployment |
| SOC Relevance | Critical protocol for detecting lateral movement and ransomware |
| Common Attacks | EternalBlue, WannaCry, Pass-the-Hash, SMB Enumeration, PsExec Abuse, Brute Force |

---

## Skills Demonstrated

- SMB Fundamentals
- Windows Networking
- Enterprise File Sharing
- Lateral Movement Detection
- Threat Detection
- Incident Investigation
- SIEM Analysis
- MITRE ATT&CK
- Blue Team Operations

---
