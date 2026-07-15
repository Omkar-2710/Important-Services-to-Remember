# Internet Control Message Protocol (ICMP)

> A practical guide to understanding ICMP from a Security Operations Center (SOC) Analyst's perspective.

---

## Table of Contents

- [Overview](#overview)
- [Why ICMP Matters](#why-icmp-matters)
- [How ICMP Works](#how-icmp-works)
- [Ports and Protocols](#ports-and-protocols)
- [Common ICMP Message Types](#common-icmp-message-types)
- [Common ICMP Tools](#common-icmp-tools)
- [ICMP in Enterprise Environments](#icmp-in-enterprise-environments)
- [Why ICMP Matters in SOC](#why-icmp-matters-in-soc)
- [Normal vs Suspicious ICMP Activity](#normal-vs-suspicious-icmp-activity)
- [Common ICMP-Based Attacks](#common-icmp-based-attacks)
- [SOC Investigation Workflow](#soc-investigation-workflow)
- [Common SIEM Alerts](#common-siem-alerts)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Interview Questions](#interview-questions)
- [Key Takeaways](#key-takeaways)

---

# Overview

**Internet Control Message Protocol (ICMP)** is a network protocol used to exchange error messages, network status information, and diagnostic data between devices.

Unlike protocols such as HTTP, DNS, or SSH, ICMP is **not used to transfer application data**. Instead, it helps devices determine whether network communication is working correctly.

ICMP is commonly used by tools such as **Ping** and **Traceroute** to troubleshoot network connectivity.

---

# Why ICMP Matters

Every enterprise network experiences connectivity issues from time to time.

ICMP helps administrators quickly determine:

- Whether a host is reachable
- Whether a network path is available
- Where connectivity problems occur
- Whether packets are being dropped
- Network latency and response time

Without ICMP, troubleshooting network problems would be significantly more difficult. :contentReference[oaicite:0]{index=0}

---

# How ICMP Works

```text
Administrator
      │
Ping Request
      │
      ▼
Target Server
      │
Echo Reply
      │
      ▼
Administrator
Host is Reachable
```

Typical workflow:

1. A device sends an ICMP Echo Request.
2. The destination system receives the request.
3. If reachable, it returns an ICMP Echo Reply.
4. The sender confirms network connectivity.

This process forms the basis of the **Ping** utility. :contentReference[oaicite:1]{index=1}

---

# Ports and Protocols

| Protocol | Port | Purpose |
|----------|------|---------|
| ICMP | None | Network diagnostics and error reporting |

### Important Note

ICMP **does not use TCP or UDP ports**.

Instead, it uses **Message Types** and **Codes** to identify different network events and error conditions. :contentReference[oaicite:2]{index=2}

---

# Common ICMP Message Types

| Message Type | Purpose |
|-------------|---------|
| Echo Request | Tests whether a host is reachable (Ping) |
| Echo Reply | Response to an Echo Request |
| Destination Unreachable | Indicates that a destination cannot be reached |
| Time Exceeded | Generated when a packet's TTL expires (Traceroute) |
| Redirect | Informs a host of a better routing path |

These messages help diagnose network connectivity and routing issues.

---

# Common ICMP Tools

### Ping

Ping sends **ICMP Echo Requests** and waits for **Echo Replies** to verify whether a remote system is reachable.

Example:

```text
Laptop
   │
Ping
   ▼
Server
   │
Echo Reply
   ▼
Connection Successful
```

---

### Traceroute

Traceroute uses ICMP messages to identify the path packets take to reach a destination.

It helps administrators determine where network delays or failures occur.

---

# ICMP in Enterprise Environments

ICMP is widely used across enterprise networks for:

- Network troubleshooting
- Server availability monitoring
- Connectivity testing
- Route verification
- Network monitoring platforms
- Performance diagnostics
- Infrastructure health checks

Many monitoring systems rely on ICMP to determine whether critical servers and network devices are online.

---

# Why ICMP Matters in SOC

Although ICMP is designed for diagnostics, attackers frequently abuse it during cyber attacks.

SOC analysts monitor ICMP activity to detect:

- Host discovery
- Network reconnaissance
- Ping sweeps
- ICMP tunneling
- Denial-of-Service attacks
- Malware command-and-control communication

Because many organizations allow ICMP traffic through firewalls, attackers may use it to avoid simple filtering mechanisms. :contentReference[oaicite:3]{index=3}

---

# Normal vs Suspicious ICMP Activity

| Normal Activity | Suspicious Activity |
|-----------------|---------------------|
| Administrator pings a server for troubleshooting | One system pings hundreds of IP addresses |
| Monitoring platform checks server availability | Continuous ICMP traffic to an unknown external IP |
| Occasional Ping or Traceroute | Extremely high ICMP traffic volume |
| Short diagnostic sessions | ICMP packets with unusually large payloads |
| Authorized infrastructure monitoring | Regular ICMP communication from an infected endpoint |

Understanding normal ICMP behavior helps SOC analysts distinguish legitimate diagnostics from reconnaissance and malware activity. :contentReference[oaicite:4]{index=4}

---

# Common ICMP-Based Attacks

## Ping Sweep

Attackers send ICMP Echo Requests to many IP addresses to identify live hosts before launching additional attacks.

### Indicators

- One host scanning many IP addresses
- Sequential ICMP Echo Requests
- Large number of reachable hosts identified

---

## ICMP Flood

Attackers overwhelm a target with excessive ICMP traffic, consuming bandwidth and system resources.

### Indicators

- Large volume of ICMP packets
- Increased bandwidth usage
- Network performance degradation

---

## Smurf Attack

Attackers send spoofed ICMP requests to a broadcast address, causing multiple systems to reply simultaneously to the victim.

This results in traffic amplification and potential denial-of-service.

---

## ICMP Tunneling

Attackers encapsulate data within ICMP packets to bypass network security controls.

### Indicators

- Large ICMP payloads
- Regular communication intervals
- Unexpected external destinations
- Long-lived ICMP sessions

---

# SOC Investigation Workflow

When investigating a suspicious ICMP alert, verify:

- Source device
- Destination IP address
- Number of destination systems contacted
- Internal or external communication
- Frequency of ICMP packets
- Packet size
- Signs of ICMP tunneling
- Evidence of Ping Sweeps
- Related firewall logs
- Endpoint activity
- Authentication logs
- EDR telemetry

Correlate ICMP activity with subsequent events such as port scanning, remote logins, or malware execution to determine whether it is part of a larger attack. :contentReference[oaicite:5]{index=5}

---

# Common SIEM Alerts

Examples include:

- ICMP Ping Sweep
- Excessive ICMP Traffic
- Possible ICMP Tunnel
- ICMP Flood Detected
- Network Reconnaissance
- Unusual ICMP Communication

---

# MITRE ATT&CK Mapping

| Tactic | ICMP Usage |
|---------|------------|
| Discovery | Ping Sweep, Host Discovery |
| Command and Control | ICMP Tunneling |
| Impact | ICMP Flood (DoS) |

---

# Interview Questions

### What is ICMP?

ICMP (Internet Control Message Protocol) is used to exchange network status, error, and diagnostic messages. It is commonly used by tools such as Ping and Traceroute and does not use TCP or UDP ports. :contentReference[oaicite:6]{index=6}

---

### Does ICMP use port numbers?

No. ICMP does not use TCP or UDP ports. Instead, it uses message types and codes to exchange diagnostic and error information. :contentReference[oaicite:7]{index=7}

---

### What is the purpose of Ping?

Ping uses ICMP Echo Request and Echo Reply messages to verify whether a remote host is reachable and to measure basic network connectivity. :contentReference[oaicite:8]{index=8}

---

### What is Traceroute?

Traceroute is a network diagnostic tool that identifies the path packets take to reach a destination, helping locate where connectivity issues occur. :contentReference[oaicite:9]{index=9}

---

### Name common ICMP attacks.

- Ping Sweep
- ICMP Flood
- Smurf Attack
- ICMP Tunneling :contentReference[oaicite:10]{index=10}

---

### How would you investigate a suspicious ICMP alert?

A typical investigation includes:

- Identifying the source and destination systems
- Reviewing the volume of ICMP traffic
- Determining whether multiple hosts were scanned
- Looking for ICMP tunneling indicators
- Correlating firewall, endpoint, and authentication logs
- Identifying any follow-on activity such as port scanning or exploitation :contentReference[oaicite:11]{index=11}

---

# Key Takeaways

| Topic | Summary |
|--------|---------|
| Purpose | Network diagnostics and error reporting |
| Default Port | None (Does not use TCP/UDP ports) |
| Common Tools | Ping, Traceroute |
| Enterprise Usage | Connectivity testing, monitoring, troubleshooting |
| SOC Relevance | Critical for detecting reconnaissance, tunneling, and DoS activity |
| Common Attacks | Ping Sweep, ICMP Flood, Smurf Attack, ICMP Tunneling |

---
