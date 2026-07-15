# Kerberos

> A practical guide to understanding Kerberos from a Security Operations Center (SOC) Analyst's perspective.

---

## Table of Contents

- [Overview](#overview)
- [Why Kerberos Matters](#why-kerberos-matters)
- [How Kerberos Works](#how-kerberos-works)
- [Ports and Protocols](#ports-and-protocols)
- [Single Sign-On (SSO)](#single-sign-on-sso)
- [Kerberos in Enterprise Environments](#kerberos-in-enterprise-environments)
- [Kerberos vs LDAP](#kerberos-vs-ldap)
- [Why Kerberos Matters in SOC](#why-kerberos-matters-in-soc)
- [Common Kerberos Attacks](#common-kerberos-attacks)
- [SOC Investigation Workflow](#soc-investigation-workflow)
- [Common SIEM Alerts](#common-siem-alerts)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Interview Questions](#interview-questions)
- [Key Takeaways](#key-takeaways)

---

# Overview

**Kerberos** is a network authentication protocol used in **Windows Active Directory** environments to securely verify the identity of users and computers.

Instead of repeatedly sending passwords across the network, Kerberos authenticates users using **encrypted tickets** issued by the **Key Distribution Center (KDC)**, which runs on the **Domain Controller (DC)**.

Kerberos primarily uses **TCP and UDP Port 88**.

---

# Why Kerberos Matters

Modern organizations rely on hundreds of applications and services every day.

Without Kerberos, users would have to enter their username and password every time they accessed:

- Shared folders
- Microsoft Outlook
- Microsoft Teams
- SQL Server
- SharePoint
- Internal web applications
- Remote Desktop
- Enterprise services

Kerberos solves this problem by providing **Single Sign-On (SSO)**, allowing users to authenticate once and securely access multiple authorized services.

---

# How Kerberos Works

```text
User
   │
   ▼
Windows Logon
   │
   ▼
Domain Controller (KDC)
   │
Verifies Identity
   │
   ▼
Issues Kerberos Ticket
   │
   ▼
User Requests Service
   │
   ▼
Presents Ticket
   │
   ▼
Service Validates Ticket
   │
   ▼
Access Granted
```

Typical workflow:

1. User logs into the Windows domain.
2. Domain Controller verifies the user's credentials.
3. A Kerberos ticket is issued.
4. The user requests access to a network service.
5. The service validates the ticket.
6. Access is granted without requiring the password again.

---

# Ports and Protocols

| Port | Protocol | Purpose |
|------|----------|---------|
| 88 | TCP | Kerberos Authentication |
| 88 | UDP | Kerberos Authentication |

Most modern environments primarily use **TCP**, although Kerberos also supports UDP.

---

# Single Sign-On (SSO)

One of Kerberos' biggest advantages is **Single Sign-On (SSO)**.

Instead of authenticating separately to every service, users authenticate once and receive a trusted ticket.

Benefits include:

- Reduced password exposure
- Improved user experience
- Centralized authentication
- Stronger security
- Simplified access to enterprise resources

---

# Kerberos in Enterprise Environments

Kerberos is used throughout Windows domain environments, including:

- Windows Domain Logon
- Active Directory
- SMB File Sharing
- Microsoft Exchange
- Microsoft SQL Server
- SharePoint
- Remote Desktop
- Enterprise Applications

Almost every domain-joined Windows computer relies on Kerberos for authentication.

---

# Kerberos vs LDAP

Although Kerberos and LDAP are closely related, they perform different roles.

| LDAP | Kerberos |
|------|-----------|
| Searches directory information | Authenticates users |
| Finds users, groups, and computers | Verifies identity |
| Port 389 | Port 88 |
| Directory Service | Authentication Protocol |

A simple way to remember:

- **LDAP** answers: *"Who is this user?"*
- **Kerberos** answers: *"Is this user really who they claim to be?"*

---

# Why Kerberos Matters in SOC

Kerberos is one of the most targeted authentication protocols in Active Directory environments.

Attackers abuse Kerberos to:

- Steal authentication tickets
- Access sensitive resources
- Move laterally across the network
- Escalate privileges
- Maintain persistence
- Avoid repeated password authentication

Monitoring Kerberos activity helps SOC analysts identify credential attacks before they lead to domain compromise.

---

# Common Kerberos Attacks

## Kerberoasting

Attackers request Kerberos service tickets for service accounts and attempt to crack them offline to recover passwords.

### Indicators

- Excessive service ticket requests
- Requests for privileged service accounts
- Unusual ticket activity from workstations

---

## Pass-the-Ticket (PtT)

Instead of stealing passwords, attackers steal valid Kerberos tickets and reuse them to authenticate to other systems.

---

## Golden Ticket

Attackers compromise the **KRBTGT** account on the Domain Controller and forge valid Kerberos Ticket Granting Tickets (TGTs).

This allows attackers to impersonate virtually any user in the domain.

---

## Silver Ticket

Attackers forge Kerberos service tickets for a specific service without compromising the entire domain.

Common targets include:

- File Servers
- SQL Servers
- IIS Web Servers

---

## AS-REP Roasting

Attackers request authentication data for user accounts that do not require Kerberos pre-authentication and attempt to crack the returned data offline.

---

# SOC Investigation Workflow

When investigating suspicious Kerberos activity, verify:

- Source endpoint
- Logged-in user
- Privilege level
- Authentication success or failure
- Number of ticket requests
- Service accounts involved
- Authentication time
- Parent process
- Related SMB or RDP activity
- PowerShell execution
- EDR detections
- Signs of lateral movement

---

# Common SIEM Alerts

Examples include:

- Kerberoasting Attempt
- Suspicious Kerberos Ticket Request
- Golden Ticket Detection
- Silver Ticket Detection
- Pass-the-Ticket Activity
- Excessive Ticket Requests
- Abnormal Kerberos Authentication
- Authentication from Unusual Host
- Service Account Abuse

---

# MITRE ATT&CK Mapping

| Tactic | Kerberos Usage |
|---------|----------------|
| Credential Access | Kerberoasting, AS-REP Roasting |
| Lateral Movement | Pass-the-Ticket |
| Persistence | Golden Ticket |
| Privilege Escalation | Forged Kerberos Tickets |
| Defense Evasion | Using stolen or forged Kerberos tickets |

---

# Interview Questions

### What is Kerberos?

Kerberos is a network authentication protocol used in Windows Active Directory environments. It securely verifies user identities using encrypted tickets instead of repeatedly transmitting passwords across the network.

---

### Which port does Kerberos use?

Kerberos primarily uses:

- **88/TCP**
- **88/UDP**

---

### What is Single Sign-On (SSO)?

Single Sign-On allows users to authenticate once and access multiple authorized services using Kerberos tickets without repeatedly entering their credentials.

---

### What is the difference between LDAP and Kerberos?

LDAP is used to search and manage directory information, whereas Kerberos is responsible for authenticating users and issuing trusted authentication tickets.

---

### Name common Kerberos attacks.

- Kerberoasting
- Pass-the-Ticket
- Golden Ticket
- Silver Ticket
- AS-REP Roasting

---

### How would you investigate a suspicious Kerberos alert?

A typical investigation includes:

- Identifying the user account
- Reviewing the source endpoint
- Checking ticket request activity
- Looking for privileged or service account abuse
- Reviewing authentication patterns
- Correlating with SMB, RDP, PowerShell, and EDR telemetry
- Determining whether the activity indicates credential theft or lateral movement

---

# Key Takeaways

| Topic | Summary |
|--------|---------|
| Purpose | Authenticates users using encrypted tickets |
| Default Port | 88 |
| Protocol | TCP / UDP |
| Ticket Issued By | Domain Controller (KDC) |
| Enterprise Usage | Active Directory, Windows Logon, SMB, SQL Server, Exchange |
| SOC Relevance | Critical protocol for authentication monitoring and credential attack detection |
| Common Attacks | Kerberoasting, Pass-the-Ticket, Golden Ticket, Silver Ticket, AS-REP Roasting |

---

## Skills Demonstrated

- Kerberos Fundamentals
- Windows Authentication
- Active Directory
- Single Sign-On (SSO)
- Threat Detection
- Incident Investigation
- SIEM Analysis
- MITRE ATT&CK
- Blue Team Operations

---
