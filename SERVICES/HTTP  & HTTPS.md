# HyperText Transfer Protocol (HTTP) & HyperText Transfer Secure (HTTPS)

> A practical guide to understanding HTTP and HTTPS from a Security Operations Center (SOC) Analyst's perspective.

---

## Table of Contents

- [Overview](#overview)
- [Why HTTP Matters](#why-http-matters)
- [How HTTP Works](#how-http-works)
- [HTTP Request and Response](#http-request-and-response)
- [HTTP Methods](#http-methods)
- [HTTP Status Codes](#http-status-codes)
- [Ports and Protocols](#ports-and-protocols)
- [HTTP vs HTTPS](#http-vs-https)
- [HTTPS in Enterprise Environments](#https-in-enterprise-environments)
- [Why HTTP/HTTPS Matters in SOC](#why-httphttps-matters-in-soc)
- [Common Web Attacks](#common-web-attacks)
- [SOC Investigation Workflow](#soc-investigation-workflow)
- [Common SIEM Alerts](#common-siem-alerts)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Interview Questions](#interview-questions)
- [Key Takeaways](#key-takeaways)

---

# Overview

**HTTP (HyperText Transfer Protocol)** is an application layer protocol used to exchange web content between a client and a web server.

When a user opens a website, the browser sends an HTTP request and the web server responds with the requested resource such as a webpage, image, video, or file.

**HTTPS (HyperText Transfer Protocol Secure)** is the secure version of HTTP. It encrypts communication using **TLS (Transport Layer Security)**, protecting sensitive information from interception while it travels across the network.

---

# Why HTTP Matters

HTTP enables communication between web browsers and web servers.

Without HTTP, users could locate a server using DNS but would have no standard method to request webpages or other web resources.

HTTP is widely used by:

- Websites
- Web applications
- REST APIs
- Internal company portals
- Cloud applications
- Software update services
- Web dashboards

Almost every web-based service depends on HTTP or HTTPS.

---

# How HTTP Works

```text
User
   │
   ▼
Browser
   │
   ▼
DNS Resolution
   │
   ▼
Receives Server IP
   │
   ▼
TCP Connection
   │
   ▼
HTTP/HTTPS Request
   │
   ▼
Web Server
   │
   ▼
HTTP Response
   │
   ▼
Browser Displays Content
```

Typical workflow:

1. User enters a website URL.
2. DNS resolves the domain name to an IP address.
3. Browser establishes a TCP connection.
4. Browser sends an HTTP or HTTPS request.
5. Server processes the request.
6. Server returns an HTTP response.
7. Browser renders the content.

---

# HTTP Request and Response

## HTTP Request

An HTTP request is a message sent by the client requesting a resource from a web server.

Common examples include:

- Opening a webpage
- Downloading an image
- Logging into an application
- Uploading a file
- Submitting a web form
- Accessing an API

---

## HTTP Response

After processing the request, the server returns an HTTP response.

A response may contain:

- HTML pages
- Images
- PDF documents
- Videos
- JSON data
- Error messages
- Authentication results

---

# HTTP Methods

| Method | Purpose | Common Use Cases |
|---------|----------|------------------|
| GET | Retrieve information | Open webpages, images, articles |
| POST | Submit new data | Login, registration, file upload |
| PUT | Update existing data | Modify user profiles or records |
| DELETE | Remove resources | Delete users or application data |

---

# HTTP Status Codes

| Status Code | Meaning |
|-------------|---------|
| 200 OK | Request completed successfully |
| 301 | Permanent redirect |
| 302 | Temporary redirect |
| 400 | Bad request |
| 401 | Authentication required |
| 403 | Access denied |
| 404 | Resource not found |
| 500 | Internal server error |

### Difference Between 401 and 403

- **401 Unauthorized** → The client has not authenticated.
- **403 Forbidden** → The client is authenticated but does not have permission to access the requested resource.

---

# Ports and Protocols

| Service | Port | Protocol |
|----------|------|----------|
| HTTP | 80 | TCP |
| HTTPS | 443 | TCP |

### Why TCP?

HTTP relies on TCP because web content must be delivered reliably and in the correct order.

TCP provides:

- Reliable communication
- Ordered packet delivery
- Error detection
- Packet retransmission when required

---

# HTTP vs HTTPS

| HTTP | HTTPS |
|------|--------|
| Port 80 | Port 443 |
| Plaintext communication | Encrypted communication |
| No encryption | Protected using TLS |
| Data can be intercepted | Data remains confidential during transmission |
| Lower security | Higher security |

HTTPS encrypts communication but **does not guarantee that a website is legitimate**. Attackers frequently use HTTPS on phishing websites and malicious infrastructure.

---

# HTTPS in Enterprise Environments

HTTPS is the primary protocol used by modern enterprise applications, including:

- Microsoft 365
- Outlook
- Microsoft Teams
- Banking applications
- Cloud platforms
- Internal business portals
- APIs
- Software update services

Most legitimate business traffic today is encrypted using HTTPS.

---

# Why HTTP/HTTPS Matters in SOC

HTTP and HTTPS generate valuable telemetry during security investigations.

SOC analysts monitor web traffic to identify:

- Malicious URLs
- Phishing websites
- Malware downloads
- Command and Control (C2) communication
- Suspicious file downloads
- Data exfiltration
- Web shell activity
- Unusual User-Agent strings

Although HTTPS encrypts network traffic, metadata such as destination domains, certificates, connection timestamps, and process information can still provide valuable investigative evidence.

---

# Common Web Attacks

## SQL Injection (SQLi)

Injects malicious SQL statements into an application's database queries.

### Indicators

- Unexpected SQL errors
- Authentication bypass attempts
- Suspicious database queries
- Web Application Firewall (WAF) alerts

---

## Cross-Site Scripting (XSS)

Injects malicious JavaScript into web pages viewed by other users.

### Indicators

- Unexpected JavaScript execution
- Stolen session cookies
- Browser security alerts

---

## Directory Traversal

Attempts to access files outside the intended web directory.

Example:

```text
../../windows/system32
```

---

## File Upload Attack

Uploads malicious executable files to a vulnerable web application.

Common examples include:

- Web shells
- Malicious scripts
- Backdoors

---

## Web Shell

A web shell is a malicious script deployed on a compromised web server that allows attackers to execute commands remotely.

Common web shell files include:

```text
shell.php
cmd.aspx
backdoor.jsp
```

---

## Command Injection

Executes operating system commands through vulnerable web applications.

Common commands include:

```text
whoami
dir
ping
ipconfig
```

---

## Credential Stuffing

Uses stolen username and password combinations obtained from previous data breaches.

---

## Brute Force Attack

Attempts multiple password combinations until valid credentials are discovered.

---

# SOC Investigation Workflow

When investigating a suspicious HTTP or HTTPS alert, verify:

- Source endpoint
- Logged-in user
- Parent process
- Accessed URL
- Domain reputation
- Domain registration age
- Downloaded files
- File hash reputation
- TLS certificate information
- Destination IP address
- Proxy logs
- Firewall logs
- EDR telemetry
- Whether multiple users accessed the same resource

---

# Common SIEM Alerts

Examples include:

- Suspicious HTTPS Connection
- Access to Malicious URL
- Phishing Website Detected
- Suspicious File Download
- Web Shell Activity
- SQL Injection Attempt
- Cross-Site Scripting Attempt
- Excessive HTTP Requests
- HTTPS Beaconing
- Suspicious User-Agent

---

# MITRE ATT&CK Mapping

| Tactic | HTTP/HTTPS Usage |
|---------|------------------|
| Initial Access | Phishing websites |
| Execution | Malware downloads |
| Command and Control | HTTPS-based C2 communication |
| Credential Access | Fake login portals |
| Exfiltration | Uploading stolen data to attacker-controlled servers |

---

# Interview Questions

### What is HTTP?

HTTP is an application layer protocol used to exchange web content between clients and web servers. It uses TCP Port **80** and transfers data in plaintext.

---

### What is HTTPS?

HTTPS is the secure version of HTTP that encrypts communication using TLS. It protects sensitive information such as usernames, passwords, and financial data while in transit. HTTPS uses TCP Port **443**.

---

### Why does HTTP use TCP?

TCP provides reliable and ordered communication, ensuring webpages and application data are delivered correctly without packet loss.

---

### What is the difference between HTTP and HTTPS?

HTTP transfers data in plaintext over Port 80, whereas HTTPS encrypts communication using TLS over Port 443, providing confidentiality and integrity.

---

### Name common web attacks.

- SQL Injection (SQLi)
- Cross-Site Scripting (XSS)
- Directory Traversal
- File Upload Attack
- Web Shell
- Command Injection
- Credential Stuffing
- Brute Force

---

### How would you investigate a suspicious HTTPS alert?

A typical investigation includes:

- Identifying the affected endpoint
- Determining the logged-in user
- Reviewing the parent process
- Validating the accessed URL
- Checking domain reputation
- Reviewing downloaded files
- Correlating with proxy, firewall, and EDR logs
- Determining whether additional endpoints accessed the same resource

---

# Key Takeaways

| Topic | Summary |
|--------|---------|
| Purpose | Transfers web content between clients and servers |
| HTTP Port | 80/TCP |
| HTTPS Port | 443/TCP |
| Encryption | HTTPS uses TLS |
| Common Methods | GET, POST, PUT, DELETE |
| Common Status Codes | 200, 301, 302, 400, 401, 403, 404, 500 |
| SOC Relevance | Critical for web traffic monitoring and threat detection |
| Common Threats | SQL Injection, XSS, Directory Traversal, Web Shell, Credential Stuffing, Brute Force |

---
