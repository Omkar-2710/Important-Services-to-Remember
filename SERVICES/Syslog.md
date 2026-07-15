# Syslog

> A practical guide to understanding Syslog from a Security Operations Center (SOC) Analyst's perspective.

---

## Table of Contents

- [Overview](#overview)
- [Why Syslog Matters](#why-syslog-matters)
- [How Syslog Works](#how-syslog-works)
- [Ports and Protocols](#ports-and-protocols)
- [Syslog Components](#syslog-components)
- [Syslog Message Structure](#syslog-message-structure)
- [Syslog in Enterprise Environments](#syslog-in-enterprise-environments)
- [Why Syslog Matters in SOC](#why-syslog-matters-in-soc)
- [Normal vs Suspicious Syslog Activity](#normal-vs-suspicious-syslog-activity)
- [Common Syslog-Based Attacks](#common-syslog-based-attacks)
- [SOC Investigation Workflow](#soc-investigation-workflow)
- [Common SIEM Alerts](#common-siem-alerts)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Interview Questions](#interview-questions)
- [Key Takeaways](#key-takeaways)

---

# Overview

**Syslog** is a standard logging protocol used to collect and forward log messages from network devices, operating systems, and security appliances to a centralized logging server or Security Information and Event Management (SIEM) platform.

Instead of reviewing logs on every individual device, administrators and SOC analysts can analyze events from a single centralized location.

Syslog commonly uses **UDP Port 514**, **TCP Port 514**, and **TCP Port 6514** for encrypted Syslog over TLS.

---

# Why Syslog Matters

Modern enterprise environments generate millions of log events every day.

Without centralized logging, investigating incidents across hundreds of devices would be extremely difficult.

Organizations use Syslog to:

- Centralize log collection
- Monitor security events
- Troubleshoot network issues
- Detect suspicious activity
- Meet compliance requirements
- Support forensic investigations
- Improve incident response

Centralized logging enables security teams to quickly identify and investigate threats across the organization. :contentReference[oaicite:1]{index=1}

---

# How Syslog Works

```text
Firewall
      │
Router
      │
Linux Server
      │
VPN Gateway
      │
Switch
      │
      ▼
Syslog Server / SIEM
      │
      ▼
SOC Analyst
```

Typical workflow:

1. A device generates an event.
2. The event is formatted as a Syslog message.
3. The message is forwarded to the Syslog server or SIEM.
4. The SIEM stores, indexes, and analyzes the log.
5. SOC analysts investigate alerts and correlate events.

---

# Ports and Protocols

| Port | Protocol | Purpose |
|------|----------|---------|
| 514 | UDP | Traditional Syslog |
| 514 | TCP | Reliable Syslog delivery |
| 6514 | TCP | Encrypted Syslog (TLS) |

### Why UDP?

Traditional Syslog uses UDP because it is lightweight and generates minimal network overhead.

### Why TCP?

TCP provides reliable log delivery by ensuring messages are received in the correct order.

### Why TLS?

TLS encrypts Syslog traffic, protecting sensitive log data from interception during transmission. :contentReference[oaicite:2]{index=2}

---

# Syslog Components

| Component | Purpose |
|-----------|---------|
| Log Source | Device generating the event |
| Syslog Server | Receives and stores Syslog messages |
| SIEM Platform | Correlates, analyzes, and alerts on log data |
| SOC Analyst | Investigates security events and incidents |

Common Syslog sources include:

- Linux Servers
- Firewalls
- Routers
- Switches
- IDS/IPS
- VPN Gateways
- Network Appliances

---

# Syslog Message Structure

A typical Syslog message contains:

```text
Timestamp
Hostname
Application
Severity Level
Log Message
```

Example:

```text
Jul 15 10:15:42 Firewall01 SSHD: Failed login attempt from 192.168.1.50
```

Key fields help analysts answer:

- When did it happen?
- Which device generated the event?
- Which application generated it?
- What happened?

---

# Syslog in Enterprise Environments

Syslog is widely used across enterprise infrastructure.

Common devices sending Syslog include:

- Cisco Firewalls
- Palo Alto Firewalls
- Fortinet Firewalls
- Check Point Firewalls
- Linux Servers
- Network Switches
- VPN Gateways
- IDS/IPS Appliances
- Proxy Servers

Many SIEM platforms, including Microsoft Sentinel, rely heavily on Syslog to collect logs from non-Windows devices. :contentReference[oaicite:3]{index=3}

---

# Why Syslog Matters in SOC

Syslog provides visibility into activity occurring across network infrastructure.

SOC analysts use Syslog to detect:

- Failed login attempts
- Firewall rule changes
- VPN activity
- Configuration changes
- Privilege escalation
- Service failures
- Network attacks
- Unauthorized administrative activity

Without Syslog, many security events from Linux systems, firewalls, routers, and other network devices would never reach the SIEM. :contentReference[oaicite:4]{index=4}

---

# Normal vs Suspicious Syslog Activity

| Normal Activity | Suspicious Activity |
|-----------------|---------------------|
| Regular firewall logging | Syslog service suddenly stops |
| Routine VPN authentication | Large number of failed logins |
| Scheduled configuration changes | Unauthorized firewall rule changes |
| Normal administrator activity | Log deletion or tampering |
| Continuous log forwarding | Log source suddenly goes offline |

Understanding normal logging behavior helps SOC analysts quickly detect attacks attempting to hide malicious activity.

---

# Common Syslog-Based Attacks

## Log Deletion

Attackers delete log files after compromising a system.

### Indicators

- Missing log entries
- Unexpected gaps in timestamps
- Log rotation anomalies

---

## Log Tampering

Attackers modify log files to conceal malicious actions.

### Indicators

- Modified timestamps
- Altered event details
- Missing authentication records

---

## Syslog Service Disabled

Attackers stop the Syslog service to prevent future events from being recorded.

### Indicators

- Sudden stop in log collection
- Missing events from affected devices
- Syslog service termination

---

## Unauthorized Configuration Changes

Attackers modify logging configurations to disable forwarding or reduce visibility.

### Indicators

- Log forwarding disabled
- New Syslog destination configured
- Retention settings modified

---

## Alert Flooding

Attackers intentionally generate excessive log events to overwhelm analysts or conceal malicious activity.

### Indicators

- Extremely high event volume
- Duplicate log messages
- Resource exhaustion on the SIEM

---

# SOC Investigation Workflow

When investigating a suspicious Syslog alert, verify:

- Which device generated the log?
- Who performed the action?
- When did the activity occur?
- Was it during an approved maintenance window?
- What changed?
- Was the change followed by suspicious activity?
- Were logs suddenly missing?
- Were there related authentication, firewall, or EDR alerts?

Always correlate Syslog events with other log sources to understand the complete attack timeline. :contentReference[oaicite:5]{index=5}

---

# Common SIEM Alerts

Examples include:

- Excessive Failed SSH Logins
- Firewall Rule Modified
- VPN Login from New Country
- Configuration Change Detected
- Syslog Service Stopped
- Log Source Offline
- High Severity Firewall Alert
- Unauthorized Administrator Login
- Multiple Authentication Failures

---

# MITRE ATT&CK Mapping

| Tactic | Syslog Usage |
|---------|--------------|
| Defense Evasion | Log deletion or disabling Syslog |
| Discovery | Reviewing logs to understand the environment |
| Persistence | Modifying logging configurations |
| Impact | Preventing log collection during an attack |

---

# Interview Questions

### What is Syslog?

Syslog is a standard logging protocol used by network devices, Linux systems, firewalls, routers, switches, and security appliances to send log messages to a centralized logging server or SIEM. :contentReference[oaicite:6]{index=6}

---

### Why do companies use Syslog?

Organizations use Syslog to centralize logs from multiple devices, making it easier to monitor security events, troubleshoot issues, and investigate incidents without logging into every device individually. :contentReference[oaicite:7]{index=7}

---

### Which ports does Syslog use?

Syslog commonly uses:

- **UDP Port 514**
- **TCP Port 514**
- **TCP Port 6514** (Syslog over TLS) :contentReference[oaicite:8]{index=8}

---

### Why is Syslog important for a SOC Analyst?

Syslog provides centralized visibility into activity occurring across servers, firewalls, routers, switches, and other network devices. It enables SOC analysts to detect threats, investigate incidents, and correlate events from multiple systems. :contentReference[oaicite:9]{index=9}

---

### Name some common attacks related to Syslog.

- Log Deletion
- Log Tampering
- Syslog Service Disabled
- Unauthorized Logging Configuration Changes
- Alert Flooding :contentReference[oaicite:10]{index=10}

---

### How would you investigate a Syslog alert?

A typical investigation includes:

- Identifying the device that generated the log
- Determining who performed the action
- Reviewing the configuration change or security event
- Checking whether the activity was authorized
- Looking for missing logs
- Correlating authentication, firewall, endpoint, and SIEM events to determine the full scope of the incident :contentReference[oaicite:11]{index=11}

---

# Key Takeaways

| Topic | Summary |
|--------|---------|
| Purpose | Centralized log collection and forwarding |
| Default Ports | UDP 514, TCP 514, TCP 6514 (TLS) |
| Common Sources | Linux Servers, Firewalls, Routers, Switches, VPN Devices |
| Enterprise Usage | Centralized monitoring and incident investigation |
| SOC Relevance | Critical source for threat detection and log correlation |
| Common Attacks | Log Deletion, Log Tampering, Disabled Logging, Alert Flooding |

---
