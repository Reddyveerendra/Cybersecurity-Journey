````markdown
# Windows Fundamentals for SOC Analysts - Day 1

## Overview

This document contains my Day 1 learning notes while preparing for SOC Analyst and Microsoft Sentinel. It covers Windows fundamentals from a security perspective, focusing on concepts that are commonly used during threat hunting, incident response, and log analysis.

---

# 1. Windows Operating System

Windows is an operating system developed by Microsoft that acts as a bridge between hardware and software. It manages system resources such as CPU, memory, storage, devices, files, and processes while providing a Graphical User Interface (GUI) that allows users to interact with the computer easily.

## Windows Editions

### Home
- Designed for personal users.

### Pro
- BitLocker
- Hyper-V
- Remote Desktop
- Group Policy

### Windows Server
- Active Directory
- DNS
- DHCP
- File Server
- IIS
- Virtualization

## Why Windows Dominates Enterprise

- Large software ecosystem
- Active Directory integration
- Strong security features
- Microsoft Azure integration
- Continuous security updates

---

# 2. Windows Architecture

Windows architecture is divided into two execution modes.

## User Mode

User Mode is where applications execute. It provides isolation so applications cannot directly access critical system resources, improving stability and security.

### Examples

- Web Browsers
- Microsoft Office
- Games
- Notepad

### Common DLLs

- kernel32.dll
- user32.dll
- advapi32.dll
- ntdll.dll

---

## System Calls

Applications running in User Mode cannot directly access hardware or kernel resources.

Instead, they use **System Calls** to request services from the Windows Kernel.

---

## Kernel Mode

Kernel Mode has full access to hardware and system memory.

It manages:

- Device Drivers
- Memory Manager
- CPU Scheduler
- Security Reference Monitor
- Hardware Abstraction Layer (HAL)
- Windows Kernel (ntoskrnl.exe)

```
        User Mode
--------------------------
Applications
Browser
Office
Games

System DLLs
kernel32.dll
user32.dll
advapi32.dll
ntdll.dll

↓

System Calls

↓

Kernel Mode
--------------------------
ntoskrnl.exe
HAL
Drivers
Memory Manager
Scheduler
Security Reference Monitor
```

### Important Concepts

### Process

A running instance of a program.

Examples:
- chrome.exe
- powershell.exe
- notepad.exe

Security Perspective:
- Parent-Child relationships
- Process Injection
- Malware execution

---

### Thread

A Thread is the smallest unit of execution inside a process.

One process can have multiple threads executing simultaneously.

Security Perspective:
- Remote Thread Injection
- Malware code injection

---

### Paging

Paging is a memory management technique that moves inactive memory pages between RAM and disk to optimize memory usage.

---

# 3. Windows Boot Process

```
Power On

↓

UEFI / BIOS

↓

Secure Boot

↓

Windows Boot Manager (bootmgr)

↓

BCD
Boot Configuration Data

↓

winload.exe

↓

ntoskrnl.exe

↓

HAL

↓

Drivers

↓

smss.exe

↓

wininit.exe

↓

services.exe
lsass.exe

↓

winlogon.exe

↓

explorer.exe
```

## Important Components

### UEFI

Modern firmware that initializes hardware and supports Secure Boot.

### Secure Boot

Prevents unauthorized bootloaders and malicious code from executing during startup.

### bootmgr

Windows Boot Manager.

Reads the Boot Configuration Data (BCD).

### BCD

Stores Windows boot configuration information.

### winload.exe

Loads:

- Windows Kernel
- HAL
- Boot Drivers

into memory.

---

# 4. Windows Components (SOC Perspective)

| Component | SOC Perspective |
|-----------|-----------------|
| Process | Malware execution, PPID anomalies |
| Services | Persistence |
| Registry | Startup persistence (Run Keys, Services) |
| NTFS | Ransomware, Alternate Data Streams |
| Event Viewer | Log investigation |
| Task Scheduler | Malware persistence |
| WMI | Lateral Movement, Persistence |
| PowerShell | Living Off The Land (LOLBins) |
| Drivers | Rootkits |

---

# 5. Windows Process Tree

```
System
 └── smss.exe
      └── wininit.exe
           ├── services.exe
           │    └── svchost.exe (-k)
           ├── lsass.exe
           └── winlogon.exe
                └── userinit.exe
                     └── explorer.exe
```

## Important Processes

### smss.exe

Session Manager

- Creates user sessions
- Starts wininit.exe

---

### wininit.exe

Windows Initialization Process

Starts:

- services.exe
- lsass.exe

---

### services.exe

Service Control Manager

Responsible for:

- Starting Windows Services
- Managing Service Lifecycle

Security Checks

- Parent should be **wininit.exe**
- Path should be **System32**
- Multiple services.exe processes are suspicious

---

### svchost.exe

Hosts Windows Services.

Expected:

- Parent: services.exe
- Command Line includes **-k**
- Path: System32

Suspicious Indicators

- Missing **-k**
- Wrong Parent Process
- Running from Temp/AppData

---

### lsass.exe

Local Security Authority Subsystem Service

Responsibilities

- Authentication
- Kerberos
- NTLM
- Credential Management

Attack Examples

- Credential Dumping
- Pass-the-Hash
- Mimikatz

---

### winlogon.exe

Responsible for user logon.

Suspicious Activity

- Missing expected command-line arguments
- Spawning PowerShell or cmd.exe
- Running outside System32

---

### explorer.exe

Windows Desktop Shell.

Usually launched by userinit.exe.

---

# 6. Environment Variables

## %TEMP%

Temporary files.

Security Perspective

- Malware frequently executes from this location.

---

## %APPDATA%

Stores application settings.

Security Perspective

- Common malware persistence location.

---

## %ProgramData%

Stores shared application data.

Security Perspective

- Frequently abused by attackers to hide payloads.

---

# 7. Common Windows Event IDs

| Event ID | Description | SOC Use Case |
|----------|-------------|--------------|
| 4624 | Successful Logon | Monitor user logins |
| 4625 | Failed Logon | Detect brute-force attacks |
| 4688 | Process Created | Detect suspicious process execution |
| 4689 | Process Terminated | Timeline investigations |
| 4720 | User Account Created | Detect unauthorized account creation |
| 4726 | User Account Deleted | Detect attacker cleanup |
| 4732 | User Added to Local Group | Detect privilege escalation |

---

# 8. Windows File System Structure

| Folder | Purpose | SOC Perspective |
|---------|---------|----------------|
| C:\Windows | Windows Operating System Files | Core OS files |
| C:\Windows\System32 | System Executables and DLLs | Verify process legitimacy |
| C:\Windows\SysWOW64 | 32-bit binaries on 64-bit Windows | Validate application architecture |
| C:\ProgramData | Shared application data | Malware hiding location |
| C:\Users | User Profiles | Investigate user activity |
| C:\Windows\Temp / %TEMP% | Temporary files | Common malware execution path |


````
