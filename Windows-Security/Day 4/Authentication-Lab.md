# Day 4 – Windows Authentication Lab

## Objective

Gain hands-on experience with Windows authentication by creating users, testing logins, analyzing security logs, and comparing administrator and standard user privileges.

---

# Lab 1 – Create Multiple Local Users

## Objective

Create both a Standard User and an Administrator account.

### Create Standard User

Open **Command Prompt (Run as Administrator)**

```cmd
net user user1 Password@123 /add
```

Create another user:

```cmd
net user user2 Password@123 /add
```

---

### Make user2 an Administrator

```cmd
net localgroup Administrators user2 /add
```

Verify:

```cmd
net localgroup Administrators
```

Expected Output:

```
Administrator
user2
```

---

## Verify Users

```cmd
net user
```

Expected Output:

```
Administrator
DefaultAccount
Guest
user1
user2
WDAGUtilityAccount
```

---

# Lab 2 – Compare User Privileges

## Objective

Understand the difference between Standard User and Administrator accounts.

### Login as user1 (Standard User)

Try the following:

- Install software
- Open Registry Editor (`regedit`)
- Stop a Windows service
- Create a folder inside:

```
C:\Windows
```

Observe:

- UAC prompts appear.
- Administrative actions are denied without admin credentials.

---

### Login as user2 (Administrator)

Repeat the same tasks.

Observe:

- Administrative tasks are allowed after UAC confirmation.
- Greater control over system settings.

---

## Comparison

| Feature | Standard User | Administrator |
|----------|---------------|---------------|
| Install Software | ❌ | ✅ |
| Edit Registry | Limited | ✅ |
| Stop Services | ❌ | ✅ |
| Create Files in C:\Windows | ❌ | ✅ |
| Change System Settings | ❌ | ✅ |

---

# Lab 3 – Successful Login

## Objective

Generate a successful authentication event.

### Steps

1. Sign out.
2. Login using:

```
Username: user1
Password: Password@123
```

Login should succeed.

---

# Lab 4 – Failed Login

## Objective

Generate failed authentication attempts.

### Steps

Login using:

```
Username: user1
Password: WrongPassword
```

Repeat 2–3 times.

Expected Result:

```
Login Failed
```

---

# Lab 5 – Analyze Authentication Logs

## Objective

Observe successful and failed login events in Event Viewer.

### Open Event Viewer

Press:

```
Win + R
```

Type:

```
eventvwr.msc
```

Navigate to:

```
Windows Logs
    └── Security
```

---

## Successful Login

Find:

```
Event ID: 4624
```

Important fields:

- Account Name
- Logon Type
- Security ID (SID)
- Authentication Package (NTLM/Kerberos)
- Logon Process
- Source Network Address (if applicable)

Example:

```
Event ID : 4624

Account Name : user1
Logon Type : 2
Authentication Package : Negotiate
```

---

## Failed Login

Find:

```
Event ID: 4625
```

Important fields:

- Account Name
- Failure Reason
- Status Code
- Logon Type

Example:

```
Failure Reason:
Unknown user name or bad password

Status:
0xC000006D
```

---

# Lab 6 – Observe Logon Types

While viewing Event ID 4624, identify the **Logon Type**.

Common values:

| Logon Type | Meaning |
|------------|----------------------------|
| 2 | Interactive Login |
| 3 | Network Login |
| 4 | Batch |
| 5 | Service |
| 7 | Unlock Workstation |
| 8 | NetworkCleartext |
| 9 | New Credentials |
| 10 | Remote Desktop |
| 11 | Cached Credentials |

---

# Lab 7 – View User SID

Display all user SIDs.

```cmd
wmic useraccount get name,sid
```

Example:

```
Name      SID
user1     S-1-5-21-...
user2     S-1-5-21-...
```

Observe:

- Every account has a unique SID.
- Usernames can change, but SIDs remain the same.

---

# Lab 8 – View Group Membership

Check whether a user is an administrator.

```cmd
net user user1
```

```cmd
net user user2
```

Observe:

```
Local Group Memberships
```

Example:

```
Administrators
Users
```

---

# Summary

During this lab, I successfully:

- Created multiple local users.
- Assigned administrative privileges to one user.
- Compared Standard User and Administrator behavior.
- Performed successful and failed login attempts.
- Analyzed Event Viewer Security logs (Event ID 4624 and 4625).
- Identified different Windows logon types.
- Viewed Security Identifiers (SIDs).
- Verified local group memberships.

---

# Commands Used

```cmd
:: Create User
net user user1 Password@123 /add

net user user2 Password@123 /add

:: Add Administrator
net localgroup Administrators user2 /add

:: View Users
net user

:: View User Details
net user user1

:: View Administrator Group
net localgroup Administrators

:: View SIDs
wmic useraccount get name,sid

:: Open Event Viewer
eventvwr.msc
```

---

# Skills Practiced

- Local User Management
- Administrator vs Standard User
- Windows Authentication
- Event Viewer Analysis
- Security Event IDs (4624 & 4625)
- Security Identifiers (SIDs)
- Windows Logon Types