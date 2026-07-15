# Remote Desktop Protocol (RDP)

> A practical guide to understanding RDP from a Security Operations Center (SOC) Analyst's perspective.

---

## Table of Contents

- [Overview](#overview)
- [Why RDP Matters](#why-rdp-matters)
- [How RDP Works](#how-rdp-works)
- [Ports and Protocols](#ports-and-protocols)
- [RDP in Enterprise Environments](#rdp-in-enterprise-environments)
- [Why RDP Matters in SOC](#why-rdp-matters-in-soc)
- [Common RDP-Based Attacks](#common-rdp-based-attacks)
- [SOC Investigation Workflow](#soc-investigation-workflow)
- [Common SIEM Alerts](#common-siem-alerts)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Interview Questions](#interview-questions)
- [Key Takeaways](#key-takeaways)

---

# Overview

**Remote Desktop Protocol (RDP)** is a Microsoft protocol that allows users to remotely connect to and control another Windows computer over a network as if they were physically sitting in front of it.

RDP is commonly used by administrators, help desk teams, and system engineers to remotely manage servers and workstations.

RDP primarily communicates over **TCP Port 3389**.

---

# Why RDP Matters

Managing hundreds of computers across multiple locations would be inefficient without remote administration.

RDP enables administrators to securely access Windows systems without requiring physical access.

Organizations rely on RDP for:

- Remote administration
- Help Desk support
- Server management
- Software installation
- System maintenance
- Cloud virtual machine management

RDP significantly improves operational efficiency in enterprise environments.

---

# How RDP Works

```text
Administrator
      │
      ▼
Connects to Remote System
      │
      ▼
RDP Authentication
      │
      ▼
Desktop Session Established
      │
      ▼
Administrator Controls Remote Computer
```

Typical workflow:

1. Administrator initiates an RDP connection.
2. The remote system authenticates the user.
3. Authentication succeeds.
4. A remote desktop session is established.
5. The administrator performs required tasks.
6. The session is terminated after work is completed.

---

# Ports and Protocols

| Port | Protocol | Purpose |
|------|----------|---------|
| 3389 | TCP | Remote Desktop communication |
| 3389 | UDP* | Performance optimization (modern Windows versions) |

> **Note:** TCP is the primary protocol you should remember for interviews and SOC investigations.

### Why TCP?

RDP requires reliable communication because:

- Keyboard input must arrive correctly.
- Mouse movements must remain synchronized.
- Screen updates must be delivered in order.
- Session stability is essential during remote administration.

---

# RDP in Enterprise Environments

RDP is commonly used throughout Windows enterprise networks.

Common use cases include:

- Windows Server administration
- Help Desk support
- Remote troubleshooting
- Cloud virtual machines
- Software deployment
- System maintenance
- Administrative access to critical servers

Properly securing RDP is essential because it often provides direct access to high-value systems.

---

# Why RDP Matters in SOC

RDP is one of the most frequently targeted remote access services.

After obtaining valid credentials, attackers commonly use RDP to:

- Gain interactive remote access
- Execute malicious commands
- Install malware
- Disable security controls
- Create backdoor accounts
- Deploy ransomware
- Move laterally across the network

Monitoring RDP activity helps SOC analysts detect unauthorized access before it leads to a larger compromise.

---

# Common RDP-Based Attacks

## RDP Brute Force

Attackers repeatedly attempt different username and password combinations until valid credentials are discovered.

### Indicators

- Hundreds of failed login attempts
- Multiple usernames targeted
- Login attempts from unknown IP addresses
- Successful login after numerous failures

---

## Credential Stuffing

Attackers use usernames and passwords obtained from previous data breaches instead of guessing credentials.

---

## BlueKeep

BlueKeep is a critical vulnerability affecting older Windows RDP services that allowed remote code execution on vulnerable systems.

Keeping Windows systems fully patched is the primary mitigation.

---

## Stolen Credentials

Attackers obtain legitimate user credentials through phishing, malware, or credential theft and use them to authenticate via RDP.

### Indicators

- Successful logins from unusual locations
- Logins outside normal working hours
- New geographic locations
- Access from previously unseen devices

---

## Lateral Movement

After compromising one endpoint, attackers use RDP to move to additional systems inside the environment.

```text
Compromised Workstation
          │
          ▼
      File Server
          │
          ▼
   Domain Controller
```

Lateral movement is commonly observed during ransomware incidents.

---

# SOC Investigation Workflow

When investigating a suspicious RDP alert, verify:

- Source IP address
- Destination system
- Targeted user account
- Authentication success or failure
- Number of failed login attempts
- Successful login after repeated failures
- Login time
- Geographic location
- Parent process
- PowerShell or Command Prompt execution
- New local user creation
- Privilege escalation activity
- Related EDR detections
- Signs of ransomware or lateral movement

---

# Common SIEM Alerts

Examples include:

- Multiple Failed RDP Logins
- Successful Login After Brute Force
- RDP Login from Unusual Country
- RDP Login Outside Business Hours
- Remote Desktop Enabled
- BlueKeep Exploitation Attempt
- Suspicious Administrator Login
- RDP Lateral Movement
- Excessive Failed Authentication Attempts

---

# MITRE ATT&CK Mapping

| Tactic | RDP Usage |
|---------|-----------|
| Initial Access | External RDP using valid or stolen credentials |
| Persistence | Maintaining remote access |
| Lateral Movement | Remote Desktop sessions between hosts |
| Execution | Running commands after successful login |
| Defense Evasion | Using legitimate administrator accounts |

---

# Interview Questions

### What is RDP?

RDP (Remote Desktop Protocol) is a Microsoft protocol that allows users to remotely access and control another Windows computer over a network. It primarily uses **TCP Port 3389**.

---

### Why do organizations use RDP?

Organizations use RDP for remote administration, troubleshooting, software installation, server management, and supporting users without requiring physical access.

---

### Which port does RDP use?

RDP primarily uses:

- **3389/TCP**

Modern Windows versions may also use UDP to improve performance, but **TCP 3389** is the primary port.

---

### Why is RDP important for SOC Analysts?

RDP is frequently targeted by attackers because it provides direct remote access to Windows systems. It is commonly abused for brute-force attacks, credential theft, lateral movement, and ransomware deployment.

---

### Name common RDP attacks.

- RDP Brute Force
- Credential Stuffing
- BlueKeep
- Stolen Credentials
- Lateral Movement

---

### How would you investigate a suspicious RDP alert?

A typical investigation includes:

- Identifying the targeted user
- Reviewing the source IP address
- Checking the destination system
- Reviewing failed and successful login attempts
- Looking for PowerShell or Command Prompt activity
- Reviewing EDR detections
- Determining whether lateral movement or ransomware activity followed the login

---

# Key Takeaways

| Topic | Summary |
|--------|---------|
| Purpose | Provides remote access to Windows systems |
| Default Port | 3389 |
| Primary Protocol | TCP |
| Enterprise Usage | Remote administration, Help Desk, server management |
| SOC Relevance | Critical source for detecting unauthorized remote access |
| Common Attacks | Brute Force, Credential Stuffing, BlueKeep, Stolen Credentials, Lateral Movement |

---

