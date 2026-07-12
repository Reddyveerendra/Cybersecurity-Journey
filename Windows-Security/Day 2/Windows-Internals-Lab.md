# Windows Fundamentals Lab – Day 2

## Objective

This lab focuses on practicing essential Windows administration tasks commonly used by System Administrators, SOC Analysts, and Security Engineers.

---

# Lab 1 – Create Users and Understand Privileges

## Create a Local User

```cmd
net user username Password@123 /add
```

### Example

```cmd
net user socuser Password@123 /add
```

---

## Add User to the Administrators Group

```cmd
net localgroup Administrators username /add
```

### Example

```cmd
net localgroup Administrators socuser /add
```

### Verification

```cmd
net user socuser
```

### What I Learned

* Created a local Windows user using the Command Prompt.
* Added the user to the local **Administrators** group.
* Understood the difference between a standard user and an administrator account.

---

# Lab 2 – Identify Users Using Security Identifiers (SID)

## Command

```cmd
wmic useraccount where sid="SID_Number" get name
```

### Example

```cmd
wmic useraccount where sid="S-1-5-20" get name
```

### Common Built-in SIDs

| SID      | Account        | Purpose                                                                                                     |
| -------- | -------------- | ----------------------------------------------------------------------------------------------------------- |
| S-1-5-18 | LocalSystem    | Highest local privileges. Used by Windows services that require complete control over the operating system. |
| S-1-5-19 | LocalService   | Limited local privileges. Used by services that need minimal permissions.                                   |
| S-1-5-20 | NetworkService | Limited local privileges but authenticates to remote systems using the computer account.                    |

### Why This Matters

SOC analysts frequently encounter SIDs in:

* Windows Event Logs
* Security logs
* Incident investigations
* Malware analysis

Being able to map a SID to its corresponding account helps identify which user or service performed an action.

---

# Lab 3 – Understanding NTFS Permissions

NTFS permissions control who can access files and folders and what actions they are allowed to perform.

| Permission           | Description                                                    |
| -------------------- | -------------------------------------------------------------- |
| Full Control         | Complete access, including changing permissions and ownership. |
| Modify               | Read, write, edit, and delete files or folders.                |
| Read & Execute       | Open and execute files or applications.                        |
| List Folder Contents | View the contents of a folder (folders only).                  |
| Read                 | View files, folders, and their properties.                     |
| Write                | Create new files or folders and modify existing content.       |

### What I Learned

* NTFS permissions provide granular access control.
* Administrators can assign permissions based on user roles.
* Proper permission management helps protect sensitive data.

---

# Lab 4 – Create a Scheduled Task

## Open Task Scheduler

```cmd
taskschd.msc
```

---

## Create a Scheduled Task Using Command Line

```cmd
schtasks /create /tn "SOC Lab Test" /tr "notepad.exe" /sc once /st 23:59
```

### Parameters

| Parameter | Description                   |
| --------- | ----------------------------- |
| /create   | Creates a new scheduled task  |
| /tn       | Task name                     |
| /tr       | Program or command to execute |
| /sc       | Schedule type                 |
| /st       | Start time                    |

### Verification

```cmd
schtasks /query
```

### What I Learned

* Windows Task Scheduler automates repetitive tasks.
* Scheduled tasks are commonly used by administrators for maintenance and backups.
* Attackers also abuse scheduled tasks for persistence, making them important to monitor during security investigations.

---

# Lab 5 – Windows Registry Hives

The Windows Registry stores configuration settings for the operating system, applications, hardware, and user profiles.

| Registry Hive              | Purpose                                                             |
| -------------------------- | ------------------------------------------------------------------- |
| HKEY_CLASSES_ROOT (HKCR)   | Stores file associations and COM object information.                |
| HKEY_CURRENT_USER (HKCU)   | Contains settings specific to the currently logged-in user.         |
| HKEY_LOCAL_MACHINE (HKLM)  | Stores system-wide hardware, software, and security configurations. |
| HKEY_USERS (HKU)           | Contains profiles and settings for all user accounts on the system. |
| HKEY_CURRENT_CONFIG (HKCC) | Stores hardware configuration currently in use.                     |

### Why the Registry Is Important

* Stores Windows configuration data.
* Used by applications to save settings.
* Frequently targeted by malware to establish persistence.
* Valuable source of forensic artifacts during incident investigations.

---

# Key Takeaways

* Created and managed local Windows users.
* Understood administrator privileges.
* Learned how Security Identifiers (SIDs) map to Windows accounts.
* Explored NTFS permissions for file and folder security.
* Created scheduled tasks using both the GUI and command line.
* Studied the five primary Windows Registry hives and their security relevance.

---

# Skills Practiced

* Windows Administration
* User and Group Management
* NTFS Permissions
* Security Identifiers (SID)
* Task Scheduler
* Windows Registry
* Windows Command Line
* SOC Fundamentals
* Blue Team Basics

---

**Tools Used**

* Command Prompt (CMD)
* Local Users and Groups (`lusrmgr.msc`)
* Task Scheduler (`taskschd.msc`)
* Windows Registry Editor (`regedit`)
* File Explorer

---

**Author**

Veerendra Reddy

Windows Security Learning Journey – Day 2
