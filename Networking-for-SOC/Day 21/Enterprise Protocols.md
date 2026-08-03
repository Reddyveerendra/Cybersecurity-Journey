# Day 5 – Enterprise Protocols

## Objective

Learn common enterprise protocols, their default ports, purpose, encryption, and security relevance from a **SOC Analyst perspective**.

---

## 1. HTTP – Hypertext Transfer Protocol

**Port:** TCP 80
**Encryption:** No

HTTP is used for communication between web browsers and web servers.

```text
Client → HTTP Request → Web Server
Client ← HTTP Response ← Web Server
```

### Example

```text
GET /login
```

### SOC Perspective

HTTP traffic is unencrypted and may expose:

* URLs
* HTTP headers
* User-Agent
* Data
* Potential credentials

### Security Risks

* Credential theft
* Man-in-the-Middle attacks
* Malicious web traffic
* Command and Control (C2)

---

## 2. HTTPS – Hypertext Transfer Protocol Secure

**Port:** TCP 443
**Encryption:** TLS

HTTPS is HTTP protected using **TLS (Transport Layer Security)**.

```text
HTTP + TLS = HTTPS
```

### SOC Perspective

The application data is encrypted, but analysts can still investigate:

* Source IP
* Destination IP
* Domain
* TLS version
* Certificate
* Connection timing
* Traffic volume

---

## 3. FTP – File Transfer Protocol

**Control Port:** TCP 21
**Data Port:** TCP 20 (active mode)
**Encryption:** No

FTP is used to upload and download files.

```text
Client → FTP Server
       ↓
Upload / Download Files
```

### SOC Perspective

Monitor for:

* Unauthorized file transfers
* Data exfiltration
* Brute-force attacks
* Suspicious FTP connections

---

## 4. SFTP – SSH File Transfer Protocol

**Port:** TCP 22
**Encryption:** SSH encryption

SFTP provides secure file transfer over SSH.

```text
Client
   ↓
SFTP over SSH
   ↓
Server
```

### Important

SFTP is **not simply FTP + SSH**. It is a separate file-transfer protocol that operates over SSH.

### SOC Perspective

Monitor for:

* Unauthorized file transfers
* Large file uploads
* Suspicious external connections
* Compromised SSH accounts

---

## 5. SSH – Secure Shell

**Port:** TCP 22
**Encryption:** Yes

SSH provides secure remote access to Linux and Unix systems.

### Uses

* Remote administration
* Secure file transfer
* Tunneling
* Port forwarding

### SOC Perspective

Monitor for:

* SSH brute force
* Password spraying
* Unauthorized remote access
* Compromised accounts
* Suspicious commands

---

## 6. Telnet

**Port:** TCP 23
**Encryption:** No

Telnet provides remote terminal access but sends credentials and data in plaintext.

```text
Client → Telnet → Remote Server
```

### SOC Perspective

Telnet is insecure and is generally replaced by SSH.

Look for:

* Clear-text credentials
* Unauthorized remote access
* Brute-force attempts
* Legacy systems

---

## 7. SMTP – Simple Mail Transfer Protocol

**Port:** TCP 25
**Mail Submission:** TCP 587
**SMTP over TLS:** TCP 465

SMTP is primarily used to **send and transfer email**.

```text
User
  ↓
Mail Client
  ↓
SMTP Server
  ↓
Recipient Mail Server
```

### SOC Perspective

SMTP is important for investigating:

* Phishing
* Spam
* Malicious attachments
* Business Email Compromise
* Email-based malware

---

## 8. POP3 – Post Office Protocol Version 3

**Port:** TCP 110
**Secure POP3:** TCP 995

POP3 is mainly used to download emails from a mail server to a mail client.

```text
Mail Server
     ↓
Download Email
     ↓
Mail Client
```

Depending on configuration, emails may be removed from the server after download.

### SOC Perspective

Monitor for:

* Suspicious authentication
* Compromised email accounts
* Unusual email access
* External connections

---

## 9. IMAP – Internet Message Access Protocol

**Port:** TCP 143
**Secure IMAP:** TCP 993

IMAP allows users to access and synchronize email with the mail server.

```text
Mail Server
     ↕
Mail Client
     ↕
Email Synchronization
```

### POP3 vs IMAP

| POP3                        | IMAP                        |
| --------------------------- | --------------------------- |
| Mainly downloads email      | Synchronizes email          |
| Less server-side management | More server-side management |
| TCP 110                     | TCP 143                     |
| Secure: 995                 | Secure: 993                 |

---

## 10. LDAP – Lightweight Directory Access Protocol

**Port:** TCP/UDP 389
**Secure LDAP (LDAPS):** TCP 636

LDAP is used to access and manage directory information.

It can provide information about:

* Users
* Groups
* Computers
* Organizational Units

```text
Application
    ↓
LDAP Query
    ↓
Directory Service
    ↓
User / Group Information
```

### SOC Perspective

Monitor for:

* User enumeration
* Group enumeration
* LDAP reconnaissance
* Suspicious directory queries
* Unauthorized access

### Important

LDAP uses **port 389**, not 441.

---

## 11. SMB – Server Message Block

**Port:** TCP 445
**Legacy NetBIOS:** TCP/UDP 137–139

SMB is commonly used in Windows environments for:

* File sharing
* Printer sharing
* Network resources
* Administrative operations

```text
Windows Client
      ↓
SMB
      ↓
File Server
      ↓
Shared Folder
```

### SOC Perspective

SMB is important for detecting:

* Lateral movement
* Credential attacks
* Ransomware propagation
* Remote administration
* Exploitation of SMB vulnerabilities

### Important

SMB commonly uses **TCP 445**, not 165.

---

## 12. Kerberos

**Port:** TCP/UDP 88

Kerberos is an authentication protocol widely used in **Active Directory environments**.

It uses tickets instead of repeatedly sending the user's password across the network.

### Main Components

```text
KDC
├── Authentication Service (AS)
└── Ticket Granting Service (TGS)
```

### Simplified Kerberos Authentication

```text
1. User authenticates
        ↓
2. AS issues TGT
        ↓
3. User presents TGT to TGS
        ↓
4. TGS issues Service Ticket
        ↓
5. User presents Service Ticket
        ↓
6. Access Service
```

### SOC Perspective

Important Kerberos attacks:

* Kerberoasting
* AS-REP Roasting
* Pass-the-Ticket
* Golden Ticket
* Silver Ticket

### Key Concept

```text
Authentication
      ↓
TGT
      ↓
TGS Request
      ↓
Service Ticket
      ↓
Access Service
```

---

## 13. RDP – Remote Desktop Protocol

**Port:** TCP/UDP 3389

RDP allows users to remotely access Windows systems using a graphical desktop.

```text
User
  ↓
RDP
  ↓
Windows Server / Workstation
```

### SOC Perspective

Monitor for:

* RDP brute force
* Unauthorized remote access
* Account compromise
* Lateral movement
* Ransomware activity

Example attack chain:

```text
Internet
   ↓
Exposed RDP
   ↓
Brute Force
   ↓
Compromised Account
   ↓
Remote Access
```

---

## 14. SNMP – Simple Network Management Protocol

**Port:** UDP 161 – SNMP queries
**Port:** UDP 162 – SNMP traps

SNMP is used to monitor and manage network devices.

### Examples

* Routers
* Switches
* Firewalls
* Servers
* Printers

```text
SNMP Manager
      ↓
SNMP Query
      ↓
Network Device
```

### SOC Perspective

Monitor for:

* Unauthorized SNMP access
* Default community strings
* Network device enumeration
* Network reconnaissance

### Important

SNMP means:

**Simple Network Management Protocol**

Not "Simple Network Mail Protocol."

---

## 15. NTP – Network Time Protocol

**Port:** UDP 123

NTP synchronizes system clocks across network devices.

```text
Computer
    ↓
NTP Request
    ↓
NTP Server
    ↓
Correct Time
```

### SOC Perspective

Accurate time synchronization is critical for:

* Incident investigation
* Log correlation
* Timeline creation
* SIEM analysis

NTP can also be abused in:

* Reflection attacks
* Amplification attacks
* Time manipulation attempts

### Important

NTP means:

**Network Time Protocol**

Not "Network Transmission Protocol."

---

# Enterprise Protocol Quick Reference

| Protocol | Purpose                | Default Port | Encryption      |
| -------- | ---------------------- | ------------ | --------------- |
| HTTP     | Web traffic            | TCP 80       | No              |
| HTTPS    | Secure web traffic     | TCP 443      | TLS             |
| FTP      | File transfer          | TCP 20/21    | No              |
| SFTP     | Secure file transfer   | TCP 22       | SSH             |
| SSH      | Secure remote access   | TCP 22       | Yes             |
| Telnet   | Remote access          | TCP 23       | No              |
| SMTP     | Email sending/transfer | TCP 25       | Depends         |
| POP3     | Download email         | TCP 110      | No / 995 secure |
| IMAP     | Email synchronization  | TCP 143      | No / 993 secure |
| LDAP     | Directory services     | TCP/UDP 389  | No / 636 secure |
| SMB      | Windows file sharing   | TCP 445      | Depends         |
| Kerberos | Authentication         | TCP/UDP 88   | Cryptographic   |
| RDP      | Remote Windows desktop | TCP/UDP 3389 | Encrypted       |
| SNMP     | Network management     | UDP 161/162  | Depends         |
| NTP      | Time synchronization   | UDP 123      | Usually no      |

---

# SOC Analyst Memory Map

```text
WEB
HTTP      → 80
HTTPS     → 443

REMOTE ACCESS
Telnet    → 23
SSH       → 22
RDP       → 3389

FILE TRANSFER / SHARING
FTP       → 20/21
SFTP      → 22
SMB       → 445

EMAIL
SMTP      → 25
POP3      → 110
IMAP      → 143

IDENTITY / AUTHENTICATION
LDAP      → 389
Kerberos  → 88

NETWORK MANAGEMENT
SNMP      → 161/162

TIME
NTP       → 123
```

# SOC Investigation Connections

```text
Kerberos
    ↓
Active Directory Authentication

LDAP
    ↓
Directory / User / Group Queries

SMB
    ↓
Windows File Sharing / Lateral Movement

RDP
    ↓
Remote Access / Lateral Movement

SSH
    ↓
Linux Remote Access

HTTP / HTTPS
    ↓
Web Traffic / C2

SMTP
    ↓
Phishing / Email Attacks

NTP
    ↓
Log Correlation / Incident Timeline

SNMP
    ↓
Network Device Monitoring
```

# Key Takeaways

* **HTTP 80** → Unencrypted web traffic
* **HTTPS 443** → HTTP protected by TLS
* **FTP 20/21** → Unencrypted file transfer
* **SFTP 22** → Secure file transfer over SSH
* **SSH 22** → Secure remote access
* **Telnet 23** → Unencrypted remote access
* **SMTP 25** → Email sending/transfer
* **POP3 110** → Email downloading
* **IMAP 143** → Email synchronization
* **LDAP 389** → Directory services
* **SMB 445** → Windows file sharing
* **Kerberos 88** → Authentication
* **RDP 3389** → Remote Windows access
* **SNMP 161/162** → Network management
* **NTP 123** → Time synchronization

## SOC Mindset

When you see a network connection in a log, don't just identify the port.

Ask:

```text
What protocol is being used?
        ↓
Who is communicating?
        ↓
What is the destination?
        ↓
Is the connection expected?
        ↓
Is the protocol encrypted?
        ↓
Is the port being used normally?
        ↓
Could this indicate an attack?
```

Understanding **protocol + port + behavior** is more important than simply memorizing port numbers.
