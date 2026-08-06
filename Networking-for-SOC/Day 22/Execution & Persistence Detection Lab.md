# Day 6 – Execution & Persistence Detection Lab

## Objective

Generate safe Windows activities in a lab environment and learn how to detect them using:

- Windows Security Logs
- Task Scheduler Logs
- Service Control Manager Logs
- Microsoft Sentinel (KQL)

---

# MITRE ATT&CK Mapping

| Technique | ATT&CK ID |
|-----------|-----------|
| PowerShell | T1059.001 |
| Windows Command Shell | T1059.003 |
| Scheduled Task | T1053.005 |
| Windows Service | T1543.003 |
| Event Log Clearing | T1070.001 |

---

# Lab 1 – PowerShell Execution

## MITRE

**T1059.001 – PowerShell**

## Execute

```powershell
Get-Process
```

or

```powershell
Get-Service
```

---

## Detection

### Event Viewer

Security Log

**Event ID**

- 4688 – Process Creation

Look for

```
powershell.exe
```

If Sysmon is installed

- Event ID 1

---

## Microsoft Sentinel KQL

```kusto
SecurityEvent
| where EventID == 4688
| where NewProcessName has "powershell.exe"
| project TimeGenerated, Computer, Account, NewProcessName, ProcessCommandLine, ParentProcessName
```

---

# Lab 2 – Windows Command Shell

## MITRE

**T1059.003 – Windows Command Shell**

## Execute

```cmd
whoami
```

or

```cmd
ipconfig
```

---

## Detection

Security Log

- Event ID 4688

Look for

```
cmd.exe
```

Sysmon

- Event ID 1

---

## Microsoft Sentinel KQL

```kusto
SecurityEvent
| where EventID == 4688
| where NewProcessName endswith "cmd.exe"
| project TimeGenerated, Computer, Account, ProcessCommandLine, ParentProcessName
```

---

# Lab 3 – Scheduled Task

## MITRE

**T1053.005 – Scheduled Task**

## Execute

```cmd
schtasks /Create /TN TestTask /TR notepad.exe /SC ONCE /ST 23:59
```

Verify

```cmd
schtasks /Query /TN TestTask
```

Delete

```cmd
schtasks /Delete /TN TestTask /F
```

---

## Detection

### Security Log

- 4698 – Task Created
- 4699 – Task Deleted
- 4702 – Task Updated

*(Requires auditing to be enabled.)*

### Process Creation

Event ID 4688

Look for

```
schtasks.exe
```

### Task Scheduler Operational Log

```
Applications and Services Logs
└── Microsoft
    └── Windows
        └── TaskScheduler
            └── Operational
```

Common Events

- 106 – Task Registered
- 140 – Task Updated
- 141 – Task Deleted

---

## Microsoft Sentinel KQL

```kusto
SecurityEvent
| where EventID == 4688
| where NewProcessName endswith "schtasks.exe"
| project TimeGenerated, Computer, Account, ProcessCommandLine, ParentProcessName
```

---

# Lab 4 – Windows Service

## MITRE

**T1543.003 – Windows Service**

## Execute

```cmd
sc create TestService binPath= "C:\Windows\System32\cmd.exe /c exit"
```

Verify

```cmd
sc query TestService
```

Delete

```cmd
sc delete TestService
```

---

## Detection

### System Log

Service Control Manager

- Event ID 7045 – Service Installed
- Event ID 7036 – Service Started / Stopped

### Security Log

- Event ID 4697 (if enabled)

### Process Creation

Event ID 4688

Look for

```
sc.exe
```

---

## Microsoft Sentinel KQL

```kusto
SecurityEvent
| where EventID == 7045
| project TimeGenerated, Computer, Account, ServiceName, ImagePath
```

---

# Lab 5 – Event Log Clearing

## MITRE

**T1070.001 – Clear Windows Event Logs**

## Execute

Clear Application Log

```cmd
wevtutil cl Application
```

or

```powershell
Clear-EventLog -LogName Application
```

---

## Detection

### Security Log

- Event ID 1102

*(Only when the Security log itself is cleared.)*

### Process Creation

Event ID 4688

Look for

```
wevtutil.exe
```

or

```
powershell.exe
```

---

## Microsoft Sentinel KQL

```kusto
SecurityEvent
| where EventID == 1102
| project TimeGenerated, Computer, Account
```

Detect use of **wevtutil.exe**

```kusto
SecurityEvent
| where EventID == 4688
| where NewProcessName endswith "wevtutil.exe"
| project TimeGenerated, Computer, Account, ProcessCommandLine
```

---

# Summary

| Activity | MITRE | Windows Event | Detect Process |
|----------|--------|---------------|----------------|
| PowerShell | T1059.001 | 4688 | powershell.exe |
| Command Prompt | T1059.003 | 4688 | cmd.exe |
| Scheduled Task | T1053.005 | 4698 / 4699 / 4702 | schtasks.exe |
| Windows Service | T1543.003 | 7045 / 4697 | sc.exe |
| Event Log Clearing | T1070.001 | 1102 | wevtutil.exe |

---

# SOC Takeaways

- Monitor **4688** to identify process execution.
- Enable auditing for scheduled task creation to capture **4698**.
- Watch for **7045** to detect newly installed services.
- Treat **1102 (Security Log Cleared)** as a high-severity event.
- Use command-line logging and Sysmon to improve visibility into attacker activity.