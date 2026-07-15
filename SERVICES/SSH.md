# Secure Shell (SSH)

> A practical guide to understanding SSH from a Security Operations Center (SOC) Analyst's perspective.

---

## Table of Contents

- [Overview](#overview)
- [Why SSH Matters](#why-ssh-matters)
- [How SSH Works](#how-ssh-works)
- [Ports and Protocols](#ports-and-protocols)
- [SSH Authentication Methods](#ssh-authentication-methods)
- [SSH in Enterprise Environments](#ssh-in-enterprise-environments)
- [Why SSH Matters in SOC](#why-ssh-matters-in-soc)
- [Common SSH-Based Attacks](#common-ssh-based-attacks)
- [SOC Investigation Workflow](#soc-investigation-workflow)
- [Common SIEM Alerts](#common-siem-alerts)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Interview Questions](#interview-questions)
- [Key Takeaways](#key-takeaways)

---

# Overview

**Secure Shell (SSH)** is a secure network protocol used to remotely access and manage Linux, Unix, cloud systems, and network devices over an encrypted connection.

Unlike Telnet, SSH encrypts all communication between the client and the remote system, protecting credentials and commands from interception.

SSH primarily communicates over **TCP Port 22**.

---

# Why SSH Matters

Managing Linux servers, cloud infrastructure, and network devices would be difficult without secure remote administration.

SSH enables administrators to remotely perform tasks such as:

- Server administration
- System maintenance
- Configuration management
- Software deployment
- Log analysis
- Secure file transfers
- Network device management

SSH has become the standard protocol for secure remote administration in enterprise environments.

---

# How SSH Works

```text
Administrator
      │
      ▼
SSH Client
      │
      ▼
TCP Connection (Port 22)
      │
      ▼
Authentication
      │
      ▼
Encrypted SSH Session
      │
      ▼
Remote Linux / Unix Server
```

Typical workflow:

1. Administrator initiates an SSH connection.
2. SSH establishes a TCP connection to the remote system.
3. User authentication is performed.
4. An encrypted session is created.
5. Commands are executed securely on the remote host.
6. The session ends when administration is complete.

---

# Ports and Protocols

| Port | Protocol | Purpose |
|------|----------|---------|
| 22 | TCP | Secure remote administration |

### Why TCP?

SSH uses TCP because remote administration requires:

- Reliable communication
- Ordered packet delivery
- Error detection
- Secure session management

These features ensure commands are executed accurately without packet loss.

---

# SSH Authentication Methods

SSH supports multiple authentication mechanisms.

| Method | Description |
|---------|-------------|
| Password Authentication | User authenticates using a username and password. |
| SSH Key Authentication | Uses a public/private key pair instead of a password. |
| Multi-Factor Authentication (MFA) | Adds an additional authentication factor for improved security. |

SSH key authentication is widely used because it provides stronger security than password-based authentication.

---

# SSH in Enterprise Environments

SSH is commonly used throughout enterprise Linux and cloud environments.

Common use cases include:

- Linux server administration
- Cloud virtual machines (AWS, Azure, GCP)
- Network devices (Routers, Switches, Firewalls)
- DevOps automation
- Application servers
- Database servers
- Container hosts

SSH is a critical protocol for managing production infrastructure.

---

# Why SSH Matters in SOC

SSH activity provides valuable visibility into remote administrative access.

Attackers frequently abuse SSH after obtaining valid credentials or stolen SSH keys to:

- Gain unauthorized remote access
- Execute malicious commands
- Install malware
- Create persistence
- Exfiltrate sensitive data
- Move laterally across Linux systems

Monitoring SSH authentication and session activity helps SOC analysts identify suspicious administrative behavior before it escalates into a larger compromise.

---

# Common SSH-Based Attacks

## SSH Brute Force

Attackers repeatedly attempt different username and password combinations until valid credentials are discovered.

### Indicators

- High volume of failed login attempts
- Repeated authentication failures
- Login attempts from unknown IP addresses
- Successful login following multiple failures

---

## Stolen SSH Keys

Attackers steal private SSH keys from compromised systems and use them to authenticate without requiring passwords.

### Indicators

- New SSH keys added to `authorized_keys`
- Logins from unfamiliar systems
- Unexpected key-based authentication

---

## Credential Theft

Compromised usernames and passwords are used to gain unauthorized SSH access.

Common sources include:

- Phishing attacks
- Password reuse
- Malware
- Credential dumping

---

## Unauthorized Remote Access

Attackers use legitimate SSH sessions to remotely control compromised Linux systems.

### Indicators

- Logins outside business hours
- Access from unusual geographic locations
- Unexpected administrator activity

---

## Lateral Movement

After compromising one Linux system, attackers use SSH to access additional servers within the environment.

```text
Compromised Linux Server
           │
           ▼
      SSH Connection
           │
           ▼
      Internal Server
           │
           ▼
     Critical Database
```

SSH is commonly abused for lateral movement in Linux-based environments.

---

# SOC Investigation Workflow

When investigating a suspicious SSH alert, verify:

- Source IP address
- Destination server
- User account
- Authentication success or failure
- Number of failed login attempts
- Login time
- Geographic location
- Commands executed
- Newly added SSH keys
- Parent process
- Related authentication logs
- EDR telemetry
- Signs of lateral movement

---

# Common SIEM Alerts

Examples include:

- Multiple Failed SSH Logins
- Successful Login After Brute Force
- SSH Login from Unusual Country
- Unauthorized SSH Access
- New SSH Key Added
- SSH Session Outside Business Hours
- Suspicious Command Execution
- SSH Lateral Movement
- Excessive Authentication Failures

---

# MITRE ATT&CK Mapping

| Tactic | SSH Usage |
|---------|-----------|
| Initial Access | SSH using valid or stolen credentials |
| Persistence | Unauthorized SSH keys |
| Execution | Remote command execution |
| Lateral Movement | SSH sessions between Linux systems |
| Credential Access | Brute force and credential theft |

---

# Interview Questions

### What is SSH?

SSH (Secure Shell) is a secure protocol used to remotely access and manage Linux, Unix, and network devices over an encrypted connection. It primarily uses **TCP Port 22**. :contentReference[oaicite:0]{index=0}

---

### Why do companies use SSH?

Organizations use SSH to securely administer Linux servers, cloud instances, and network devices. It encrypts communication and supports authentication using passwords or SSH keys. :contentReference[oaicite:1]{index=1}

---

### Which port does SSH use?

SSH uses **TCP Port 22**. :contentReference[oaicite:2]{index=2}

---

### Why is SSH more secure than Telnet?

SSH encrypts all communication, including authentication and command execution, whereas Telnet transmits data in plaintext. This protects against eavesdropping and credential theft. :contentReference[oaicite:3]{index=3}

---

### Name common SSH attacks.

- SSH Brute Force
- Stolen SSH Keys
- Credential Theft
- Unauthorized Remote Access
- Lateral Movement :contentReference[oaicite:4]{index=4}

---

### How would you investigate a suspicious SSH alert?

A typical investigation includes:

- Identifying the user account
- Reviewing the source IP address
- Checking the destination server
- Reviewing failed and successful login attempts
- Examining executed commands
- Looking for newly added SSH keys
- Correlating authentication logs with EDR telemetry
- Determining whether lateral movement occurred :contentReference[oaicite:5]{index=5}

---

# Key Takeaways

| Topic | Summary |
|--------|---------|
| Purpose | Secure remote administration of Linux, Unix, and network devices |
| Default Port | 22 |
| Protocol | TCP |
| Authentication | Passwords or SSH Keys |
| Enterprise Usage | Linux servers, cloud environments, routers, switches, firewalls |
| SOC Relevance | Critical source for monitoring remote administrative access |
| Common Attacks | Brute Force, Stolen SSH Keys, Credential Theft, Unauthorized Access, Lateral Movement |

---

