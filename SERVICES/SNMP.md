# Simple Network Management Protocol (SNMP)

> A practical guide to understanding SNMP from a Security Operations Center (SOC) Analyst's perspective.

---

## Table of Contents

- [Overview](#overview)
- [Why SNMP Matters](#why-snmp-matters)
- [How SNMP Works](#how-snmp-works)
- [Ports and Protocols](#ports-and-protocols)
- [SNMP Components](#snmp-components)
- [SNMP Versions](#snmp-versions)
- [SNMP in Enterprise Environments](#snmp-in-enterprise-environments)
- [Why SNMP Matters in SOC](#why-snmp-matters-in-soc)
- [Normal vs Suspicious SNMP Activity](#normal-vs-suspicious-snmp-activity)
- [Common SNMP-Based Attacks](#common-snmp-based-attacks)
- [SOC Investigation Workflow](#soc-investigation-workflow)
- [Common SIEM Alerts](#common-siem-alerts)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Interview Questions](#interview-questions)
- [Key Takeaways](#key-takeaways)

---

# Overview

**Simple Network Management Protocol (SNMP)** is a network management protocol used to monitor, manage, and collect information from network devices such as routers, switches, firewalls, servers, printers, wireless controllers, and storage devices.

Rather than manually checking every network device, administrators use SNMP to centrally monitor device health, performance, and availability.

SNMP primarily uses **UDP Port 161** for communication and **UDP Port 162** for SNMP Traps (alerts).

---

# Why SNMP Matters

Modern enterprise networks may contain hundreds or even thousands of network devices.

Monitoring each device manually is impractical.

SNMP enables centralized monitoring of:

- Routers
- Switches
- Firewalls
- Wireless Controllers
- Access Points
- Servers
- Network Printers
- UPS Devices
- Storage Devices (NAS/SAN)

This allows administrators to quickly identify performance issues, outages, and hardware failures before they impact business operations.

---

# How SNMP Works

```text
Monitoring Server (SNMP Manager)
              │
        SNMP Request
              │
              ▼
Network Device (SNMP Agent)
              │
 Returns Device Information
(CPU, Memory, Interfaces, Uptime)
              │
              ▼
Monitoring Dashboard
```

Typical workflow:

1. The SNMP Manager sends a request to a network device.
2. The SNMP Agent running on the device receives the request.
3. The agent retrieves information from the device's MIB.
4. The requested information is returned to the monitoring server.
5. The monitoring platform displays the collected data.

Some events are reported automatically through **SNMP Traps**, without waiting for a request.

---

# Ports and Protocols

| Port | Protocol | Purpose |
|------|----------|---------|
| 161 | UDP | SNMP queries between the Manager and Agent |
| 162 | UDP | SNMP Traps (device-generated alerts) |

### Easy Way to Remember

- **161 → Ask**
- **162 → Alert**

---

# SNMP Components

SNMP consists of three main components.

| Component | Purpose |
|-----------|---------|
| SNMP Manager | Monitoring system that collects information from devices |
| SNMP Agent | Software running on the network device that responds to requests |
| MIB (Management Information Base) | Database containing information about the device |

Examples of information stored in the MIB include:

- CPU utilization
- Memory usage
- Device uptime
- Interface status
- Device name
- Temperature

---

# SNMP Versions

| Version | Security |
|---------|----------|
| SNMPv1 | Basic implementation with minimal security |
| SNMPv2c | Improved performance but still relies on community strings |
| SNMPv3 | Authentication, encryption, and improved security |

**Interview Tip**

If asked which version is preferred, answer:

> **SNMPv3**, because it provides authentication and encryption, making it significantly more secure than earlier versions.

---

# SNMP in Enterprise Environments

SNMP is widely used throughout enterprise networks.

Common use cases include:

- Monitoring routers
- Monitoring switches
- Monitoring firewalls
- Monitoring servers
- Monitoring storage systems
- Monitoring printers
- Tracking interface status
- Monitoring CPU and memory utilization
- Receiving automatic device alerts

Most enterprise monitoring platforms rely on SNMP to maintain visibility into network infrastructure.

---

# Why SNMP Matters in SOC

Network infrastructure is often one of the first targets during a cyber attack.

Attackers may use SNMP to gather information about:

- Network devices
- IP addressing
- Routing information
- Device models
- Software versions
- Network topology

SOC analysts monitor SNMP activity to detect:

- Unauthorized network reconnaissance
- Excessive SNMP queries
- Default community string usage
- Unauthorized configuration changes
- Device failures
- Critical infrastructure alerts

Monitoring SNMP activity provides early visibility into reconnaissance targeting network infrastructure.

---

# Normal vs Suspicious SNMP Activity

| Normal Activity | Suspicious Activity |
|-----------------|---------------------|
| Monitoring server polls routers and switches | Employee workstation sends SNMP requests |
| Regular CPU and memory monitoring | Hundreds of SNMP requests in a short period |
| Periodic health checks | SNMP traffic from an external IP address |
| Device sends Trap for high CPU usage | Repeated use of default community strings (`public`, `private`) |
| Authorized monitoring platform communicates with infrastructure | Unexpected SNMP write operations |

Understanding normal monitoring behavior helps SOC analysts quickly identify reconnaissance and unauthorized device access.

---

# Common SNMP-Based Attacks

## SNMP Enumeration

Attackers query SNMP-enabled devices to collect information about the network.

### Indicators

- Device information requests
- Interface enumeration
- Routing table collection
- Software version discovery
- Network topology mapping

---

## Default Community Strings

Older SNMP versions commonly use default community strings such as:

- public
- private

If these defaults remain unchanged, attackers may gain unauthorized access to device information.

---

## Information Disclosure

Poorly configured SNMP services may expose:

- Hostnames
- IP addresses
- Device models
- Routing information
- Interface details
- Network topology

This information is valuable during the reconnaissance phase of an attack.

---

## Unauthorized Configuration Changes

If SNMP write access is enabled, attackers may modify device configurations.

Examples include:

- Disabling interfaces
- Changing routing configurations
- Reconfiguring firewalls
- Modifying network settings

---

## Denial of Service (DoS)

Attackers may overwhelm devices with excessive SNMP requests, affecting performance and availability.

---

# SOC Investigation Workflow

When investigating a suspicious SNMP alert, verify:

- Source IP address
- Source device
- Target network device
- Read or write operation
- Community string used
- Number of queried devices
- Presence of SNMP Traps
- Device health status
- Authentication failures
- Configuration changes
- Related SSH or administrative logins
- Firewall logs
- EDR telemetry

Correlate SNMP activity with authentication, firewall, and endpoint events to determine whether it is part of a larger attack.

---

# Common SIEM Alerts

Examples include:

- SNMP Enumeration
- Unauthorized SNMP Access
- Default Community String Detected
- Excessive SNMP Requests
- SNMP Authentication Failure
- SNMP Write Operation
- Device Reboot Trap
- Interface Down Trap
- High CPU Trap

---

# MITRE ATT&CK Mapping

| Tactic | SNMP Usage |
|---------|------------|
| Discovery | SNMP Enumeration |
| Credential Access | Guessing default community strings |
| Impact | Unauthorized device configuration changes |
| Defense Evasion | Disabling monitoring through device modifications |

---

# Interview Questions

### What is SNMP?

SNMP (Simple Network Management Protocol) is a protocol used to monitor and manage network devices such as routers, switches, firewalls, servers, and printers. It primarily uses **UDP Port 161** for queries and **UDP Port 162** for SNMP Traps.

---

### Why do companies use SNMP?

Organizations use SNMP to centrally monitor the health and performance of network devices, including CPU usage, memory utilization, interface status, and device uptime.

---

### Which ports does SNMP use?

SNMP uses:

- **UDP Port 161** for SNMP queries.
- **UDP Port 162** for SNMP Traps.

---

### What is an SNMP Trap?

An SNMP Trap is an unsolicited alert sent by a network device to the monitoring server when an important event occurs, such as a device reboot, interface failure, or high CPU utilization.

---

### Name common SNMP attacks.

- SNMP Enumeration
- Default Community String Abuse
- Information Disclosure
- Unauthorized Configuration Changes
- Denial of Service (DoS)

---

### Which SNMP version is most secure?

**SNMPv3** is the preferred version because it supports authentication and encryption, providing stronger security than SNMPv1 and SNMPv2c.

---

### How would you investigate a suspicious SNMP alert?

A typical investigation includes:

- Identifying the source device
- Determining the targeted network device
- Verifying whether the request was a read or write operation
- Checking the community string used
- Looking for configuration changes
- Reviewing SNMP Traps
- Correlating with SSH, firewall, authentication, and EDR logs

---

# Key Takeaways

| Topic | Summary |
|--------|---------|
| Purpose | Monitor and manage network devices |
| Default Ports | UDP 161 (Queries), UDP 162 (Traps) |
| Main Components | SNMP Manager, SNMP Agent, MIB |
| Preferred Version | SNMPv3 |
| Enterprise Usage | Routers, Switches, Firewalls, Servers, Printers |
| SOC Relevance | Critical for monitoring network infrastructure and detecting reconnaissance |
| Common Attacks | Enumeration, Default Community Strings, Information Disclosure, Unauthorized Write Operations, DoS |

---
