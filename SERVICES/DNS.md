# Domain Name System (DNS)

> A practical guide to understanding Domain Name System (DNS) from a Security Operations Center (SOC) Analyst's perspective.
---

## Table of Contents

- [Overview](#overview)
- [Why Do We Need DNS?](#why-do-we-need-dns)
  - [Why Organizations Depend on DNS](#why-organizations-depend-on-dns)
- [Where Is DNS Used?](#where-is-dns-used)
- [How DNS Works](#how-dns-works)
- [DNS Resolution Flow](#dns-resolution-flow)
  - [Step-by-Step DNS Resolution](#step-by-step-dns-resolution)
  - [Why Should a SOC Analyst Understand DNS Resolution?](#why-should-a-soc-analyst-understand-dns-resolution)
- [DNS Records](#dns-records)
  - [Why Are TXT Records Important?](#why-are-txt-records-important)
- [Ports and Protocols](#ports-and-protocols)
  - [Why Does DNS Mostly Use UDP?](#why-does-dns-mostly-use-udp)
  - [When Does DNS Use TCP?](#when-does-dns-use-tcp)
- [Why DNS Matters for SOC Analysts](#why-dns-matters-for-soc-analysts)
  - [What Can DNS Logs Reveal?](#what-can-dns-logs-reveal)
  - [DNS Log Correlation](#dns-log-correlation)
- [Common DNS Attacks](#common-dns-attacks)
  - [1. DNS Tunneling](#1-dns-tunneling)
  - [2. DNS Spoofing](#2-dns-spoofing)
  - [3. DNS Cache Poisoning](#3-dns-cache-poisoning)
  - [4. Domain Generation Algorithm (DGA)](#4-domain-generation-algorithm-dga)
  - [5. Fast Flux](#5-fast-flux)
- [How Attackers Abuse DNS](#how-attackers-abuse-dns)
- [SOC Investigation Workflow](#soc-investigation-workflow)
  - [Step 1 – Identify the Source Endpoint](#step-1--identify-the-source-endpoint)
  - [Step 2 – Identify the Logged-in User](#step-2--identify-the-logged-in-user)
  - [Step 3 – Identify the Parent Process](#step-3--identify-the-parent-process)
  - [Step 4 – Analyze the Queried Domain](#step-4--analyze-the-queried-domain)
  - [Step 5 – Check Domain Reputation](#step-5--check-domain-reputation)
  - [Step 6 – Determine What Happened After DNS Resolution](#step-6--determine-what-happened-after-dns-resolution)
  - [Step 7 – Check Whether Other Endpoints Queried the Same Domain](#step-7--check-whether-other-endpoints-queried-the-same-domain)
  - [Step 8 – Correlate with Other Security Logs](#step-8--correlate-with-other-security-logs)
- [Common SIEM Alerts](#common-siem-alerts)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Key Takeaways](#key-takeaways)
- [Skills Demonstrated](#skills-demonstrated)
- [Conclusion](#conclusion)

---

## Overview

The **Domain Name System (DNS)** is a service that converts human-readable domain names (such as `google.com`) into IP addresses (such as `142.250.x.x`) so computers can communicate over a network.

Think of DNS as the **phonebook of the Internet**.

Humans prefer to remember names:
- google.com
- github.com
- microsoft.com

Computers, however, communicate using numerical IP addresses.

Instead of asking users to remember the IP address of every website or application they want to access, DNS automatically performs the translation in the background.

Without DNS, users would need to remember IP addresses for every service they use, making the Internet much harder to navigate.

> **Interview Tip**
>
> "DNS is a service that translates human-readable domain names into IP addresses so computers can communicate over a network. It acts like the phonebook of the Internet because humans remember names while computers communicate using IP addresses."

---

## Why Do We Need DNS?

Imagine there is no DNS.

To open Google, instead of typing:
```text
google.com
```
you would have to remember:
```text
142.250.xxx.xxx
```
Now imagine remembering the IP addresses for:
- Gmail
- YouTube
- LinkedIn
- Amazon
- Microsoft
- GitHub
- Banking websites
- Every company application you use

Clearly, this isn't practical.

DNS solves this problem by allowing users to work with easy-to-remember names while computers continue communicating using IP addresses.

A simple analogy is your phone contacts. Instead of remembering:
```text
+91 XXXXXXXXXX
```
you simply save:
```text
Mom ❤️
```
Your phone automatically converts the contact name into the correct phone number. DNS works in a very similar way—it converts a domain name into an IP address behind the scenes.

### Why Organizations Depend on DNS

Modern organizations rely heavily on DNS because almost every business application uses domain names.

Imagine a company with thousands of employees. Every day, employees access services such as:
- Microsoft Outlook
- Microsoft Teams
- VPN
- HR Portal
- Internal Websites
- Active Directory
- File Servers
- Cloud Applications

Instead of remembering the IP address of every server, employees simply access resources using names like:
```text
mail.company.com
vpn.company.com
hr.company.com
```
DNS automatically resolves these names to the correct IP addresses.

#### Another Major Advantage

Suppose the HR application is moved to a new server.

Old IP: `10.10.20.15`
New IP: `10.10.50.25`

Users continue accessing: `hr.company.com`

The only thing that changes is the DNS record. No user needs to remember a new IP address or update their bookmarks. This flexibility is one of the primary reasons DNS is essential in enterprise environments.

---

## Where Is DNS Used?

One of the easiest ways to understand DNS is to realize that it works almost everywhere.

Whenever you:
- Open a website
- Send an email
- Connect to a VPN
- Sign in to Microsoft Teams
- Access Active Directory resources
- Download Windows Updates
- Update antivirus signatures
- Connect to cloud services

DNS is working quietly in the background.

A simple rule to remember is:
> **If a device communicates using a domain name instead of an IP address, DNS is involved.**

---

## How DNS Works

Whenever you type a website address into your browser, your computer cannot immediately connect to that website.

First, it must discover the IP address associated with the domain name. DNS performs this lookup automatically. The entire process usually takes only a few milliseconds.

Although it happens very quickly, several steps occur before the webpage is displayed.

---

## DNS Resolution Flow
```
User
   │
   ▼
Browser
   │
   ▼
Operating System
   │
   ▼
Local DNS Cache
   │
(Cache Miss)
   │
   ▼
Configured DNS Resolver
   │
   ▼
Authoritative DNS Server
   │
Returns IP Address
   │
   ▼
Browser Connects to Web Server
   │
   ▼
Website Loads
```

### Step-by-Step DNS Resolution

#### Step 1 — User Requests a Website
The user enters a domain name into the browser.
Example: `www.google.com`
At this point, the browser knows the website name but does not know its IP address.

#### Step 2 — Browser Checks Local Cache
The browser first checks whether it has recently visited the same website. If the IP address already exists in the local DNS cache, the browser immediately uses it. No DNS request is sent. If the record is not available, the operating system continues the lookup process.

#### Step 3 — Operating System Sends a DNS Query
The operating system sends a DNS request to the configured DNS server. This is usually:
- Company DNS Server
- ISP DNS Server
- Google DNS (8.8.8.8)
- Cloudflare DNS (1.1.1.1)

The DNS server searches for the requested domain.

#### Step 4 — DNS Server Returns the IP Address
Once the DNS server finds the requested record, it returns the corresponding IP address.
Example:
`google.com → 142.250.xxx.xxx`
Now the browser knows where Google's server is located.

#### Step 5 — Browser Establishes the Connection
Using the returned IP address, the browser establishes a connection with the destination server. Depending on the website, this may involve:
- TCP Handshake
- TLS Handshake (HTTPS)
- HTTP Request

The webpage is then downloaded and displayed to the user.

### Why Should a SOC Analyst Understand DNS Resolution?

Understanding the DNS resolution process helps analysts investigate security incidents more effectively. Almost every outbound network connection begins with a DNS lookup.

For example:
```
Malware
   ↓
DNS Query
   ↓
IP Address Returned
   ↓
HTTPS Connection
   ↓
Malicious Payload Download
```
If the DNS request never succeeds, the malware may never reach its Command and Control (C2) server. This is one of the reasons DNS logs are considered a valuable source of security telemetry.

---

## DNS Records

A DNS record is an entry stored on a DNS server that tells clients how a particular domain should be handled. Think of DNS records as instructions.

Each record has a different responsibility. Some records point a domain to an IP address, while others identify mail servers or store email authentication information.

As a SOC Analyst, you do not need to memorize every DNS record. However, the following record types appear frequently during investigations and are worth understanding.

| Record  | Purpose                                   | Example                             |
| :------ | :---------------------------------------- | :---------------------------------- |
| **A**   | Maps a domain name to an IPv4 address     | `google.com → 142.250.x.x`          |
| **AAAA**| Maps a domain name to an IPv6 address     | IPv6 environments                   |
| **MX**  | Specifies which mail server receives email| `company.com → mail.company.com`    |
| **CNAME**| Creates an alias for another domain      | `www.company.com → company.com`     |
| **TXT** | Stores text information                   | SPF, DKIM, DMARC                    |
| **PTR** | Performs reverse lookups (IP → Hostname)  | Used during investigations          |

### Why Are TXT Records Important?

TXT records are commonly used to publish email authentication policies such as:
- SPF
- DKIM
- DMARC

These technologies help reduce email spoofing and phishing attacks by allowing mail servers to verify whether an email originates from an authorized source. As a SOC Analyst, you will often encounter TXT records while investigating phishing campaigns and email security incidents.

---

## Ports and Protocols

DNS primarily operates on **Port 53**.

| Port | Protocol | Purpose                                     |
| :--- | :------- | :------------------------------------------ |
| **53** | UDP      | Standard DNS queries                        |
| **53** | TCP      | Zone Transfers, DNSSEC, and large responses |

### Why Does DNS Mostly Use UDP?

Most DNS requests are very small and require a quick response. UDP is preferred because:
- No connection establishment is required.
- Lower protocol overhead.
- Faster communication.
- Suitable for small DNS queries.

For normal name resolution, speed is generally more important than reliability. Therefore, DNS uses **UDP Port 53** by default.

### When Does DNS Use TCP?

Although UDP is the default protocol, DNS switches to TCP whenever reliable communication is required. Common situations include:
- Zone Transfers (AXFR)
- DNSSEC
- Large DNS responses that exceed the UDP packet size limit.

---

## Why DNS Matters for SOC Analysts

DNS is one of the most valuable sources of network telemetry during security monitoring and incident response.

Almost every connection to an external service begins with a DNS lookup. Before a user opens a website, before malware contacts its Command and Control (C2) server, and before an application communicates with a cloud service, a DNS query is usually generated.

Because of this, DNS often provides one of the earliest indicators of malicious activity.

For example, when malware wants to communicate with its C2 server, it usually doesn't know the server's IP address in advance. Instead, it first resolves the attacker's domain using DNS.
```
Malware Execution
        │
        ▼
DNS Query
        │
        ▼
Attacker Domain Resolved
        │
        ▼
Connection Established
        │
        ▼
Malicious Activity Begins
```
If security teams monitor DNS traffic, they may detect malicious activity before the attacker successfully communicates with their infrastructure. For this reason, DNS logs are considered one of the most valuable data sources in a Security Operations Center (SOC).

### What Can DNS Logs Reveal?

DNS logs provide valuable insights into how systems communicate across the network. They can help analysts identify:
- Malware communication
- Command and Control (C2) traffic
- Phishing domains
- Newly Registered Domains (NRDs)
- DNS Tunneling
- Domain Generation Algorithm (DGA) activity
- Fast Flux infrastructure
- Suspicious outbound connections
- Data exfiltration attempts

A single DNS query may appear harmless, but when correlated with endpoint and network telemetry, it often becomes the first clue in identifying an attack.

### DNS Log Correlation

DNS logs should never be investigated in isolation. Always correlate DNS activity with other security logs to understand the complete attack chain.

Common log sources include:

| Log Source         | Why It Matters                                |
| :----------------- | :-------------------------------------------- |
| DNS Logs           | Shows which domains were queried              |
| Firewall Logs      | Shows outbound network connections            |
| Proxy Logs         | Identifies web browsing activity              |
| EDR Telemetry      | Shows the process responsible for the DNS query |
| Windows Event Logs | Provides user logon and system activity       |
| Email Security Logs| Helps investigate phishing campaigns        |
| Threat Intelligence| Identifies known malicious domains            |

Correlating multiple log sources provides context that a single log source cannot.

---

## Common DNS Attacks

Attackers frequently abuse DNS because it is trusted by almost every organization. Blocking DNS completely is not practical because nearly every application depends on it. As a result, attackers often use DNS to hide their activities or communicate with external infrastructure.

Below are some of the most common DNS-related attacks that SOC Analysts should understand.

### 1. DNS Tunneling

#### What is DNS Tunneling?
DNS Tunneling is a technique where attackers hide data inside DNS requests and responses instead of using traditional protocols such as HTTP or HTTPS. Attackers abuse this trusted protocol by embedding data inside DNS queries, effectively creating a covert communication channel. Because DNS traffic is commonly allowed through firewalls, this technique can bypass network restrictions if DNS traffic is not properly monitored.

#### Why Do Attackers Use DNS Tunneling?
- Data Exfiltration
- Command and Control (C2)
- Firewall Bypass
- Covert Communication

#### Example
Instead of sending stolen data over HTTPS, malware may send requests like:
```text
dGhpcy1pcy1zZWNyZXQtZGF0YQ.attacker.com
```
The encoded string contains stolen information hidden inside the DNS request. To the network, it appears to be a normal DNS lookup.

#### SOC Indicators
- Extremely long subdomains
- Base64 or encoded strings in subdomains
- A large number of DNS requests from a single host
- Excessive TXT record queries
- High DNS traffic to unknown domains

### 2. DNS Spoofing

#### What is DNS Spoofing?
DNS Spoofing occurs when an attacker sends a fake DNS response before the legitimate DNS server replies. As a result, the victim receives a malicious IP address instead of the correct one.
```
User Types bank.com
        │
Fake DNS Response
        │
Attacker's IP Address
        │
Victim Opens Fake Website
```
The user believes they are visiting the legitimate website, while in reality they have been redirected to an attacker-controlled server.

#### Why Do Attackers Use DNS Spoofing?
- Credential theft
- Phishing
- Malware delivery
- User redirection

#### SOC Indicators
- Users redirected to unexpected websites
- Certificate warnings in browsers
- DNS responses from unexpected IP addresses
- Multiple users reporting incorrect website behavior

### 3. DNS Cache Poisoning

#### What is DNS Cache Poisoning?
DNS Cache Poisoning occurs when an attacker successfully inserts a fake DNS record into a DNS resolver's cache. Instead of attacking individual users, the attacker compromises the DNS cache itself. As a result, every user who queries that domain receives the malicious IP address until the cache expires.

#### Example
```
bank.com
   ↓
Fake DNS Record Stored
   ↓
DNS Cache
   ↓
All Users Receive Malicious IP
```

#### Impact
A successful cache poisoning attack can affect hundreds or even thousands of users simultaneously.

#### SOC Indicators
- Multiple users redirected to the same malicious IP
- Unexpected DNS responses from internal resolvers
- A sudden increase in phishing reports for a specific domain
- Inconsistent DNS resolutions across different resolvers

### 4. Domain Generation Algorithm (DGA)

#### What is a DGA?
Some malware families do not use a fixed Command and Control (C2) domain. Instead, they generate hundreds or thousands of random domain names every day until one successfully connects to the attacker's infrastructure.
Example:
```text
xhdyw82.com
kshf72.net
qowie92.org
nznqk11.xyz
```
Eventually, one of these domains is registered by the attacker. The malware connects to that domain and establishes communication.

#### Why Do Attackers Use DGA?
Because blocking one malicious domain is easy. Generating thousands of new domains every day makes blocking significantly more difficult.

#### SOC Indicators
- High volume of NXDOMAIN (Non-Existent Domain) responses
- Random-looking, algorithmically generated domain names
- Hundreds of failed DNS lookups from a single host
- Similar query patterns across multiple hosts

### 5. Fast Flux

#### What is Fast Flux?
Fast Flux is a technique where attackers rapidly change the IP addresses associated with a malicious domain.
For example:
```
08:00 AM: evil.com → 185.x.x.x
10:00 AM: evil.com → 92.x.x.x
12:00 PM: evil.com → 34.x.x.x
```
By constantly changing IP addresses, attackers make it difficult for defenders to block their infrastructure.

#### Why Do Attackers Use Fast Flux?
- Increase infrastructure resilience
- Evade IP-based blocking
- Make takedown efforts more difficult

#### SOC Indicators
- Frequent IP address changes for a single domain
- Very low Time-to-Live (TTL) values in DNS records
- Multiple A records for a single domain
- A large number of geographically distributed IP addresses for a domain

---

## How Attackers Abuse DNS

DNS is not malicious by itself. However, attackers frequently abuse it because it is trusted and almost always allowed through firewalls.

Common attacker activities include:
- Resolving Command and Control (C2) domains
- Downloading malware
- Phishing campaigns
- DNS Tunneling
- Data exfiltration
- Malware updates
- Beaconing
- Infrastructure discovery

A useful rule to remember is:
> **If an attacker needs to communicate with an external server, DNS is often one of the first protocols involved.**

---

## SOC Investigation Workflow

Receiving a **Suspicious DNS Query** alert does not automatically mean the activity is malicious. As a SOC Analyst, your goal is to gather evidence, validate the activity, and determine whether it represents a genuine threat or normal business behavior.

```
SIEM Alert
      │
      ▼
Identify Source Host
      │
      ▼
Identify Logged-in User
      │
      ▼
Identify Parent Process
      │
      ▼
Analyze Queried Domain
      │
      ▼
Check Domain Reputation
      │
      ▼
Correlate with Other Logs
      │
      ▼
Determine Impact
      │
      ▼
Escalate or Close Alert
```

### Step 1 – Identify the Source Endpoint

Start by identifying which endpoint generated the DNS request. Gather the following information:
- Hostname & Source IP Address
- Asset Name & Criticality
- Operating System
- Department or Business Unit

Understanding the affected asset provides context. An alert from a Domain Controller is far more critical than one from a test machine.

### Step 2 – Identify the Logged-in User

Determine who was using the system when the DNS request occurred.
- Who was logged in?
- Is the account a standard user or administrator?
- Has this user generated similar alerts before?

Understanding the user helps determine whether the activity aligns with their normal behavior.

### Step 3 – Identify the Parent Process

Identifying which process generated the DNS request is a critical step. A lookup from a browser is expected; a lookup from PowerShell deserves closer attention.

**Common Legitimate Processes:**
- `chrome.exe`, `msedge.exe`, `firefox.exe`
- `outlook.exe`, `teams.exe`

**Processes That Require Investigation:**
- `powershell.exe`, `cmd.exe`
- `wscript.exe`, `cscript.exe`
- `mshta.exe`, `rundll32.exe`, `regsvr32.exe`, `certutil.exe`

> **SOC Tip**
>
> A suspicious process does not automatically indicate malicious activity, but it should always be investigated further.

### Step 4 – Analyze the Queried Domain

Examine the domain that was requested. Attackers often register domains that closely resemble legitimate services (**Typosquatting**).

- **Legitimate:** `microsoft.com`, `google.com`
- **Potentially Suspicious:** `micros0ft-login.com`, `secure-google-login.net`

### Step 5 – Check Domain Reputation

Use threat intelligence platforms to determine if the domain is known to be malicious.
- **Resources:** VirusTotal, Cisco Talos, URLhaus, AbuseIPDB, AlienVault OTX
- **Check for:** Malware associations, phishing reports, registration date (WHOIS), and previous malicious activity.

Domains registered only a few hours or days before the alert should be treated with additional scrutiny.

### Step 6 – Determine What Happened After DNS Resolution

Receiving an IP address doesn't mean an attack occurred. Determine whether the endpoint actually connected to the resolved IP.

Example attack chain:
```
DNS Query
      ▼
IP Address Returned
      ▼
HTTPS Connection
      ▼
Executable Download
      ▼
PowerShell Execution
      ▼
Persistence Created
```

### Step 7 – Check Whether Other Endpoints Queried the Same Domain

Determine if the activity is isolated or widespread.
- Did multiple hosts query the domain?
- Are multiple users affected?
- Did the queries occur around the same time?

If several systems contact the same suspicious domain, the incident may involve phishing, malware propagation, or C2 communication.

### Step 8 – Correlate with Other Security Logs

Never investigate DNS activity in isolation. Correlate the event with other available telemetry.

| Log Source         | Purpose                    |
| :----------------- | :------------------------- |
| DNS Logs           | Domain resolution history  |
| Firewall Logs      | Outbound connections       |
| Proxy Logs         | Web traffic                |
| EDR                | Process execution          |
| Windows Event Logs | User activity              |
| Email Gateway      | Phishing investigations    |

---

## Common SIEM Alerts

| Alert                          | What It Indicates                                 |
| :----------------------------- | :------------------------------------------------ |
| **Suspicious DNS Query**         | A domain requiring further investigation        |
| **DNS Tunneling Detected**       | Possible covert communication or data exfiltration |
| **Excessive DNS Requests**       | Beaconing, malware, or tunneling activity         |
| **High NXDOMAIN Responses**      | Possible Domain Generation Algorithm (DGA) activity |
| **Newly Registered Domain (NRD)**| Potential phishing or malware infrastructure    |
| **Fast Flux Domain**             | Rapidly changing attacker infrastructure          |
| **Suspicious TXT Record Activity**| Potential DNS tunneling or malware communication  |
| **DNS Beaconing**                | Periodic communication with external infrastructure |

---

## MITRE ATT&CK Mapping

DNS activity appears throughout multiple stages of an attack.

| MITRE Tactic         | DNS Usage                                  |
| :------------------- | :----------------------------------------- |
| **Reconnaissance**     | Domain discovery and enumeration           |
| **Resource Development**| Registration of attacker-controlled domains|
| **Initial Access**     | Resolving phishing infrastructure          |
| **Command and Control**| Malware communication with C2 servers      |
| **Discovery**          | Internal hostname lookups                  |
| **Exfiltration**       | DNS tunneling for data theft               |

---


---

## Key Takeaways

| Topic                 | Summary                                                      |
| :-------------------- | :----------------------------------------------------------- |
| **Purpose of DNS**      | Resolves domain names into IP addresses                      |
| **Default Port**        | 53                                                           |
| **Primary Protocol**    | UDP                                                          |
| **TCP Usage**           | Zone transfers, DNSSEC, and large responses                  |
| **Common DNS Records**  | A, AAAA, MX, CNAME, TXT, PTR                                 |
| **SOC Importance**      | One of the earliest indicators of network activity           |
| **Common Threats**      | DNS Tunneling, DGA, Fast Flux, Cache Poisoning, Spoofing     |
| **Investigation Focus** | Host, User, Process, Domain, Reputation, Network Correlation |

---

## Skills Demonstrated

By completing this topic, you should be able to:
- Explain DNS confidently during interviews.
- Understand why DNS is critical in enterprise environments.
- Describe how DNS resolution works.
- Recognize common DNS record types.
- Understand why DNS primarily uses UDP.
- Identify common DNS-based attacks.
- Investigate DNS-related SIEM alerts.
- Correlate DNS events with endpoint and network telemetry.
- Map DNS activity to the MITRE ATT&CK framework.
- Apply a structured investigation methodology during incident response.

---

## Conclusion

DNS is much more than a service that translates domain names into IP addresses. From a SOC perspective, DNS is one of the earliest and most valuable indicators of network activity. Whether investigating malware, phishing, or C2 communication, understanding DNS allows analysts to detect suspicious behavior, correlate events, and make informed decisions during incident response.

Mastering DNS fundamentals is an essential step toward becoming an effective SOC Analyst because almost every modern cyberattack interacts with DNS at some stage of the attack lifecycle.

---
