# Windows Authentication - Kerberos, NTLM & SPNs

This document provides an overview of **Windows Authentication**, including **Kerberos**, **NTLM**, **Tickets (TGT & Service Tickets)**, **Authentication Flow**, and **Service Principal Names (SPNs)**.

---

# 1. Kerberos Authentication

Kerberos is the **default and modern authentication protocol** used in Microsoft Active Directory. It is a **ticket-based authentication protocol** that provides:

- Secure authentication
- Mutual authentication
- Single Sign-On (SSO)
- Protection against password sniffing
- Ticket-based access to network resources

## Components

### 1. Authentication Service (AS)

The Authentication Service (AS) is responsible for:

- Verifying the user's credentials.
- Authenticating the username and password.
- Issuing a **Ticket Granting Ticket (TGT)**.
- Creating a session key for secure communication between the client and the KDC.

> **Note:** The TGT is encrypted and signed using the **KRBTGT account's secret key**.

---

### 2. Ticket Granting Service (TGS)

The Ticket Granting Service (TGS) is responsible for:

- Validating the user's TGT.
- Receiving service requests from authenticated users.
- Issuing a **Service Ticket** for the requested service.

Examples of services:

- File Server
- SQL Server
- IIS Web Server
- LDAP
- Remote Desktop

---

## Kerberos Authentication Flow

```text
User Login
     │
     ▼
Authentication Service (AS)
     │
     ▼
Username & Password Verified
     │
     ▼
TGT + Session Key Created
     │
     ▼
User Requests Service
     │
     ▼
Client Sends TGT + SPN to TGS
     │
     ▼
TGS Validates TGT
     │
     ▼
Service Ticket Issued
     │
     ▼
Client Presents Service Ticket
     │
     ▼
Access Granted
```

---

# 2. NTLM Authentication

NTLM (NT LAN Manager) is Microsoft's **legacy authentication protocol**.

Unlike Kerberos, NTLM is a **challenge-response authentication protocol**.

Instead of sending the password over the network, Windows computes an **NT password hash** locally and uses it to generate a challenge response.

---

## LM Authentication (Older Version)

Before NTLM, Microsoft used **LAN Manager (LM)** authentication.

LM Process:

```text
Password
    │
Convert to Uppercase
    │
Split into Two Parts
    │
Encrypt using DES
    │
Generate LM Hash
```

Limitations:

- Converts passwords to uppercase
- Uses weak DES encryption
- Easy to crack

---

## NTLM Authentication Flow

```text
User Requests Resource
        │
        ▼
Server Generates Random Challenge
        │
        ▼
Client Computes NTLM Response
(using NT Password Hash)
        │
        ▼
Username + Challenge Response Sent
        │
        ▼
Server Forwards Request to Domain Controller
        │
        ▼
Domain Controller Computes Expected Response
        │
        ▼
Responses Match?
        │
       Yes
        ▼
Access Granted
```

---

## Common NTLM Attacks

### Pass-the-Hash (PtH)

- Steals the NT hash.
- Uses the hash to authenticate without knowing the password.

### NTLM Relay

- Relays NTLM authentication to another server.
- No password cracking required.

### Offline Password Cracking

- Attempts to recover passwords from captured NT hashes.

---

# 3. Kerberos Tickets

Kerberos uses two primary tickets.

---

## Ticket Granting Ticket (TGT)

The **Ticket Granting Ticket (TGT)** is issued by the **Authentication Service (AS)** after successful user authentication.

Purpose:

- Proves the user has already authenticated.
- Used to request Service Tickets.
- Prevents sending the password multiple times.

Contents:

- Username
- User SID
- Group Membership
- Session Key
- Ticket Lifetime

Encrypted using:

- KRBTGT Account Secret Key

---

## Service Ticket

A **Service Ticket** is issued by the **Ticket Granting Service (TGS)**.

Purpose:

- Grants access to a specific service.
- Presented directly to the target server.

Examples:

- CIFS/FileServer
- MSSQLSvc/SQL01
- HTTP/WebServer
- LDAP/DC01

Encrypted using:

- Target Service Account Key

---

## Ticket Flow

```text
User Login
     │
     ▼
Authentication Service
     │
     ▼
TGT Created
     │
     ▼
User Requests SQL Server
     │
     ▼
TGS Validates TGT
     │
     ▼
Service Ticket Created
     │
     ▼
SQL Server
     │
     ▼
Access Granted
```

---

# 4. Kerberos Authentication Flow

```text
Username + Password
          │
          ▼
Authentication Service (AS)
          │
          ▼
TGT + Session Key
          │
          ▼
User Requests Resource
          │
          ▼
Client Sends TGT + SPN
          │
          ▼
Ticket Granting Service (TGS)
          │
          ▼
Service Ticket Issued
          │
          ▼
Client Presents Service Ticket
          │
          ▼
Target Server
          │
          ▼
Access Granted
```

---

# 5. Service Principal Names (SPNs)

A **Service Principal Name (SPN)** is the **unique identifier of a service** in Active Directory.

It tells Kerberos which service the user wants to access.

Without an SPN, the Domain Controller cannot issue the correct Service Ticket.

---

## SPN Format

```text
ServiceClass/Hostname
```

Examples:

```text
CIFS/FileServer

HTTP/WebServer

LDAP/DC01

MSSQLSvc/SQL01:1433

TERMSRV/Server01
```

---

## SPN Authentication Flow

```text
User Wants SQL Server
        │
        ▼
Client Identifies SPN
(MSSQLSvc/SQL01)
        │
        ▼
KDC Searches Active Directory
        │
        ▼
Finds Matching Service Account
        │
        ▼
Issues Service Ticket
        │
        ▼
Client Sends Service Ticket
        │
        ▼
SQL Server
        │
        ▼
Access Granted
```

---

# Kerberos vs NTLM

| Feature | Kerberos | NTLM |
|----------|-----------|------|
| Authentication Type | Ticket-Based | Challenge-Response |
| Password Sent | No | No |
| Tickets | ✅ Yes | ❌ No |
| Single Sign-On | ✅ Yes | ❌ Limited |
| Mutual Authentication | ✅ Yes | ❌ No |
| Security | High | Medium |
| Default in Active Directory | ✅ Yes | Fallback |

---

# Common Event IDs

| Event ID | Description |
|----------|-------------|
| 4624 | Successful Logon |
| 4625 | Failed Logon |
| 4768 | TGT Requested |
| 4769 | Service Ticket Requested |
| 4770 | Service Ticket Renewed |
| 4771 | Kerberos Pre-authentication Failed |
| 4776 | NTLM Credential Validation |

---

# Common Attacks

## Kerberos

- Kerberoasting
- Pass-the-Ticket
- Golden Ticket
- Silver Ticket
- AS-REP Roasting

---

## NTLM

- Pass-the-Hash
- NTLM Relay
- Credential Dumping
- Offline Password Cracking

---

# Quick Summary

| Topic | Description |
|---------|-------------|
| Kerberos | Modern ticket-based authentication protocol |
| AS | Authenticates users and issues TGT |
| TGT | Used to request Service Tickets |
| TGS | Issues Service Tickets after validating TGT |
| Service Ticket | Grants access to a specific service |
| NTLM | Legacy challenge-response authentication |
| SPN | Unique identifier of a service in Active Directory |
| Pass-the-Hash | Uses NT hash instead of password |
| Kerberoasting | Offline cracking of Service Tickets |

---

## Learning Outcome

After completing this section, you should understand:

- Windows authentication methods
- Kerberos authentication workflow
- NTLM authentication process
- Ticket Granting Ticket (TGT)
- Service Tickets
- Authentication Service (AS)
- Ticket Granting Service (TGS)
- Service Principal Names (SPNs)
- Windows authentication event IDs
- Common authentication attacks used in Active Directory environments