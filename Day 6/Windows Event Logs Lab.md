# Day 6 – Windows Event Logs Lab

## Objective

Generate common Windows Security Events and analyze them using Event Viewer. This lab focuses on understanding how user actions are recorded and why these events are important for security monitoring.

---

# Lab 1 – Successful Logon (Event ID 4624)

## Objective

Generate a successful login event.

### Steps

1. Sign out of Windows.
2. Log in with a valid user account.
3. Open **Event Viewer** (`eventvwr.msc`).
4. Navigate to:

```
Windows Logs
    └── Security
```

5. Filter or search for **Event ID 4624**.

### Record

| Field | Value |
|-------|-------|
| Event ID | 4624 |
| Description | Successful account logon |
| Screenshot | *(Insert Screenshot Here)* |
| Security Significance | Confirms that a user successfully authenticated. Used to track login activity and detect unauthorized access. |

---

# Lab 2 – Failed Logon (Event ID 4625)

## Objective

Generate a failed login event.

### Steps

1. Sign out of Windows.
2. Attempt to log in using an incorrect password.
3. Repeat 2–3 times.
4. Open **Event Viewer**.
5. Locate **Event ID 4625**.

### Record

| Field | Value |
|-------|-------|
| Event ID | 4625 |
| Description | Failed account logon |
| Security Significance | Multiple failed logons may indicate brute-force attacks, password spraying, or unauthorized access attempts. |

---

# Lab 3 – Create a New User (Event ID 4720)

## Objective

Create a new local user account and observe the corresponding event.

### Create User

Open **Command Prompt (Run as Administrator)**

```cmd
net user labuser Password@123 /add
```

Open **Event Viewer** and locate **Event ID 4720**.

### Record

| Field | Value |
|-------|-------|
| Event ID | 4720 |
| Description | A user account was created |
| Screenshot | *(Insert Screenshot Here)* |
| Security Significance | Detects creation of new user accounts. Unexpected account creation can indicate persistence by an attacker. |

---

# Lab 4 – Add User to Administrators Group (Event ID 4732)

## Objective

Add a user to the local Administrators group.

### Command

```cmd
net localgroup Administrators labuser /add
```

Open **Event Viewer** and locate **Event ID 4732**.

### Record

| Field | Value |
|-------|-------|
| Event ID | 4732 |
| Description | A member was added to a security-enabled local group |
| Screenshot | *(Insert Screenshot Here)* |
| Security Significance | Indicates privilege escalation. Attackers often add accounts to the Administrators group to gain elevated access. |

---

# Lab 5 – Launch Programs (Event ID 4688)

## Objective

Generate process creation events.

### Launch Programs

Open several applications, such as:

- Command Prompt (`cmd.exe`)
- Notepad (`notepad.exe`)
- Calculator (`calc.exe`)
- PowerShell (`powershell.exe`)

Open **Event Viewer** and locate **Event ID 4688**.

> **Note:** If Event ID 4688 is not generated, enable **Audit Process Creation** in Local Security Policy (`secpol.msc`) under **Advanced Audit Policy Configuration → Detailed Tracking → Audit Process Creation**.

### Record

| Field | Value |
|-------|-------|
| Event ID | 4688 |
| Description | A new process has been created |
| Screenshot | *(Insert Screenshot Here)* |
| Security Significance | Helps detect suspicious process execution such as PowerShell abuse, command shells, or malware. |

---

# Event Summary

| Event ID | Description | Log Location | Security Significance |
|-----------|-------------|--------------|-----------------------|
| 4624 | Successful Logon | Security | Tracks successful user authentication and login activity. |
| 4625 | Failed Logon | Security | Detects failed authentication attempts and possible brute-force attacks. |
| 4720 | User Account Created | Security | Monitors creation of new accounts for persistence or unauthorized access. |
| 4732 | User Added to Local Group | Security | Detects privilege escalation through group membership changes. |
| 4688 | Process Created | Security | Monitors execution of applications and suspicious processes. |

---

# Tools Used

- Event Viewer (`eventvwr.msc`)
- Command Prompt
- Local Users and Groups (`lusrmgr.msc`)
- Local Security Policy (`secpol.msc`)

---

# Commands Used

```cmd
:: Create User
net user labuser Password@123 /add

:: Add User to Administrators
net localgroup Administrators labuser /add

:: Launch Programs
cmd.exe
notepad.exe
powershell.exe
calc.exe

:: Open Event Viewer
eventvwr.msc
```

---

# Skills Practiced

- Generated Windows Security Events
- Investigated Security Logs in Event Viewer
- Created and Managed Local User Accounts
- Modified Local Group Memberships
- Analyzed Process Creation Events
- Understood the Security Significance of Common Event IDs

---

# Conclusion

In this lab, I generated and analyzed five important Windows Security Event IDs commonly monitored by SOC analysts. By correlating user actions with the resulting event logs, I gained practical experience in identifying authentication events, account management activities, privilege changes, and process creation. These events form the basis of detection rules in SIEM platforms such as **Microsoft Sentinel**, **Splunk**, and **Microsoft Defender XDR**.