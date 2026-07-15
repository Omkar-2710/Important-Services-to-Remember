# Lightweight Directory Access Protocol (LDAP)

> A practical guide to understanding LDAP from a Security Operations Center (SOC) Analyst's perspective.

---

## Table of Contents

- [Overview](#overview)
- [Why LDAP Matters](#why-ldap-matters)
- [How LDAP Works](#how-ldap-works)
- [Ports and Protocols](#ports-and-protocols)
- [LDAP vs LDAPS](#ldap-vs-ldaps)
- [LDAP in Enterprise Environments](#ldap-in-enterprise-environments)
- [Why LDAP Matters in SOC](#why-ldap-matters-in-soc)
- [Common LDAP-Based Attacks](#common-ldap-based-attacks)
- [SOC Investigation Workflow](#soc-investigation-workflow)
- [Common SIEM Alerts](#common-siem-alerts)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [LDAP vs Kerberos](#ldap-vs-kerberos)
- [Interview Questions](#interview-questions)
- [Key Takeaways](#key-takeaways)

---

# Overview

**Lightweight Directory Access Protocol (LDAP)** is a protocol used to access and manage directory services such as **Microsoft Active Directory (AD)**.

LDAP allows users, applications, and administrators to search for directory objects including:

- Users
- Groups
- Computers
- Printers
- Organizational Units (OUs)
- Security Groups

LDAP primarily uses **TCP Port 389**, while **LDAPS** (LDAP over TLS) uses **TCP Port 636**.

---

# Why LDAP Matters

Large organizations manage thousands of users, devices, and resources.

Instead of maintaining separate user databases for every application, LDAP provides a **centralized directory** that stores and organizes directory information.

Organizations rely on LDAP for:

- User lookups
- Group membership
- Email address searches
- Device information
- Active Directory management
- Enterprise application integration

LDAP simplifies identity and resource management across the enterprise.

---

# How LDAP Works

```text
User/Application
        │
        ▼
 Sends LDAP Query
        │
        ▼
Active Directory
        │
        ▼
Searches Directory
        │
        ▼
Returns Requested Information
```

Typical workflow:

1. A user or application sends an LDAP query.
2. Active Directory searches its directory database.
3. Matching objects are located.
4. Requested information is returned.

**Important:** LDAP performs **directory lookups**, not user authentication.

Authentication is typically handled by **Kerberos**.

---

# Ports and Protocols

| Port | Protocol | Purpose |
|------|----------|---------|
| 389 | TCP | Standard LDAP communication |
| 636 | TCP | Secure LDAP (LDAPS) using TLS |

---

# LDAP vs LDAPS

| LDAP | LDAPS |
|------|--------|
| Port 389 | Port 636 |
| Unencrypted communication | Encrypted using TLS |
| Directory queries | Secure directory queries |
| Suitable for trusted networks | Recommended for production environments |

LDAPS protects directory information from interception by encrypting communication between clients and directory servers.

---

# LDAP in Enterprise Environments

LDAP is widely used throughout Windows enterprise environments.

Common use cases include:

- Active Directory
- Windows logon
- Microsoft Outlook
- VPN authentication lookups
- HR applications
- Identity management systems
- Enterprise web applications

Nearly every domain-joined Windows environment depends on LDAP.

---

# Why LDAP Matters in SOC

LDAP activity provides valuable insight into Active Directory reconnaissance.

Attackers commonly use LDAP after gaining initial access to discover:

- Domain users
- Administrative accounts
- Security groups
- Domain Controllers
- Servers
- Service accounts
- Organizational structure

Unusual LDAP queries often indicate reconnaissance before privilege escalation or lateral movement.

---

# Common LDAP-Based Attacks

## LDAP Enumeration

Attackers query Active Directory to collect information about users, groups, computers, and network resources.

### Indicators

- High volume of LDAP queries
- Enumeration of all users or groups
- Requests for privileged accounts
- Queries originating from user workstations

---

## Active Directory Reconnaissance

Attackers gather information about the environment before launching additional attacks.

Common objectives include:

- Identifying Domain Admins
- Discovering Domain Controllers
- Enumerating servers
- Mapping organizational structure

---

## Credential Hunting

Attackers search LDAP for high-value accounts such as:

- Domain Admins
- Service Accounts
- Administrative Users
- Privileged Groups

Compromising these accounts significantly increases attacker capabilities.

---

## BloodHound / SharpHound Collection

BloodHound is a reconnaissance tool used to map Active Directory relationships.

SharpHound collects directory information through LDAP to identify attack paths toward privileged accounts.

### Indicators

- Large numbers of LDAP queries
- SharpHound execution
- Enumeration of privileged groups
- Relationship mapping activity

---

# SOC Investigation Workflow

When investigating suspicious LDAP activity, verify:

- Source endpoint
- Logged-in user
- User privilege level
- Number of LDAP queries
- Objects being queried
- Query destination
- Parent process
- Presence of SharpHound or PowerShell
- Authentication activity
- Related SMB connections
- EDR telemetry
- Evidence of privilege escalation or lateral movement

---

# Common SIEM Alerts

Examples include:

- Excessive LDAP Queries
- LDAP Enumeration Detected
- Suspicious Active Directory Discovery
- SharpHound Activity
- BloodHound Collection
- LDAP Queries from Workstation
- Unusual LDAP Search Filters
- Access to Privileged Group Information

---

# MITRE ATT&CK Mapping

| Tactic | LDAP Usage |
|---------|------------|
| Discovery | Enumerating users, groups, computers, and domain information |
| Credential Access | Identifying privileged and service accounts |
| Lateral Movement | Collecting information before moving to other systems |

---

# LDAP vs Kerberos

LDAP and Kerberos serve different purposes but work together in Active Directory.

| LDAP | Kerberos |
|------|-----------|
| Finds directory information | Authenticates users |
| Searches Active Directory | Verifies identity |
| Port 389 | Port 88 |
| Answers "Who is this user?" | Answers "Is this user authentic?" |

A simple way to remember the difference:

- **LDAP** finds information.
- **Kerberos** verifies identity.

---

# Interview Questions

### What is LDAP?

LDAP (Lightweight Directory Access Protocol) is a protocol used to access and manage directory services such as Microsoft Active Directory. It enables users and applications to search for directory objects including users, groups, computers, and other network resources.

---

### Which ports does LDAP use?

LDAP commonly uses:

- **389/TCP** for LDAP
- **636/TCP** for LDAPS

---

### What is the difference between LDAP and LDAPS?

LDAP communicates without encryption, while LDAPS encrypts communication using TLS to securely transmit directory information.

---

### Why is LDAP important for SOC Analysts?

LDAP is commonly used during Active Directory reconnaissance. Monitoring LDAP activity helps detect user enumeration, privilege discovery, and attacker reconnaissance before lateral movement.

---

### What is LDAP Enumeration?

LDAP Enumeration is the process of querying Active Directory to gather information about users, groups, computers, and privileged accounts. Attackers often perform enumeration before privilege escalation or lateral movement.

---

### How would you investigate a suspicious LDAP alert?

A typical investigation includes:

- Identifying the source endpoint
- Reviewing the user account
- Checking query volume
- Determining which directory objects were requested
- Reviewing the parent process
- Looking for SharpHound or PowerShell activity
- Correlating with SMB, authentication, and EDR telemetry

---

# Key Takeaways

| Topic | Summary |
|--------|---------|
| Purpose | Searches and manages directory information |
| Default Port | 389 |
| Secure Port | 636 |
| Encryption | LDAPS uses TLS |
| Enterprise Usage | Active Directory, Outlook, VPN, identity management |
| SOC Relevance | Critical source for Active Directory reconnaissance detection |
| Common Attacks | LDAP Enumeration, AD Reconnaissance, Credential Hunting, BloodHound Collection |

---

## Skills Demonstrated

- LDAP Fundamentals
- Active Directory
- Windows Networking
- Identity Management
- Threat Detection
- Active Directory Reconnaissance
- SIEM Analysis
- MITRE ATT&CK
- Blue Team Operations

---
