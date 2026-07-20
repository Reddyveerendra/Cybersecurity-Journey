# Day 8 – Active Directory Security Lab

## Objective

Practice investigating common Active Directory security events and understand how they relate to real-world SOC investigations.

---

# 1. Audit Privileged Groups

### Objective

Identify users who have administrative privileges in the domain.

### Steps

```
Server Manager
    ↓
Tools
    ↓
Active Directory Users and Computers
    ↓
Domain
    ↓
Built-in / Users
```

Review the following privileged groups:

- Domain Admins
- Enterprise Admins
- Schema Admins
- Administrators
- Backup Operators
- Server Operators
- Account Operators

Check:

- Members of each group
- Recently added users
- Unexpected privileged accounts

### Related Event IDs

| Event ID | Description |
|----------|-------------|
| 4728 | User added to Global Security Group |
| 4729 | User removed from Global Security Group |
| 4732 | User added to Local Security Group |
| 4733 | User removed from Local Security Group |

---

# 2. Review Authentication Logs

### Open

```
Event Viewer
    ↓
Windows Logs
    ↓
Security
```

Review these Event IDs:

| Event ID | Description |
|----------|-------------|
| 4624 | Successful Logon |
| 4625 | Failed Logon |
| 4634 | Logoff |
| 4672 | Privileged Logon |
| 4768 | Kerberos TGT Request |
| 4769 | Kerberos Service Ticket (TGS) Request |
| 4776 | NTLM Authentication |

### Common Logon Types

| Logon Type | Meaning |
|------------|---------|
| 2 | Interactive Logon |
| 3 | Network Logon |
| 5 | Service Logon |
| 7 | Unlock Workstation |
| 10 | Remote Desktop (RDP) |
| 11 | Cached Interactive Logon |

---

# 3. Observe Account Lockouts

Generate an account lockout by entering an incorrect password multiple times.

Review:

| Event ID | Description |
|----------|-------------|
| 4625 | Failed Logon |
| 4740 | User Account Locked Out |

Possible causes:

- User entered an incorrect password multiple times.
- Scheduled Task using an old password.
- Windows Service running with outdated credentials.
- Cached credentials after a password reset.
- Mapped network drives using old credentials.
- Malware repeatedly attempting authentication.
- Password spraying attack.
- Brute-force attack.

During investigation, verify:

- Caller Computer Name
- Source IP Address
- Username
- Time of lockout
- Authentication method

---

# 4. Document How Group Policy Affects Users

Apply the latest Group Policy.

```powershell
gpupdate /force
```

Generate a Group Policy report.

```powershell
gpresult /h report.html
```

Review:

- Applied GPOs
- Denied GPOs
- Password Policy
- Account Lockout Policy
- Security Filtering
- User Configuration
- Computer Configuration

Observe how the applied policies affect user logins and system behavior.

---

# 5. Map Activities to MITRE ATT&CK

| Lab Activity | MITRE Technique | Technique ID |
|---------------|----------------|--------------|
| Failed Logins | Brute Force | T1110 |
| Privileged Group Enumeration | Permission Groups Discovery | T1069 |
| User Enumeration | Account Discovery | T1087 |
| Domain Admin Login | Valid Accounts | T1078 |
| Kerberos TGT Request | Steal or Forge Kerberos Tickets | T1558 |
| Kerberoasting | Kerberoasting | T1558.003 |
| NTLM Authentication | Use Alternate Authentication Material | T1550 |
| Group Policy Changes | Modify Authentication Process | T1556 |

---

# Lab Summary

After completing this lab, you should be able to:

- Audit privileged Active Directory groups.
- Investigate successful and failed authentication events.
- Analyze account lockouts and determine their root cause.
- Verify the impact of Group Policy on users and computers.
- Correlate Windows security events with MITRE ATT&CK techniques.