# Day 7 – Network Security

## Objective

Understand the core network security technologies used in enterprise environments and learn how they are relevant to a **SOC Analyst**.

Topics covered:

* Firewalls
* Stateless vs Stateful Firewalls
* Next-Generation Firewalls (NGFW)
* IDS vs IPS
* VPN
* Proxy
* Web Proxy
* WAF
* NAC
* Zero Trust
* DMZ
* Network Segmentation
* Threat Intelligence

---

# 1. Firewall

A **firewall** is a hardware, software, or cloud-based security control that monitors and controls network traffic based on predefined security rules and policies.

A firewall can make decisions based on:

* Source IP
* Destination IP
* Source Port
* Destination Port
* Protocol
* Application
* User or Identity
* Domain or URL (depending on firewall type)

### Example

```text
Internet
    |
    v
Firewall
    |
    v
Internal Network
```

### Example Rule

```text
Allow
Source: Internal Network
Destination: Web Server
Port: TCP 443
Action: Allow
```

Another example:

```text
Deny
Source: Internet
Destination: Internal Network
Port: TCP 445
Action: Block
```

### SOC Perspective

Firewall logs can help detect:

* Blocked connections
* Allowed connections
* Port scanning
* Suspicious external IPs
* Command and Control (C2)
* Unauthorized access attempts

---

# 2. Stateless Firewall

A **stateless firewall** evaluates each network packet independently.

It does not maintain the state of an ongoing connection.

It typically makes decisions using:

* Source IP
* Destination IP
* Source Port
* Destination Port
* Protocol

### Example

```text
Packet 1
    |
    v
Check Firewall Rule
    |
    v
Allow / Block

Packet 2
    |
    v
Check Firewall Rule Again
    |
    v
Allow / Block
```

The firewall does not remember whether a packet belongs to an existing connection.

### SOC Perspective

Stateless firewalls are simple and fast but provide less context about network sessions.

---

# 3. Stateful Firewall

A **stateful firewall** tracks active network connections and maintains a state table.

Example:

```text
Internal Client
192.168.1.10
      |
      | TCP 443
      v
Web Server
8.8.8.8
```

The firewall can track:

```text
Source IP: 192.168.1.10
Source Port: 50000
Destination IP: 8.8.8.8
Destination Port: 443
Protocol: TCP
State: ESTABLISHED
```

### TCP Connection

```text
SYN
  |
  v
SYN-ACK
  |
  v
ACK
  |
  v
ESTABLISHED
```

The firewall tracks the state of the connection.

### SOC Perspective

Stateful firewalls provide more context and can identify unusual connection behavior.

---

# 4. NGFW – Next-Generation Firewall

A **Next-Generation Firewall (NGFW)** provides traditional firewall capabilities along with advanced security features.

Common capabilities include:

* Traditional firewall filtering
* Application awareness
* Intrusion Prevention System (IPS)
* User and identity-based policies
* URL filtering
* Malware detection
* SSL/TLS inspection
* Threat intelligence integration

### Traditional Firewall

May see:

```text
TCP 443
```

### NGFW

May identify:

```text
Application: Microsoft Teams
User: User1
Source: 192.168.1.10
Destination: Application Server
```

### SOC Perspective

NGFW logs can help detect:

* Command and Control (C2)
* Suspicious applications
* Lateral movement
* Data exfiltration
* Malicious IP connections

---

# 5. IDS – Intrusion Detection System

An IDS detects suspicious or malicious activity and generates an alert.

```text
Network Traffic
      |
      v
IDS
      |
      v
Suspicious Activity
      |
      v
Alert
```

An IDS generally does not automatically block the traffic.

### SOC Perspective

The SOC analyst investigates the generated alert and determines whether the activity is:

* True Positive
* False Positive
* Benign Activity

---

# 6. IPS – Intrusion Prevention System

An IPS detects suspicious activity and can automatically take action.

```text
Network Traffic
      |
      v
IPS
      |
      v
Detect Attack
      |
      v
Block / Drop / Reset
```

### Simple Difference

```text
IDS = Detect + Alert

IPS = Detect + Prevent
```

### SOC Perspective

An IPS event may show:

```text
Attack Detected
      |
      v
Connection Blocked
      |
      v
SOC Investigation
```

---

# 7. VPN – Virtual Private Network

A VPN creates an encrypted connection over an untrusted network such as the Internet.

Example:

```text
Employee
    |
    v
Encrypted VPN Tunnel
    |
    v
Internet
    |
    v
VPN Gateway
    |
    v
Internal Network
```

A VPN allows authorized users to securely access internal resources.

### SOC Perspective

Monitor VPN logs for:

* Failed login attempts
* Brute-force attacks
* Impossible travel
* Unusual login locations
* Unusual login times
* Compromised accounts
* Multiple simultaneous sessions

---

# 8. Proxy

A proxy acts as an intermediary between a client and another server.

Without Proxy:

```text
Client
   |
   v
Internet
```

With Proxy:

```text
Client
   |
   v
Proxy
   |
   v
Internet
```

The destination server sees the proxy's IP address instead of directly seeing the client's IP address.

### Important

A proxy does not automatically hide your IP address from your ISP.

The ISP can still see that you are communicating with the proxy.

The proxy primarily acts as an intermediary between the client and destination.

### SOC Perspective

Proxy logs can contain:

* User
* Source IP
* Destination Domain
* URL
* HTTP Method
* User-Agent
* Response Code
* Bytes Transferred

---

# 9. Web Proxy

A **Web Proxy** specifically handles web traffic such as HTTP and HTTPS.

```text
User
  |
  v
Web Proxy
  |
  v
Internet
  |
  v
Website
```

Organizations use web proxies to:

* Filter websites
* Block malicious URLs
* Monitor web activity
* Enforce browsing policies
* Detect malware
* Apply threat intelligence

### Example

```text
User
  |
  v
Web Proxy
  |
  v
Threat Intelligence Check
  |
  +---- Trusted Domain → ALLOW
  |
  +---- Malicious Domain → BLOCK
```

### SOC Perspective

Web proxy logs can help detect:

* Phishing
* Malware downloads
* Command and Control
* Suspicious domains
* Data exfiltration

---

# 10. WAF – Web Application Firewall

A **Web Application Firewall (WAF)** protects web applications from attacks targeting the application layer.

```text
Internet
    |
    v
WAF
    |
    v
Web Application
    |
    v
Database
```

A WAF can detect and block attacks such as:

* SQL Injection
* Cross-Site Scripting (XSS)
* Path Traversal
* Command Injection
* Malicious HTTP requests

### Example

Normal request:

```text
GET /products?id=10
```

Suspicious request:

```text
GET /products?id=10' OR '1'='1
```

The WAF may detect the suspicious request and block it.

---

## WAF Logs

WAF logs may contain:

* Source IP
* Destination/Application
* Request URI
* HTTP Method
* User-Agent
* Rule ID
* Attack Type
* Action
* HTTP Response Code
* Country
* Timestamp

### Example

```text
Source IP: 185.x.x.x
URI: /login
Method: POST
Attack: SQL Injection
Rule ID: 942100
Action: Block
```

---

## WAF KQL Investigation

### Find SQL Injection Attempts

```kusto
WAFLogs
| where TimeGenerated > ago(24h)
| where AttackType =~ "SQL Injection"
| summarize
    Attempts = count(),
    UniqueURLs = dcount(RequestUri)
    by SourceIP, bin(TimeGenerated, 1h)
| order by Attempts desc
```

### Investigate a Specific IP

```kusto
WAFLogs
| where SourceIP == "185.x.x.x"
| project
    TimeGenerated,
    SourceIP,
    RequestUri,
    HttpMethod,
    AttackType,
    Action,
    RuleId,
    ResponseCode
| order by TimeGenerated desc
```

### SOC Investigation Flow

```text
External IP
     |
     v
SQL Injection Attempt
     |
     v
WAF Detects Attack
     |
     v
WAF Blocks Request
     |
     v
Multiple Attempts
     |
     v
SOC Investigation
```

### Important

A WAF block does not automatically mean the attack succeeded.

Investigate:

* Was the request blocked?
* Did the application respond?
* Was there a successful HTTP 200 response?
* Were there database errors?
* Did suspicious activity occur after the request?

---

# 11. NAC – Network Access Control

NAC controls which users and devices are allowed to access a network.

Example:

```text
Laptop
   |
   v
NAC
   |
   +---- Check User
   |
   +---- Check Device
   |
   +---- Check Security Posture
   |
   +---- Check Antivirus
   |
   +---- Check OS
   |
   v
Allow / Deny / Quarantine
```

### Example

Compliant corporate laptop:

```text
Updated OS
Antivirus Enabled
Corporate Device
      |
      v
ALLOW
```

Unknown device:

```text
Unknown Device
No Security Software
      |
      v
QUARANTINE
```

### SOC Perspective

NAC can help identify:

* Unauthorized devices
* Rogue devices
* Non-compliant endpoints
* Network access violations

---

# 12. Zero Trust

Zero Trust follows the principle:

> Never trust implicitly; verify explicitly and continuously.

Traditional security model:

```text
Inside Network = Trusted
Outside Network = Untrusted
```

Zero Trust:

```text
User
Device
Location
Application
Risk
    |
    v
Continuous Verification
    |
    v
Access Decision
```

### Core Principles

* Verify explicitly
* Use least privilege
* Assume breach

### SOC Perspective

Zero Trust helps reduce:

* Lateral movement
* Excessive privileges
* Unauthorized access
* Insider threats

---

# 13. DMZ – Demilitarized Zone

A DMZ is a separate network segment used to host systems that need to be accessible from external networks.

Examples:

* Public Web Servers
* Public DNS Servers
* Mail Servers
* Reverse Proxies

### Example Architecture

```text
Internet
    |
    v
Firewall
    |
    v
DMZ
    |
    +---- Web Server
    |
    +---- Mail Server
    |
    +---- Public DNS
    |
    v
Internal Firewall
    |
    v
Internal Network
```

The purpose of a DMZ is to prevent direct access from the Internet to the internal network.

### SOC Perspective

If a public web server in the DMZ is compromised, segmentation should limit the attacker's ability to move into the internal network.

---

# 14. Network Segmentation

Network segmentation divides a large network into smaller logical or physical network segments.

Example:

```text
Corporate Network
       |
       +---- User Network
       |
       +---- Server Network
       |
       +---- Database Network
       |
       +---- Security Network
       |
       +---- Guest Network
```

Access between segments can be controlled using:

* Firewalls
* ACLs
* VLANs
* NAC
* Identity-based policies

### SOC Perspective

Network segmentation helps limit:

* Lateral movement
* Attack surface
* Blast radius
* Unauthorized access

### Example

```text
Attacker
   |
   v
Compromised User PC
   |
   X
Cannot directly access
   |
   v
Database Network
```

---

# 15. Threat Intelligence

Threat Intelligence is information about threats that helps organizations detect, investigate, and respond to attacks.

Threat intelligence can include:

* Malicious IP addresses
* Malicious domains
* File hashes
* Malicious URLs
* Email addresses
* Attack techniques
* Threat actor information

---

## IOC – Indicator of Compromise

An IOC is evidence that may indicate a system has been compromised.

Examples:

* Malicious IP
* Malicious Domain
* File Hash
* Malicious URL
* Suspicious Email Address

---

## IOA – Indicator of Attack

An IOA represents suspicious behavior or activity that may indicate an attack.

Examples:

* PowerShell downloading a suspicious file
* Multiple failed login attempts
* Credential dumping
* Unusual lateral movement
* Suspicious process execution

### Threat Intelligence Flow

```text
Threat Intelligence
        |
        v
IOC / IOA
        |
        v
SIEM / EDR / Firewall / WAF
        |
        v
Detection
        |
        v
SOC Alert
        |
        v
Investigation
```

### SOC Perspective

Threat intelligence can be used to:

* Enrich alerts
* Block malicious IPs
* Block malicious domains
* Identify known malware
* Investigate suspicious activity
* Improve detection rules

---

# Network Security – SOC Memory Map

```text
FIREWALL
    |
    v
Control Network Traffic

STATELESS
    |
    v
Checks Packets Independently

STATEFUL
    |
    v
Tracks Connection State

NGFW
    |
    v
Advanced Firewall + Security Features

IDS
    |
    v
Detect + Alert

IPS
    |
    v
Detect + Prevent

VPN
    |
    v
Encrypted Tunnel

PROXY
    |
    v
Intermediary

WEB PROXY
    |
    v
Web Traffic Filtering / Monitoring

WAF
    |
    v
Protects Web Applications

NAC
    |
    v
Controls Network Access

ZERO TRUST
    |
    v
Verify Explicitly + Least Privilege

DMZ
    |
    v
Publicly Accessible Systems

NETWORK SEGMENTATION
    |
    v
Limit Lateral Movement

THREAT INTELLIGENCE
    |
    v
IOC + IOA + Context
```

---

# SOC Investigation Connections

## WAF

```text
SQL Injection / XSS
        |
        v
WAF Detection
        |
        v
WAF Logs
        |
        v
KQL Investigation
```

## Firewall

```text
Blocked / Allowed Connection
        |
        v
Suspicious IP
        |
        v
Threat Intelligence Enrichment
        |
        v
SOC Investigation
```

## VPN

```text
VPN Login
    |
    v
Unusual Location
    |
    v
Impossible Travel
    |
    v
Potential Account Compromise
```

## NAC

```text
Unknown Device
    |
    v
Network Access Attempt
    |
    v
NAC Detection
    |
    v
Allow / Deny / Quarantine
```

## DMZ

```text
Public Web Server
       |
       v
Compromise
       |
       v
Network Segmentation
       |
       v
Limit Lateral Movement
```

---

# Key Takeaways

* **Firewall** → Controls network traffic using rules and policies.
* **Stateless Firewall** → Evaluates packets independently.
* **Stateful Firewall** → Tracks active connections.
* **NGFW** → Firewall with advanced security capabilities.
* **IDS** → Detects and alerts.
* **IPS** → Detects and prevents.
* **VPN** → Creates an encrypted tunnel over an untrusted network.
* **Proxy** → Acts as an intermediary between client and destination.
* **Web Proxy** → Filters and monitors web traffic.
* **WAF** → Protects web applications from application-layer attacks.
* **NAC** → Controls network access based on users and device security posture.
* **Zero Trust** → Verify explicitly and continuously; use least privilege.
* **DMZ** → Isolates publicly accessible systems from the internal network.
* **Network Segmentation** → Limits lateral movement and attack impact.
* **Threat Intelligence** → Provides IOCs, IOAs, and context for detecting threats.

---

# SOC Analyst Mindset

When investigating a network security event, ask:

```text
Who is connecting?
        |
        v
Where are they connecting?
        |
        v
What protocol and port are being used?
        |
        v
Is the connection allowed or blocked?
        |
        v
Is the destination trusted?
        |
        v
Is the device authorized?
        |
        v
Is the behavior normal?
        |
        v
Could this indicate an attack?
        |
        v
Which logs can confirm the activity?
```

## Final Goal

Do not just memorize security technologies.

Understand the relationship:

```text
Network Traffic
      |
      v
Security Control
      |
      v
Logs
      |
      v
SIEM
      |
      v
Detection
      |
      v
SOC Alert
      |
      v
Investigation
      |
      v
Response
```

This is the core workflow of **Network Security from a SOC Analyst perspective**.
