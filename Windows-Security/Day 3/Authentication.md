# Day 3 – Windows Authentication

## Objective

Understand how Windows verifies a user's identity and grants access to system resources.

---

# 1. Authentication vs. Authorization

## Authentication

Authentication verifies **who the user is**.

Windows checks the user's credentials (username, password, smart card, Windows Hello, etc.) before allowing them to log in.

Example:

```
Username: Administrator
Password: ********
```

If the credentials are correct, the user is authenticated.

---

## Authorization

Authorization determines **what an authenticated user is allowed to access**.

After successful authentication, Windows checks:

- User permissions
- Group memberships
- NTFS permissions
- Share permissions
- User privileges

Example:

A standard user can read a folder but cannot delete it.

---

## Difference

| Authentication | Authorization |
|---------------|---------------|
| Verifies identity | Determines permissions |
| Happens first | Happens after authentication |
| Username & Password | ACLs, NTFS Permissions, Group Membership |
| "Who are you?" | "What are you allowed to do?" |

---

# 2. Local Accounts

A **local account** exists only on the local Windows machine.

Its credentials are stored locally and are not managed by Active Directory.

## Types of Local Accounts

- Standard User
- Administrator
- Guest (Disabled by default)

---

## Security Account Manager (SAM)

Windows stores local account information inside the **Security Account Manager (SAM)** database.

Location:

```
C:\Windows\System32\Config\SAM
```

The SAM database stores:

- Username
- Security Identifier (SID)
- Password Hash (Not the actual password)
- Group Membership

> Note:
> The SAM file is protected and cannot be accessed while Windows is running.

---

## Create a Local User

```cmd
net user username Password@123 /add
```

Example

```cmd
net user labuser Password@123 /add
```

---

## Delete a Local User

```cmd
net user username /delete
```

Example

```cmd
net user labuser /delete
```

---

# 3. Security Identifiers (SIDs)

A **Security Identifier (SID)** is a unique value assigned to every user, group, or computer.

Windows uses the SID internally instead of the username.

Even if a username changes, the SID remains the same.

---

## Common Windows SIDs

| SID | Account |
|------|----------|
| S-1-5-18 | LocalSystem |
| S-1-5-19 | Local Service |
| S-1-5-20 | Network Service |
| S-1-5-32-544 | Administrators Group |
| S-1-5-32-545 | Users Group |

---

## Find SID

```cmd
wmic useraccount get name,sid
```

Or

```powershell
Get-LocalUser
```

---

# 4. NTLM Authentication

NTLM (NT LAN Manager) is Microsoft's older authentication protocol.

It uses a **challenge-response** mechanism instead of sending passwords over the network.

## NTLM Authentication Flow

```
Client
   │
   │ Username
   ▼
Server
   │
   │ Sends Random Challenge
   ▼
Client
   │
   │ Encrypts Challenge using Password Hash
   ▼
Server
   │
   │ Verifies Response
   ▼
Authentication Success
```

### Steps

1. User enters username and password.
2. Windows creates a password hash.
3. The server sends a random challenge.
4. The client encrypts the challenge using the password hash.
5. The server validates the response.
6. Access is granted if the response matches.

---

## NTLM Limitations

- Older protocol
- No mutual authentication
- Slower than Kerberos
- Vulnerable to Pass-the-Hash attacks

---

# 5. Kerberos Authentication

Kerberos is the default authentication protocol in Active Directory environments.

Instead of repeatedly sending passwords, it uses **tickets**.

---

## Components

- Client
- Domain Controller (KDC)
- Ticket Granting Ticket (TGT)
- Service Ticket
- Resource Server 

---

## Kerberos Flow

```
User Login
      │
      ▼
Authenticate with Domain Controller
      │
      ▼
Receive Ticket Granting Ticket (TGT)
      │
      ▼
Request Service Ticket
      │
      ▼
Receive Service Ticket
      │
      ▼
Access File Server / SQL Server / Other Services
```

---

## Advantages

- Faster than NTLM
- Mutual authentication
- More secure
- Supports Single Sign-On (SSO)

---

# NTLM vs Kerberos

| NTLM | Kerberos |
|------|-----------|
| Older protocol | Modern protocol |
| Uses challenge-response | Uses tickets |
| No mutual authentication | Mutual authentication |
| Slower | Faster |
| Workgroup & Legacy | Active Directory |

---

# 6. User Account Control (UAC)

User Account Control (UAC) helps prevent unauthorized changes to Windows.

Even if a user belongs to the Administrators group, applications run with standard user privileges by default.

Administrative privileges are granted only after user approval.

Example:

When installing software, Windows displays:

```
Do you want to allow this app
to make changes to your device?
```

This is the UAC prompt.

---

## Least Privileged Access (LPA)

LPA (Least Privileged Access) is the security principle of giving users only the permissions they need to perform their tasks.

Benefits:

- Reduces malware impact
- Limits unauthorized changes
- Improves overall security

---

# 7. Windows Logon Types

Windows records different logon types in the Security Event Logs.

| Logon Type | Meaning |
|------------|------------------------------|
| 2 | Interactive (Keyboard Login) |
| 3 | Network (SMB, Shared Folder) |
| 4 | Batch (Scheduled Task) |
| 5 | Service |
| 7 | Unlock Workstation |
| 8 | NetworkCleartext |
| 9 | New Credentials (RunAs) |
| 10 | Remote Desktop (RDP) |
| 11 | Cached Credentials |

---

## View Logon Events

Open:

```
Event Viewer
```

Navigate to:

```
Windows Logs
    └── Security
```

Look for:

```
Event ID 4624
```

This event records successful logon attempts and includes the logon type.

---

# Key Takeaways

- Authentication verifies a user's identity.
- Authorization determines what resources a user can access.
- Local accounts are stored in the SAM database.
- Every Windows account has a unique SID.
- NTLM uses a challenge-response authentication mechanism.
- Kerberos uses tickets and is the default protocol in Active Directory.
- UAC helps prevent unauthorized system changes.
- Windows logs different logon types for auditing and security monitoring.

---

# Commands Practiced

```cmd
:: Create User
net user username Password@123 /add

:: Delete User
net user username /delete

:: View User SID
wmic useraccount get name,sid

:: Open Event Viewer
eventvwr.msc
```

---

# Skills Learned

- Authentication vs Authorization
- Local Account Management
- Security Identifiers (SIDs)
- NTLM Authentication
- Kerberos Authentication
- User Account Control (UAC)
- Windows Logon Types
- Event Viewer Log Analysis