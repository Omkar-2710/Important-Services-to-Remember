# Email Protocols (SMTP, POP3 & IMAP)

> A practical guide to understanding email protocols from a Security Operations Center (SOC) Analyst's perspective.

---

## Table of Contents

- [Overview](#overview)
- [Why Email Protocols Matter](#why-email-protocols-matter)
- [How Email Works](#how-email-works)
- [SMTP (Sending Email)](#smtp-sending-email)
- [POP3 (Downloading Email)](#pop3-downloading-email)
- [IMAP (Synchronizing Email)](#imap-synchronizing-email)
- [Ports and Protocols](#ports-and-protocols)
- [POP3 vs IMAP](#pop3-vs-imap)
- [Email Protocols in Enterprise Environments](#email-protocols-in-enterprise-environments)
- [Why Email Protocols Matter in SOC](#why-email-protocols-matter-in-soc)
- [Common Email-Based Attacks](#common-email-based-attacks)
- [SOC Investigation Workflow](#soc-investigation-workflow)
- [Common SIEM Alerts](#common-siem-alerts)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Interview Questions](#interview-questions)
- [Key Takeaways](#key-takeaways)

---

# Overview

Email communication relies on three primary protocols that work together.

- **SMTP (Simple Mail Transfer Protocol)** sends emails.
- **POP3 (Post Office Protocol v3)** downloads emails from the mail server.
- **IMAP (Internet Message Access Protocol)** synchronizes emails across multiple devices.

Understanding these protocols is essential because email remains one of the most common entry points for cyber attacks. More than 90% of phishing campaigns begin with email communication.

---

# Why Email Protocols Matter

Every email follows two distinct processes:

1. Sending the email.
2. Receiving the email.

Different protocols handle each task.

Organizations rely on email protocols for:

- Business communication
- Internal collaboration
- Customer communication
- Email synchronization
- Notifications and alerts
- File sharing
- Authentication workflows

Without these protocols, modern email services would not function reliably.

---

# How Email Works

```text
Sender
   │
Compose Email
   │
   ▼
SMTP
   │
   ▼
Sender Mail Server
   │
   ▼
Internet
   │
   ▼
Recipient Mail Server
   │
   ▼
POP3 / IMAP
   │
   ▼
Recipient Inbox
```

Typical workflow:

1. User composes an email.
2. SMTP sends the message to the sender's mail server.
3. The email is delivered across the Internet.
4. The recipient's mail server receives the message.
5. POP3 downloads the email or IMAP synchronizes it across devices.

---

# SMTP (Sending Email)

**Simple Mail Transfer Protocol (SMTP)** is responsible for sending emails from email clients to mail servers and between mail servers.

SMTP is responsible only for **sending** email.

Common use cases include:

- Sending emails
- Mail server communication
- Business email delivery
- Notification systems
- Application-generated emails

---

# POP3 (Downloading Email)

**Post Office Protocol Version 3 (POP3)** downloads emails from the mail server to a local device.

After downloading, emails may optionally be removed from the mail server.

POP3 is best suited for:

- Offline email access
- Single-device usage
- Local mailbox storage

---

# IMAP (Synchronizing Email)

**Internet Message Access Protocol (IMAP)** allows users to access and synchronize emails directly from the mail server.

Unlike POP3, emails remain stored on the server, allowing multiple devices to display the same mailbox.

IMAP is commonly used for:

- Microsoft Outlook
- Mobile devices
- Tablets
- Multiple-device synchronization
- Enterprise email environments

---

# Ports and Protocols

| Protocol | Port | Secure Port | Purpose |
|----------|------|-------------|---------|
| SMTP | 25 | 465 / 587 | Sending emails |
| POP3 | 110 | 995 | Downloading emails |
| IMAP | 143 | 993 | Synchronizing emails |

---

# POP3 vs IMAP

| POP3 | IMAP |
|------|------|
| Downloads emails locally | Keeps emails on the server |
| Primarily designed for one device | Supports multiple devices |
| Offline access | Mailbox synchronization |
| Optional server deletion | Messages remain on the server |

IMAP is the preferred protocol in modern enterprise environments because users commonly access the same mailbox from laptops, mobile phones, and tablets. :contentReference[oaicite:1]{index=1}

---

# Email Protocols in Enterprise Environments

Email protocols are used throughout enterprise environments, including:

- Microsoft Exchange
- Microsoft 365
- Outlook
- Gmail Workspace
- Help Desk systems
- Automated notification platforms
- Business communication systems

Email remains one of the most critical communication services in modern organizations.

---

# Why Email Protocols Matter in SOC

Email is one of the primary attack vectors used by threat actors.

SOC analysts monitor email activity to identify:

- Phishing campaigns
- Email spoofing
- Business Email Compromise (BEC)
- Malware delivery
- Malicious URLs
- Credential theft
- Spam campaigns
- Suspicious mailbox activity

A typical attack chain often begins with email before progressing to credential theft, lateral movement, and ransomware deployment. :contentReference[oaicite:2]{index=2}

---

# Common Email-Based Attacks

## Phishing

Fraudulent emails designed to trick users into revealing credentials or executing malicious content.

### Indicators

- Suspicious sender
- Fake login pages
- Urgent language
- Unexpected attachments

---

## Spear Phishing

Highly targeted phishing attacks directed at specific individuals or departments.

Common targets include:

- Finance
- Human Resources
- Executives
- IT Administrators

---

## Business Email Compromise (BEC)

Attackers compromise or impersonate trusted business accounts to convince employees to transfer money or disclose sensitive information.

---

## Email Spoofing

Attackers forge the sender's email address to appear as a trusted individual or organization.

### Indicators

- SPF failures
- DKIM failures
- DMARC failures
- Domain impersonation

---

## Malware Attachments

Attackers distribute malicious files through email attachments.

Common attachment types include:

- PDF
- DOCX
- ZIP
- EXE

---

## Malicious URLs

Emails contain links directing users to phishing pages or malware-hosting websites.

---

# SOC Investigation Workflow

When investigating a suspicious email alert, verify:

- Sender address
- Recipient(s)
- Internal or external sender
- SPF, DKIM, and DMARC validation
- Email subject
- Attachments
- Embedded URLs
- Whether users clicked the link
- Whether credentials were entered
- Malware detections
- Endpoint activity
- Related authentication logs
- EDR telemetry
- Signs of lateral movement

---

# Common SIEM Alerts

Examples include:

- High Confidence Phishing
- Email Spoofing
- Malware Attachment Detected
- Suspicious URL Clicked
- Mass Email Campaign
- Business Email Compromise (BEC)
- SPF Failure
- DKIM Failure
- DMARC Failure
- Mailbox Forwarding Rule Created

---

# MITRE ATT&CK Mapping

| Tactic | Email Usage |
|---------|-------------|
| Initial Access | Phishing emails |
| Execution | Malicious attachments |
| Credential Access | Fake login pages |
| Persistence | Malicious mailbox forwarding rules |
| Collection | Email theft |

---

# Interview Questions

### What is SMTP?

SMTP (Simple Mail Transfer Protocol) is used to send emails from email clients to mail servers and between mail servers. It commonly uses TCP Ports **25**, **465**, and **587**. :contentReference[oaicite:3]{index=3}

---

### What is POP3?

POP3 (Post Office Protocol Version 3) downloads emails from a mail server to a local device. It commonly uses TCP Port **110**, while secure POP3 uses **995**. :contentReference[oaicite:4]{index=4}

---

### What is IMAP?

IMAP (Internet Message Access Protocol) allows users to access and synchronize emails directly from the mail server across multiple devices. It commonly uses TCP Port **143**, while secure IMAP uses **993**. :contentReference[oaicite:5]{index=5}

---

### What is the difference between SMTP, POP3, and IMAP?

- SMTP sends emails.
- POP3 downloads emails to a local device.
- IMAP synchronizes emails across multiple devices while keeping them on the server. :contentReference[oaicite:6]{index=6}

---

### Why are email protocols important for SOC Analysts?

Email protocols are important because phishing remains one of the most common initial access techniques used by attackers. Monitoring email activity helps identify phishing, malware delivery, spoofing, and credential theft before attackers can move deeper into the environment. :contentReference[oaicite:7]{index=7}

---

### How would you investigate a phishing email alert?

A typical investigation includes:

- Identifying the sender
- Reviewing recipients
- Verifying SPF, DKIM, and DMARC
- Analyzing attachments and URLs
- Determining whether users clicked the link
- Reviewing endpoint activity
- Correlating with authentication and EDR logs

---

# Key Takeaways

| Topic | Summary |
|--------|---------|
| SMTP | Sends emails |
| POP3 | Downloads emails to a local device |
| IMAP | Synchronizes emails across multiple devices |
| SMTP Ports | 25, 465, 587 |
| POP3 Ports | 110, 995 |
| IMAP Ports | 143, 993 |
| SOC Relevance | Critical for detecting phishing, spoofing, malware delivery, and account compromise |
| Common Threats | Phishing, BEC, Email Spoofing, Malware Attachments, Malicious URLs |

---
