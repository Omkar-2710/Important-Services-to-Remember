# File Transfer Protocols (FTP, FTPS & SFTP)

> A practical guide to understanding FTP, FTPS, and SFTP from a Security Operations Center (SOC) Analyst's perspective.

---

## Table of Contents

- [Overview](#overview)
- [Why File Transfer Protocols Matter](#why-file-transfer-protocols-matter)
- [How File Transfers Work](#how-file-transfers-work)
- [FTP (File Transfer Protocol)](#ftp-file-transfer-protocol)
- [FTPS (FTP Secure)](#ftps-ftp-secure)
- [SFTP (SSH File Transfer Protocol)](#sftp-ssh-file-transfer-protocol)
- [Ports and Protocols](#ports-and-protocols)
- [FTP vs FTPS vs SFTP](#ftp-vs-ftps-vs-sftp)
- [File Transfer Protocols in Enterprise Environments](#file-transfer-protocols-in-enterprise-environments)
- [Why File Transfer Protocols Matter in SOC](#why-file-transfer-protocols-matter-in-soc)
- [Common File Transfer Attacks](#common-file-transfer-attacks)
- [SOC Investigation Workflow](#soc-investigation-workflow)
- [Common SIEM Alerts](#common-siem-alerts)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Interview Questions](#interview-questions)
- [Key Takeaways](#key-takeaways)

---

# Overview

**FTP (File Transfer Protocol)**, **FTPS (File Transfer Protocol Secure)**, and **SFTP (SSH File Transfer Protocol)** are protocols used to transfer files between computers over a network.

Although they serve a similar purpose, they differ significantly in terms of security and implementation:

- **FTP** transfers files without encryption.
- **FTPS** secures FTP using SSL/TLS encryption.
- **SFTP** is a completely different protocol that operates over SSH.

Understanding these differences is essential for both SOC investigations and cybersecurity interviews.

---

# Why File Transfer Protocols Matter

Organizations regularly transfer files between users, servers, cloud platforms, and business applications.

Common use cases include:

- Website deployment
- Server-to-server file transfers
- Software deployment
- Backup operations
- Business file exchange
- Data migration

Because file transfer protocols often handle sensitive information, they are frequently targeted by attackers.

---

# How File Transfers Work

```text
User / Application
        │
        ▼
FTP / FTPS / SFTP Client
        │
        ▼
Network Connection
        │
        ▼
File Transfer Server
        │
        ▼
Upload / Download Files
```

Typical workflow:

1. User initiates a connection to the file server.
2. Authentication is performed.
3. A file upload or download request is sent.
4. The server processes the request.
5. Files are transferred successfully.

---

# FTP (File Transfer Protocol)

**FTP** is a standard protocol used to transfer files between computers over a network.

It uses:

- **TCP Port 21** for control commands.
- **TCP Port 20** for data transfer in Active Mode.

FTP **does not encrypt** authentication or file transfers.

Common use cases include:

- Legacy applications
- Website uploads
- File servers
- Backup systems
- Industrial environments

Because FTP transmits usernames, passwords, and files in plaintext, it is no longer recommended for sensitive data. :contentReference[oaicite:1]{index=1}

---

# FTPS (FTP Secure)

**FTPS** is FTP enhanced with **SSL/TLS encryption**.

Unlike standard FTP, FTPS encrypts:

- Usernames
- Passwords
- File transfers

FTPS commonly uses:

- **TCP Port 990** (Implicit FTPS)
- **TCP Port 21** (Explicit FTPS using TLS)

FTPS maintains compatibility with traditional FTP while adding encryption for secure communication. :contentReference[oaicite:2]{index=2}

---

# SFTP (SSH File Transfer Protocol)

**SFTP** is a secure file transfer protocol that operates over **SSH (Secure Shell)**.

Unlike FTPS, SFTP is **not** an extension of FTP.

It:

- Uses the SSH protocol
- Provides encrypted authentication
- Encrypts all file transfers
- Uses **TCP Port 22**

Because many organizations already use SSH for remote administration, SFTP has become one of the most widely used secure file transfer protocols. :contentReference[oaicite:3]{index=3}

---

# Ports and Protocols

| Protocol | Port | Purpose |
|----------|------|---------|
| FTP | 21/TCP | Control connection |
| FTP (Active Mode) | 20/TCP | Data transfer |
| FTPS | 990/TCP (or 21 with TLS) | Encrypted FTP communication |
| SFTP | 22/TCP | Secure file transfer over SSH |

---

# FTP vs FTPS vs SFTP

| Feature | FTP | FTPS | SFTP |
|---------|-----|-------|-------|
| Encryption | ❌ No | ✅ SSL/TLS | ✅ SSH |
| Uses FTP Protocol | ✅ Yes | ✅ Yes | ❌ No |
| Uses SSH | ❌ No | ❌ No | ✅ Yes |
| Primary Port | 21 | 990 / 21 (TLS) | 22 |
| Secure | ❌ No | ✅ Yes | ✅ Yes |

A simple way to remember:

- **FTP → No encryption**
- **FTPS → FTP + SSL/TLS**
- **SFTP → SSH-based secure file transfer**

---

# File Transfer Protocols in Enterprise Environments

File transfer protocols are widely used across enterprise environments.

Common use cases include:

- Website deployment
- Backup transfers
- Software updates
- Business-to-business file exchange
- Cloud file transfers
- Linux server administration
- Automated data synchronization

Modern organizations typically prefer **SFTP** or **FTPS** because they provide encrypted communication.

---

# Why File Transfer Protocols Matter in SOC

File transfer activity can provide valuable evidence during a security investigation.

Attackers commonly abuse file transfer protocols to:

- Upload malware
- Deploy web shells
- Steal sensitive documents
- Exfiltrate customer data
- Transfer ransomware
- Move stolen files outside the organization

Monitoring file transfer activity helps SOC analysts identify unauthorized access, malware delivery, and data exfiltration. :contentReference[oaicite:4]{index=4}

---

# Common File Transfer Attacks

## FTP Brute Force

Attackers repeatedly attempt different username and password combinations until valid credentials are discovered.

### Indicators

- Multiple failed login attempts
- Successful login after repeated failures
- Authentication attempts from unfamiliar IP addresses

---

## Anonymous FTP Abuse

Some FTP servers allow anonymous access.

If improperly configured, attackers may access or download files without authentication.

---

## Credential Sniffing

FTP transmits credentials in plaintext.

Attackers monitoring network traffic may capture:

- Usernames
- Passwords
- File contents

---

## Unauthorized File Upload

Attackers upload malicious files such as:

- Malware
- Web shells
- Backdoors
- Ransomware

---

## Data Exfiltration

Attackers use FTP, FTPS, or SFTP to transfer stolen data outside the organization.

Common targets include:

- Customer databases
- Backups
- Financial records
- Source code
- Internal documents

---

## SSH Key Abuse (SFTP)

Although SFTP encrypts communication, attackers may still gain access by stealing SSH keys or valid credentials.

Encryption protects the connection—not compromised accounts.

---

# SOC Investigation Workflow

When investigating a suspicious file transfer alert, verify:

- Source IP address
- Destination server
- User account
- Authentication success or failure
- Files uploaded or downloaded
- File size
- Large outbound transfers
- Executable file uploads (.exe, .dll, .php, .jsp)
- Anonymous FTP usage
- Related authentication logs
- EDR telemetry
- Signs of malware or data exfiltration

---

# Common SIEM Alerts

Examples include:

- FTP Brute Force
- Anonymous FTP Login
- Suspicious FTP Login
- Unauthorized FTP Access
- Excessive File Download
- Large File Upload
- FTP Data Exfiltration
- Suspicious SFTP Login
- Unauthorized SSH Key Usage
- Excessive File Transfer Activity

---

# MITRE ATT&CK Mapping

| Tactic | File Transfer Usage |
|---------|---------------------|
| Initial Access | Brute-force FTP/SFTP credentials |
| Persistence | Uploading web shells or backdoors |
| Collection | Gathering sensitive files |
| Exfiltration | Transferring stolen data to external servers |

---

# Interview Questions

### What is FTP?

FTP (File Transfer Protocol) is used to transfer files between computers over a network. It primarily uses **TCP Port 21** for control commands and does not encrypt authentication or file transfers. :contentReference[oaicite:5]{index=5}

---

### Why is FTP considered insecure?

FTP transmits usernames, passwords, and file data in plaintext. Anyone who intercepts the network traffic may be able to read the credentials and transferred files. :contentReference[oaicite:6]{index=6}

---

### What is FTPS?

FTPS is FTP secured with SSL/TLS encryption. It encrypts authentication and file transfers while maintaining compatibility with the traditional FTP protocol. :contentReference[oaicite:7]{index=7}

---

### What is SFTP?

SFTP (SSH File Transfer Protocol) is a secure file transfer protocol that operates over SSH. It uses **TCP Port 22** and encrypts both authentication and file transfers. :contentReference[oaicite:8]{index=8}

---

### What is the difference between FTP, FTPS, and SFTP?

- FTP transfers files without encryption.
- FTPS adds SSL/TLS encryption to FTP.
- SFTP is a different protocol that operates over SSH and provides encrypted file transfers.

---

### How would you investigate a suspicious FTP or SFTP alert?

A typical investigation includes:

- Identifying the user account
- Reviewing the source and destination systems
- Checking authentication activity
- Reviewing uploaded or downloaded files
- Looking for unusually large transfers
- Determining whether executable files were transferred
- Correlating authentication, endpoint, and network logs
- Determining whether data exfiltration or malware delivery occurred

---

# Key Takeaways

| Topic | Summary |
|--------|---------|
| FTP | Basic file transfer protocol without encryption |
| FTPS | FTP secured using SSL/TLS |
| SFTP | Secure file transfer over SSH |
| FTP Ports | 20, 21 |
| FTPS Port | 990 (or 21 with TLS) |
| SFTP Port | 22 |
| SOC Relevance | Critical for detecting unauthorized file transfers and data exfiltration |
| Common Threats | Brute Force, Anonymous Access, Credential Sniffing, Malware Uploads, Data Exfiltration |

---
