# Dynamic Host Configuration Protocol (DHCP)

> A practical guide to understanding DHCP from a Security Operations Center (SOC) Analyst's perspective.

---

## Table of Contents

- [Overview](#overview)
- [Why DHCP Matters](#why-dhcp-matters)
- [How DHCP Works](#how-dhcp-works)
- [Ports and Protocols](#ports-and-protocols)
- [The DHCP DORA Process](#the-dhcp-dora-process)
- [DHCP in Enterprise Environments](#dhcp-in-enterprise-environments)
- [Why DHCP Matters in SOC](#why-dhcp-matters-in-soc)
- [Normal vs Suspicious DHCP Activity](#normal-vs-suspicious-dhcp-activity)
- [Common DHCP-Based Attacks](#common-dhcp-based-attacks)
- [SOC Investigation Workflow](#soc-investigation-workflow)
- [Common SIEM Alerts](#common-siem-alerts)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Interview Questions](#interview-questions)
- [Key Takeaways](#key-takeaways)

---

# Overview

**Dynamic Host Configuration Protocol (DHCP)** is a network protocol that automatically assigns IP addresses and other network configuration settings to devices when they join a network.

Instead of manually configuring every device, DHCP automatically provides:

- IP Address
- Subnet Mask
- Default Gateway
- DNS Server

DHCP primarily uses **UDP Port 67** (Server) and **UDP Port 68** (Client).

---

# Why DHCP Matters

Modern organizations connect hundreds or thousands of devices to their networks.

Without DHCP, administrators would have to manually configure network settings for every device, increasing the risk of errors and IP address conflicts.

Organizations use DHCP to:

- Automatically assign IP addresses
- Simplify network administration
- Prevent IP conflicts
- Reduce manual configuration
- Support employee laptops
- Support mobile devices
- Support virtual machines
- Enable plug-and-play network connectivity

DHCP allows new devices to join the network with minimal administrative effort. :contentReference[oaicite:1]{index=1}

---

# How DHCP Works

```text
New Device
     │
     ▼
DHCP Discover
     │
     ▼
DHCP Server
     │
     ▼
DHCP Offer
     │
     ▼
DHCP Request
     │
     ▼
DHCP Acknowledge
     │
     ▼
Device Receives IP Address
```

Whenever a new device connects to the network, it requests network configuration from the DHCP server.

The communication follows the **DORA** process.

---

# Ports and Protocols

| Port | Protocol | Purpose |
|------|----------|---------|
| 67 | UDP | DHCP Server |
| 68 | UDP | DHCP Client |

### Easy Way to Remember

- **67 → Server**
- **68 → Client**

### Why UDP?

DHCP uses UDP because a new device does not yet have an IP address or an established connection.

UDP allows lightweight communication during the initial network configuration process. :contentReference[oaicite:2]{index=2}

---

# The DHCP DORA Process

Every DHCP transaction follows four steps known as **DORA**.

## Discover

The client broadcasts:

> "Is there a DHCP server available?"

---

## Offer

The DHCP server replies:

> "I can offer you this IP address."

---

## Request

The client responds:

> "I would like to use this IP address."

---

## Acknowledge

The DHCP server confirms:

> "Approved. You may use this IP address."

```text
Client
   │
Discover
   ▼
DHCP Server
   │
Offer
   ▼
Client
   │
Request
   ▼
DHCP Server
   │
Acknowledge
   ▼
Client Gets IP Address
```

**Interview Tip**

If asked *"How does DHCP work?"*, simply answer:

> DHCP works through the **DORA** process:
> **Discover → Offer → Request → Acknowledge.** :contentReference[oaicite:3]{index=3}

---

# DHCP in Enterprise Environments

DHCP is used throughout enterprise networks.

Common examples include:

- Employee laptops
- Desktop computers
- Virtual machines
- Wireless devices
- VoIP phones
- Printers
- Servers
- Guest networks
- Corporate Wi-Fi

Most enterprise devices rely on DHCP to obtain their network configuration automatically.

---

# Why DHCP Matters in SOC

DHCP logs answer one of the most important questions during an investigation:

> **"Who was using this IP address at a specific time?"**

For example:

```text
SIEM Alert
192.168.10.55
Downloaded Malware
        │
        ▼
Check DHCP Logs
        │
        ▼
Identify Hostname
        │
        ▼
Identify User
```

Without DHCP logs, investigators may know the malicious IP address but not the device or user associated with it.

This makes DHCP logs extremely valuable for incident response and forensic investigations. :contentReference[oaicite:4]{index=4}

---

# Normal vs Suspicious DHCP Activity

| Normal Activity | Suspicious Activity |
|-----------------|---------------------|
| Device requests an IP address after joining the network | Hundreds of DHCP requests from one MAC address |
| Single authorized DHCP server responds | Multiple DHCP servers responding on the same network |
| Device receives one IP lease | Frequent IP address changes |
| Approved employee devices obtain leases | Unknown devices receiving IP addresses |
| Normal DORA communication | IP address pool exhaustion |

Understanding normal DHCP behavior helps SOC analysts quickly detect rogue devices and denial-of-service attacks. :contentReference[oaicite:5]{index=5}

---

# Common DHCP-Based Attacks

## Rogue DHCP Server

An attacker connects an unauthorized DHCP server to the network.

Instead of receiving legitimate network settings, devices receive malicious configurations.

### Possible Impact

- Fake DNS server
- Fake Default Gateway
- Traffic redirection
- Phishing
- Man-in-the-Middle attacks

---

## DHCP Starvation Attack

Attackers flood the DHCP server with thousands of fake DHCP requests until all available IP addresses are exhausted.

### Indicators

- Large number of DHCP Discover messages
- IP address pool exhaustion
- New devices unable to obtain IP addresses

---

## Man-in-the-Middle (MITM)

A rogue DHCP server provides malicious network settings.

Victim traffic is redirected through the attacker's system.

---

## Unauthorized Device Access

An attacker connects an unauthorized laptop or device to the corporate network.

The device receives an IP address from DHCP and gains network connectivity.

This is why many organizations implement **Network Access Control (NAC)**.

---

# SOC Investigation Workflow

When investigating a suspicious DHCP alert, verify:

- DHCP server involved
- Whether the DHCP server is authorized
- Source MAC address
- Hostname
- Assigned IP address
- Devices that received IP addresses
- DNS server assigned
- Default gateway assigned
- Signs of DHCP starvation
- Presence of multiple DHCP servers
- Related authentication logs
- Firewall logs
- EDR telemetry

Correlate DHCP events with DNS, authentication, and endpoint logs to determine whether they are part of a broader attack. :contentReference[oaicite:6]{index=6}

---

# Common SIEM Alerts

Examples include:

- Rogue DHCP Server Detected
- DHCP Starvation Attack
- IP Address Conflict
- Unauthorized Device Connected
- Unusual DHCP Lease Activity
- DHCP Scope Exhausted
- Multiple DHCP Servers Detected

---

# MITRE ATT&CK Mapping

| Tactic | DHCP Usage |
|---------|------------|
| Initial Access | Unauthorized device joins the network |
| Credential Access | Redirecting users to malicious DNS via Rogue DHCP |
| Collection | Gathering client network information |
| Impact | DHCP Starvation causing Denial-of-Service |

---

# Interview Questions

### What is DHCP?

DHCP (Dynamic Host Configuration Protocol) automatically assigns IP addresses and network settings, such as the subnet mask, default gateway, and DNS server, to devices joining a network. It uses **UDP Ports 67 and 68**. :contentReference[oaicite:7]{index=7}

---

### Why do companies use DHCP?

Organizations use DHCP to automate IP address management, reduce manual configuration, prevent IP conflicts, and simplify network administration. :contentReference[oaicite:8]{index=8}

---

### Which ports does DHCP use?

DHCP uses:

- **UDP Port 67** (Server)
- **UDP Port 68** (Client) :contentReference[oaicite:9]{index=9}

---

### Explain the DHCP process.

DHCP follows the **DORA** process:

- Discover
- Offer
- Request
- Acknowledge

This process allows a client to automatically obtain an IP address from a DHCP server. :contentReference[oaicite:10]{index=10}

---

### What is a Rogue DHCP Server?

A Rogue DHCP Server is an unauthorized DHCP server that provides incorrect network configuration to devices. It can redirect users to malicious DNS servers or gateways and facilitate Man-in-the-Middle attacks. :contentReference[oaicite:11]{index=11}

---

### What is DHCP Starvation?

DHCP Starvation is a Denial-of-Service attack where an attacker sends a large number of fake DHCP requests until the DHCP server runs out of available IP addresses, preventing legitimate devices from joining the network. :contentReference[oaicite:12]{index=12}

---

### How would you investigate a suspicious DHCP alert?

A typical investigation includes:

- Verifying the DHCP server
- Identifying affected devices
- Reviewing assigned DNS and gateway settings
- Looking for rogue DHCP servers
- Checking for DHCP starvation activity
- Correlating DHCP events with DNS, firewall, authentication, and endpoint logs

---

# Key Takeaways

| Topic | Summary |
|--------|---------|
| Purpose | Automatically assigns IP addresses and network settings |
| Default Ports | UDP 67 (Server), UDP 68 (Client) |
| DHCP Process | DORA (Discover, Offer, Request, Acknowledge) |
| Enterprise Usage | Automatic IP address management |
| SOC Relevance | Maps IP addresses to devices during investigations |
| Common Attacks | Rogue DHCP, DHCP Starvation, MITM, Unauthorized Device Access |

---
