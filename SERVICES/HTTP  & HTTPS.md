# HyperText Transfer Protocol (HTTP) & HyperText Transfer Protocol Secure (HTTPS)

> A practical guide to understanding HTTP and HTTPS from a Security Operations Center (SOC) Analyst's perspective.

![Category](https://img.shields.io/badge/Category-Web%20Protocol-blue)
![Focus](https://img.shields.io/badge/Focus-SOC%20Operations-success)
![Level](https://img.shields.io/badge/Level-Beginner%20to%20Intermediate-orange)

---

# Table of Contents

- [Overview](#overview)
- [Why Do We Need HTTP?](#why-do-we-need-http)
- [Where Is HTTP Used?](#where-is-http-used)
- [How HTTP Works](#how-http-works)
- [HTTP Request and Response](#http-request-and-response)
- [HTTP Methods](#http-methods)
- [HTTP Status Codes](#http-status-codes)
- [Ports and Protocols](#ports-and-protocols)
- [What is HTTPS?](#what-is-https)
- [HTTP vs HTTPS](#http-vs-https)

---

# Overview

Whenever you open a website, log into an application, watch a YouTube video, or shop online, your browser communicates with a web server using **HTTP** or **HTTPS**.

**HTTP (HyperText Transfer Protocol)** is an application layer protocol used to transfer web pages and other web content between a client (such as a web browser) and a web server.

HTTP uses **TCP Port 80** and sends data in **plaintext**, which means anyone who intercepts the traffic may be able to read its contents.

Because modern websites frequently exchange sensitive information such as usernames, passwords, payment details, and personal data, HTTP has largely been replaced by **HTTPS**, which encrypts communication using **TLS (Transport Layer Security)**.

> **Interview Tip**
>
> HTTP stands for **HyperText Transfer Protocol**. It is an application layer protocol used to transfer web pages and other web content between a client and a web server. It uses **TCP Port 80** and transmits data in plaintext.

---

# Why Do We Need HTTP?

Imagine you type:

```text
google.com
```

DNS successfully resolves the domain name into an IP address.

```text
google.com

↓

142.250.xxx.xxx
```

Your computer now knows **where** Google's server is located.

But another question remains:

**How does your browser ask the server to send the homepage?**

That's exactly why HTTP exists.

DNS tells your computer **where** to go.

HTTP tells the server **what** you want.

Without HTTP, your browser could locate a web server but would have no standard way to request:

- A webpage
- An image
- A video
- A PDF
- Login information
- Search results

### Think of It Like Ordering Food

Imagine you want to order food from a restaurant.

**DNS** gives you the restaurant's address.

**HTTP** is how you place your order.

Knowing the address alone doesn't get you the food—you still need to tell the restaurant what you want.

Similarly, after DNS finds the server, HTTP is responsible for requesting the desired resource.

---

# Where Is HTTP Used?

HTTP is used whenever a client requests content from a web server.

Although HTTPS is now the standard for secure communication, the underlying process still relies on HTTP.

Some common examples include:

- Websites
- Web Applications
- Company Portals
- REST APIs
- Cloud Applications
- Software Update Services
- Web Dashboards
- Internal Business Applications

A simple example looks like this:

```text
Employee

↓

Chrome Browser

↓

Company HR Portal

↓

HTTP Request

↓

Web Server

↓

HTTP Response

↓

Browser Displays Webpage
```

Every interaction with a website begins with a request from the client and a response from the server.

---

# How HTTP Works

Before a webpage appears on your screen, several steps occur behind the scenes.

The complete process looks like this:

```text
User

↓

Browser

↓

DNS Resolution

↓

IP Address Returned

↓

TCP Connection

↓

HTTP Request

↓

Web Server

↓

HTTP Response

↓

Browser Displays Website
```

Although this entire process usually completes in a fraction of a second, understanding each step is important for troubleshooting and security investigations.

---

## Step-by-Step HTTP Communication

### Step 1 – User Requests a Website

The user enters a website address.

Example:

```text
www.company.com
```

---

### Step 2 – DNS Resolves the Domain Name

The browser first performs a DNS lookup to determine the server's IP address.

Example:

```text
www.company.com

↓

192.168.10.25
```

Without DNS, the browser wouldn't know where to send the request.

---

### Step 3 – Browser Establishes a TCP Connection

Once the IP address is known, the browser establishes a TCP connection with the destination server.

For HTTP, this connection is made on:

```text
Port 80
```

For HTTPS, the connection is established on:

```text
Port 443
```

Reliable communication is important because webpages often consist of multiple files such as HTML, CSS, JavaScript, images, and videos.

---

### Step 4 – Browser Sends an HTTP Request

After the connection is established, the browser sends an HTTP request asking for a specific resource.

Example:

```http
GET /
```

This simply means:

> "Please send me the homepage."

---

### Step 5 – Web Server Processes the Request

The web server receives the request, processes it, and determines how to respond.

Depending on the request, the server may return:

- A webpage
- An image
- A PDF
- A JSON response
- An error message
- A login result

---

### Step 6 – Browser Receives the Response

The server replies with an HTTP response.

Example:

```http
HTTP/1.1 200 OK
```

The browser then renders the content for the user.

---

# HTTP Request and Response

HTTP communication always consists of two parts:

1. Request
2. Response

Without both, web communication cannot occur.

---

## What is an HTTP Request?

An HTTP request is a message sent from a client to a web server asking for a specific resource.

Every action you perform on a website begins with an HTTP request.

For example:

- Opening a webpage
- Logging into an account
- Downloading a file
- Uploading an image
- Searching for a product
- Submitting a contact form

All of these actions generate HTTP requests.

Example:

```http
GET /index.html HTTP/1.1
Host: company.com
```

---

## What is an HTTP Response?

After receiving the request, the server sends back an HTTP response.

The response may contain:

- HTML pages
- Images
- Videos
- PDF files
- JSON data
- Error messages
- Login results

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
```

The browser then displays the returned content to the user.

---

# HTTP Methods

HTTP methods define **what action the client wants the server to perform**.

Think of them as instructions sent with every request.

---

## GET

### Purpose

Retrieve information from the server.

GET requests should not modify data.

### Common Examples

- Open a homepage
- View a product
- Read an article
- Display an image

Example:

```http
GET /products
```

---

## POST

### Purpose

Send data to the server.

POST is commonly used whenever users submit information.

### Common Examples

- User Login
- Registration
- Contact Forms
- File Uploads

Example:

```http
POST /login
```

---

## PUT

### Purpose

Update existing information.

### Common Examples

- Update Profile
- Change Password
- Edit User Information

Example:

```http
PUT /users/101
```

---

## DELETE

### Purpose

Delete existing resources.

### Common Examples

- Delete User
- Remove Product
- Delete File

Example:

```http
DELETE /users/101
```

---

## Interview Tip

A simple way to remember the most common HTTP methods is:

| Method | Purpose |
|---------|---------|
| GET | Retrieve data |
| POST | Create or submit data |
| PUT | Update existing data |
| DELETE | Remove data |

---

# HTTP Status Codes

Whenever a server responds to an HTTP request, it also returns a **status code**.

The status code tells the client whether the request was successful or if an error occurred.

Understanding these codes is important because they frequently appear in web server logs, proxy logs, and SIEM alerts.

| Status Code | Meaning | Description |
|-------------|---------|-------------|
| **200 OK** | Success | The request completed successfully. |
| **301 Moved Permanently** | Redirect | The requested resource has been permanently moved to a new location. |
| **302 Found** | Temporary Redirect | The resource is temporarily available at another location. |
| **400 Bad Request** | Client Error | The server could not understand the request due to invalid syntax. |
| **401 Unauthorized** | Authentication Required | The client must authenticate before accessing the resource. |
| **403 Forbidden** | Access Denied | The client is authenticated but does not have permission to access the resource. |
| **404 Not Found** | Resource Missing | The requested page or resource could not be found. |
| **500 Internal Server Error** | Server Error | The server encountered an unexpected condition while processing the request. |

> **Interview Tip**
>
> **401 Unauthorized** means the user has **not authenticated**.
>
> **403 Forbidden** means the user is **authenticated but does not have permission** to access the requested resource.

---

# Ports and Protocols

HTTP relies on **TCP** because web content must be delivered reliably and in the correct order.

| Protocol | Port | Purpose |
|----------|------|---------|
| HTTP | 80/TCP | Standard web communication |
| HTTPS | 443/TCP | Encrypted web communication using TLS |

## Why Does HTTP Use TCP?

Unlike protocols that prioritize speed over reliability, web communication requires complete and ordered delivery.

A webpage is made up of multiple components such as:

- HTML
- CSS
- JavaScript
- Images
- Videos

If packets are lost or arrive out of order, parts of the webpage may fail to load correctly.

TCP solves this by providing:

- Reliable delivery
- Packet sequencing
- Error detection
- Retransmission of lost packets

This is why HTTP and HTTPS both rely on TCP.

---

# What is HTTPS?

HTTPS stands for **HyperText Transfer Protocol Secure**.

It is simply **HTTP protected by TLS (Transport Layer Security)**.

Instead of sending information in plaintext, HTTPS encrypts communication before it travels across the network.

Think of HTTPS as sending a sealed envelope instead of an open postcard.

Even if someone intercepts the encrypted traffic, they cannot easily read its contents without the appropriate encryption keys.

HTTPS protects sensitive information such as:

- Usernames
- Passwords
- Banking Information
- Credit Card Details
- Personal Data
- Authentication Tokens

Today, nearly every legitimate website uses HTTPS.

---

# HTTP vs HTTPS

| Feature | HTTP | HTTPS |
|---------|------|--------|
| Default Port | 80 | 443 |
| Encryption | No | Yes (TLS) |
| Security | Lower | Higher |
| Data Visibility | Plaintext | Encrypted |
| Authentication | No | Supports certificate validation |

> **Interview Tip**
>
> HTTP transfers data in plaintext over TCP Port 80, while HTTPS encrypts communication using TLS over TCP Port 443. HTTPS protects data from interception and significantly improves the confidentiality and integrity of web communication.

---
# Why HTTP & HTTPS Matter for SOC Analysts

HTTP and HTTPS are among the most monitored protocols in a Security Operations Center (SOC).

Almost every employee, application, cloud service, and attacker communicates over HTTP or HTTPS.

Whether a user is browsing a website, downloading software, signing into Microsoft 365, or accessing a cloud application, web traffic is almost always involved.

Unfortunately, attackers also rely on HTTP and HTTPS because these protocols are trusted and widely allowed through enterprise firewalls.

For this reason, web traffic is one of the richest sources of evidence during incident investigations.

---

## Why Attackers Prefer HTTP & HTTPS

Attackers rarely communicate directly over unusual protocols.

Instead, they often blend into normal web traffic.

Examples include:

- Downloading malware
- Command and Control (C2) communication
- Data exfiltration
- Phishing websites
- Fake login portals
- Malware updates
- API abuse

Because HTTPS encrypts communication, attackers often use it to make malicious traffic more difficult to inspect.

Encryption protects legitimate users—but it also protects attackers from simple packet inspection.

---

## Why Monitoring HTTP & HTTPS Is Important

Monitoring web traffic helps analysts identify:

- Malware downloads
- Phishing websites
- Command and Control communication
- Suspicious file downloads
- Data exfiltration
- Web shell activity
- SQL Injection attempts
- Cross-Site Scripting (XSS)
- Directory Traversal attacks
- Credential theft

A single HTTP request may appear harmless.

However, when correlated with endpoint telemetry, proxy logs, firewall logs, and DNS activity, it often reveals the complete attack chain.

---

## HTTP Log Correlation

HTTP logs should never be analyzed alone.

Always correlate web activity with other security telemetry.

| Log Source | Why It Matters |
|------------|----------------|
| Proxy Logs | Shows visited URLs and downloaded files |
| Firewall Logs | Shows outbound network connections |
| DNS Logs | Shows domain resolution before web communication |
| EDR | Identifies the process responsible for the connection |
| Windows Event Logs | User activity and logon events |
| Email Security | Determines whether the website originated from a phishing email |
| Threat Intelligence | Checks domain and IP reputation |

The more evidence you correlate, the more accurate your investigation becomes.

---

# Common Web Attacks

Understanding how attackers abuse web applications is one of the most important skills for a SOC Analyst.

The attacks below appear regularly in SIEM alerts, penetration testing reports, vulnerability assessments, and incident investigations.

---

# 1. SQL Injection (SQLi)

## What is SQL Injection?

SQL Injection is a web attack where an attacker inserts malicious SQL statements into an application's input fields.

Instead of submitting normal user input, the attacker sends SQL commands that are executed by the backend database.

If the application does not properly validate user input, attackers may gain unauthorized access to sensitive information.

---

## Why Do Attackers Use SQL Injection?

Attackers commonly use SQL Injection to:

- Read sensitive database information
- Modify stored data
- Delete records
- Bypass login pages
- Gain administrative access

---

## Example

Instead of entering:

```text
Username: admin
Password: password123
```

An attacker may attempt to inject SQL commands into the login form.

If the application is vulnerable, authentication controls may be bypassed.

---

## SOC Indicators

Look for:

- Repeated login failures
- Unusual database errors
- SQL keywords inside URLs
- High number of requests to login pages
- WAF alerts

---

## Investigation Tips

- Review web server logs.
- Check WAF alerts.
- Identify the source IP address.
- Determine whether authentication was bypassed.
- Review affected database activity.

---

# 2. Cross-Site Scripting (XSS)

## What is XSS?

Cross-Site Scripting (XSS) occurs when an attacker injects malicious JavaScript into a trusted website.

When another user visits the page, the browser executes the malicious script.

Unlike SQL Injection, XSS targets the user's browser instead of the backend database.

---

## Why Do Attackers Use XSS?

Common objectives include:

- Cookie theft
- Session hijacking
- Credential theft
- Browser redirection
- Displaying fake login forms

---

## SOC Indicators

Watch for:

- JavaScript embedded inside user input
- Unexpected script execution
- Suspicious browser redirects
- Authentication anomalies

---

## Investigation Tips

- Review HTTP requests containing scripts.
- Check application logs.
- Determine whether user sessions were stolen.
- Identify affected users.

---

# 3. Directory Traversal

## What is Directory Traversal?

Directory Traversal is an attack where an attacker attempts to access files outside the intended web directory.

Instead of requesting normal web pages, the attacker manipulates file paths to reach sensitive system files.

---

## Example

```text
../../../../windows/system32

../../../etc/passwd
```

---

## Why Do Attackers Use It?

Attackers attempt to access:

- Password files
- Configuration files
- Application secrets
- System information

---

## SOC Indicators

Look for:

- Multiple "../" sequences
- Requests targeting sensitive directories
- Repeated HTTP 403 or 404 responses
- WAF alerts

---

## Investigation Tips

- Review requested URLs.
- Check web server logs.
- Identify downloaded files.
- Determine whether sensitive files were exposed.

---

# 4. File Upload Attack

## What is a File Upload Attack?

Many websites allow users to upload files.

Examples include:

- Profile pictures
- Documents
- Images
- Resumes

If file uploads are not properly validated, attackers may upload malicious files instead of legitimate documents.

---

## Example

Instead of uploading:

```text
resume.pdf
```

An attacker uploads:

```text
shell.php
```

The uploaded file can later be executed on the web server.

---

## Why Do Attackers Use It?

Common objectives include:

- Remote Code Execution
- Web Shell deployment
- Malware delivery
- Server compromise

---

## SOC Indicators

Watch for:

- Executable files uploaded through web forms
- Unexpected PHP or ASPX files
- Multiple upload attempts
- Large upload activity

---

# 5. Web Shell

## What is a Web Shell?

A web shell is a malicious script uploaded to a compromised web server.

Once executed, it allows attackers to remotely control the server through a web browser.

Think of it as a command prompt that runs inside a web application.

---

## Why Do Attackers Use Web Shells?

Web shells are commonly used to:

- Execute operating system commands
- Upload additional malware
- Create new administrator accounts
- Steal sensitive files
- Maintain persistence

---

## SOC Indicators

Watch for:

- Unexpected PHP, ASPX, JSP, or WAR files
- Command execution through web applications
- Unusual outbound connections
- Repeated access to uploaded scripts

---

## Investigation Tips

- Review recently uploaded files.
- Compare file hashes with known-good versions.
- Identify who uploaded the file.
- Review server logs.
- Check for persistence mechanisms.

---

# 6. Command Injection

## What is Command Injection?

Command Injection occurs when an attacker manipulates user input to execute operating system commands on the server.

Unlike SQL Injection, the attacker is targeting the operating system rather than the database.

---

## Example

Commands such as:

```text
whoami

ipconfig

dir

ping
```

may be executed if the application is vulnerable.

---

## Why Do Attackers Use It?

Attackers use command injection to:

- Execute arbitrary commands
- Download malware
- Escalate privileges
- Move laterally
- Gain full control of the server

---

## SOC Indicators

- Unexpected system commands
- Suspicious child processes
- PowerShell execution
- Network connections from web servers

---

# 7. Credential Stuffing

## What is Credential Stuffing?

Credential Stuffing is an attack where attackers use stolen username and password combinations obtained from previous data breaches.

Many users reuse passwords across multiple websites.

Attackers take advantage of this by attempting automated logins against different services.

---

## SOC Indicators

Look for:

- Large number of login attempts
- Successful logins from unusual locations
- Multiple accounts accessed from the same IP
- High authentication failure rates

---

# 8. Brute Force Attack

## What is a Brute Force Attack?

A brute force attack is a trial-and-error technique where attackers repeatedly guess passwords until one succeeds.

Unlike Credential Stuffing, attackers are not using stolen credentials—they are simply trying many password combinations.

---

## SOC Indicators

Watch for:

- Continuous login failures
- Multiple authentication attempts
- Account lockouts
- Login attempts from unfamiliar IP addresses

---

# How Attackers Abuse HTTPS

Many beginners assume:

```text
HTTPS

↓

Secure Website
```

This is a common misconception.

HTTPS only encrypts communication.

It **does not guarantee that a website is trustworthy**.

Attackers also use HTTPS certificates for:

- Phishing websites
- Malware downloads
- Fake banking portals
- Command and Control servers
- Credential harvesting pages

For this reason, analysts should never assume a website is safe simply because it uses HTTPS.

Always verify:

- Domain reputation
- Certificate details
- Registration age
- URL structure
- Threat intelligence

A secure connection does not always mean a legitimate destination.

---
