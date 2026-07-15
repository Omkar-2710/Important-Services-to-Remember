# Domain Name System (DNS)

> A practical SOC Analyst reference covering DNS fundamentals, security implications, common attack techniques, and investigation workflows.

![Status](https://img.shields.io/badge/Category-Network%20Service-blue)
![Focus](https://img.shields.io/badge/Focus-SOC%20Operations-success)
![Level](https://img.shields.io/badge/Level-Beginner%20to%20Intermediate-orange)

---

# Table of Contents

- [Overview](#overview)
- [Quick Facts](#quick-facts)
- [DNS Resolution Flow](#dns-resolution-flow)
- [DNS Records](#dns-records)
- [Enterprise Use Cases](#enterprise-use-cases)
- [Why DNS Matters for SOC Analysts](#why-dns-matters-for-soc-analysts)
- [Threat Landscape](#threat-landscape)
- [Detection Opportunities](#detection-opportunities)
- [SOC Investigation Workflow](#soc-investigation-workflow)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Key Takeaways](#key-takeaways)

---

# Overview

The **Domain Name System (DNS)** is a distributed naming service that resolves domain names into IP addresses, allowing systems to locate and communicate with resources across a network.

Because nearly every application relies on DNS before establishing communication, DNS logs are one of the most valuable telemetry sources during threat detection and incident investigations.

---

# Quick Facts

| Item | Value |
|------|-------|
| Service | Domain Name System (DNS) |
| OSI Layer | Application Layer (Layer 7) |
| Default Port | 53 |
| Protocol | UDP / TCP |
| Primary Purpose | Name Resolution |
| Enterprise Criticality | High |
| SOC Visibility | High |

---

# DNS Resolution Flow

```text
User
   │
   ▼
Browser
   │
   ▼
Operating System Resolver
   │
   ▼
Local DNS Cache
   │
(Cache Miss)
   │
   ▼
Recursive DNS Resolver
   │
   ▼
Authoritative DNS Server
   │
Returns IP Address
   │
   ▼
Browser Connects to Destination
```

### Resolution Process

1. User requests a domain.
2. Browser checks local cache.
3. OS queries the configured DNS resolver.
4. Recursive resolver contacts the authoritative DNS server if necessary.
5. IP address is returned.
6. Client establishes the network connection.

---

# DNS Records

| Record | Description | Common Usage |
|---------|-------------|--------------|
| A | Maps hostname to IPv4 address | Website resolution |
| AAAA | Maps hostname to IPv6 address | IPv6 environments |
| MX | Specifies mail server | Email routing |
| CNAME | Alias for another hostname | Service aliases |
| TXT | Stores text-based metadata | SPF, DKIM, DMARC |
| PTR | Reverse DNS lookup | Investigation and logging |

---

# Ports and Protocols

| Port | Protocol | Usage |
|------|----------|------|
| 53 | UDP | Standard DNS queries |
| 53 | TCP | Zone transfers, DNSSEC, large responses |

### UDP

Used because:

- Lightweight
- Fast
- Low overhead
- Suitable for small queries

### TCP

Used when:

- Zone Transfers (AXFR)
- DNSSEC
- Large DNS responses
- Reliable communication is required

---

# Enterprise Use Cases

DNS is required by almost every enterprise service.

Examples include:

- Active Directory
- Microsoft 365
- VPN Services
- Internal Web Applications
- Email Infrastructure
- Cloud Services
- Endpoint Protection Platforms
- Software Updates
- Identity Services
- Proxy Servers

Without DNS, most enterprise applications cannot function correctly.

---

# Why DNS Matters for SOC Analysts

DNS activity often appears before any outbound connection.

Monitoring DNS helps analysts identify:

- Malware communication
- Command and Control (C2)
- DNS Tunneling
- Phishing infrastructure
- Newly Registered Domains (NRDs)
- Domain Generation Algorithms (DGA)
- Fast Flux infrastructure
- Data exfiltration

DNS should always be correlated with:

- Firewall Logs
- Proxy Logs
- EDR Telemetry
- Web Traffic
- Threat Intelligence
- Email Security Logs

---

# Threat Landscape

| Technique | Objective |
|-----------|-----------|
| DNS Tunneling | Data Exfiltration |
| DNS Spoofing | Redirect victims |
| Cache Poisoning | Manipulate DNS responses |
| DGA | Dynamic C2 communication |
| Fast Flux | Evade blocking |
| DNS Beaconing | Persistent malware communication |

---

# Detection Opportunities

## DNS Tunneling

Indicators:

- Extremely long subdomains
- Base64-like strings
- High query volume
- TXT record abuse
- Consistent outbound DNS traffic

---

## Domain Generation Algorithm (DGA)

Indicators:

- Random domain names
- High NXDOMAIN rate
- Large number of failed lookups
- Repeated DNS retries

---

## Fast Flux

Indicators:

- Rapid IP changes
- Low TTL values
- Multiple A records
- Frequent DNS updates

---

## Suspicious Domains

Look for:

- Typosquatting
- Newly registered domains
- Rare domains
- Low reputation domains
- Uncommon TLDs

---

# SOC Investigation Workflow

When investigating a suspicious DNS alert:

### Endpoint Validation

- Source IP
- Hostname
- Logged-in user
- Asset criticality

---

### Process Analysis

Identify the parent process.

Common legitimate processes:

- chrome.exe
- msedge.exe
- firefox.exe
- outlook.exe

Potentially suspicious:

- powershell.exe
- cmd.exe
- wscript.exe
- mshta.exe
- rundll32.exe
- regsvr32.exe

---

### Domain Analysis

Review:

- Domain reputation
- WHOIS registration age
- Passive DNS history
- Threat intelligence matches
- Category
- Geolocation

---

### Network Correlation

Determine whether:

- HTTPS connection followed DNS
- File download occurred
- PowerShell executed
- Additional domains were contacted
- Multiple hosts resolved the same domain

---

### Log Sources

Correlate findings with:

- DNS Logs
- Firewall Logs
- Proxy Logs
- EDR
- Windows Event Logs
- Email Gateway
- SIEM Timeline

---

# MITRE ATT&CK Mapping

| Tactic | DNS Usage |
|---------|-----------|
| Reconnaissance | Domain discovery |
| Initial Access | Phishing domains |
| Command and Control | Malware communication |
| Discovery | Internal hostname resolution |
| Exfiltration | DNS tunneling |

---

# Key Takeaways

- DNS is a critical enterprise service.
- Nearly every network connection begins with DNS.
- DNS logs provide early visibility into malicious activity.
- Always correlate DNS events with endpoint and network telemetry.
- Parent process analysis is essential during investigations.
- High NXDOMAIN rates and random domains may indicate DGA activity.
- Long encoded subdomains may indicate DNS tunneling.
- DNS should never be investigated in isolation.

---

# Skills Demonstrated

- DNS Fundamentals
- SOC Monitoring
- Incident Investigation
- Threat Hunting
- Network Security
- MITRE ATT&CK
- Blue Team Operations
- Log Correlation

---

> **Note:** This repository focuses on DNS from a Security Operations Center (SOC) perspective. It is intended as a practical reference for security monitoring, incident response, and interview preparation rather than a comprehensive networking guide.
