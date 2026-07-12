# Day 7 – Windows Security Features (Learning)

## Objective

Learn about the built-in Windows security features that help protect a system from malware, unauthorized access, and other cyber threats. These components are commonly monitored by SOC analysts during security investigations.

---

# 1. Microsoft Defender Antivirus

Microsoft Defender Antivirus is the built-in antivirus solution in Windows.

It detects, prevents, and removes malicious software using:

- Signature-based detection
- Behavior-based detection
- Heuristic analysis
- Cloud-delivered protection

When a threat is detected, Defender can quarantine or remove the malicious file.

## Features

- Real-time protection
- Scheduled scans
- Automatic signature updates
- Cloud protection
- Quarantine infected files
- Protection history

### SOC Importance

SOC analysts monitor Defender alerts to investigate malware infections and suspicious activity on endpoints.

---

# 2. Windows Defender Firewall

Windows Defender Firewall is a **host-based firewall** that monitors and filters inbound and outbound network traffic based on security rules.

It helps prevent unauthorized access to the system by allowing or blocking network connections.

## Features

- Inbound rules
- Outbound rules
- Domain, Private, and Public firewall profiles
- Port filtering
- Application-based rules
- Custom firewall rules

### SOC Importance

Firewall logs help analysts detect unauthorized connections, lateral movement, and suspicious network activity.

---

# 3. Microsoft Defender SmartScreen

Microsoft Defender SmartScreen protects users from phishing websites, malicious downloads, and untrusted applications.

Before allowing access or execution, SmartScreen checks the reputation of:

- Websites
- Downloads
- Applications

against Microsoft's cloud reputation service.

## Features

- Website reputation checking
- Download reputation checking
- Application reputation validation
- Phishing protection

### SOC Importance

SmartScreen helps prevent users from downloading malware or visiting malicious websites, reducing the risk of initial compromise.

---

# 4. PowerShell

PowerShell is Microsoft's command-line shell and scripting language.

It is widely used by administrators and developers to automate tasks, troubleshoot issues, configure systems, and manage Windows locally or remotely.

## Common Uses

- User management
- Service management
- Process management
- System administration
- Automation
- Configuration management

### SOC Importance

PowerShell is frequently abused by attackers for executing malicious scripts, downloading payloads, and performing reconnaissance. SOC analysts monitor PowerShell activity using process creation events (Event ID 4688) and PowerShell Operational Logs.

---

# 5. Task Scheduler

Task Scheduler is a Windows component used to automatically execute programs or scripts based on predefined triggers.

## Common Triggers

- At system startup
- At user logon
- Daily
- Weekly
- On specific events

### Uses

- Windows Updates
- Automated backups
- Maintenance tasks
- Script execution

### SOC Importance

Attackers may create scheduled tasks to maintain persistence after compromising a system. Analysts should monitor newly created or modified scheduled tasks.

---

# 6. Windows Services

A Windows Service is a background process that runs independently of a logged-in user.

Services can start automatically during system boot, manually by a user, or when triggered by another application.

Unlike normal applications, services usually do not have a graphical interface (GUI).

## Startup Types

- Automatic
- Automatic (Delayed Start)
- Manual
- Disabled

## Common Windows Services

| Service | Purpose |
|----------|---------|
| Windows Defender Antivirus Service | Malware protection |
| Windows Update | Installs Windows updates |
| DHCP Client | Obtains IP addresses |
| DNS Client | Resolves domain names |
| Print Spooler | Manages printing |
| Windows Time | Synchronizes system time |

### SOC Importance

Attackers may install malicious services or modify existing services to maintain persistence. A newly installed service often generates **Event ID 7045**.

---

# 7. Basic Hardening Concepts

Windows hardening is the process of reducing the system's attack surface by applying security best practices and minimizing potential vulnerabilities.

## Hardening Best Practices

- Keep Windows updated with the latest security patches.
- Enable Microsoft Defender Antivirus.
- Enable Windows Defender Firewall.
- Use strong passwords.
- Apply the Principle of Least Privilege (PoLP).
- Disable unnecessary services.
- Remove unused user accounts.
- Enable automatic updates.
- Monitor Windows Event Logs.
- Restrict administrative privileges.

### SOC Importance

Proper system hardening reduces the likelihood of successful attacks and limits the impact of security incidents.

---

# Summary

| Feature | Purpose | SOC Importance |
|---------|---------|----------------|
| Microsoft Defender Antivirus | Detects and removes malware | Malware detection and endpoint protection |
| Windows Defender Firewall | Filters inbound and outbound network traffic | Detects unauthorized network activity |
| Microsoft Defender SmartScreen | Blocks phishing websites and malicious downloads | Prevents initial compromise |
| PowerShell | Automates administration and system management | Frequently abused by attackers |
| Task Scheduler | Automates tasks based on triggers | Common persistence technique |
| Windows Services | Runs background processes | Attackers may install or modify services |
| Windows Hardening | Reduces the attack surface | Improves overall system security |

---

# Tools Covered

| Tool | Command |
|------|---------|
| Windows Security | `windowsdefender:` |
| Windows Defender Firewall | `wf.msc` |
| PowerShell | `powershell.exe` |
| Task Scheduler | `taskschd.msc` |
| Services | `services.msc` |
| Event Viewer | `eventvwr.msc` |

---

# Key Takeaways

- Microsoft Defender Antivirus protects Windows from malware using multiple detection techniques.
- Windows Defender Firewall filters network traffic and blocks unauthorized connections.
- Microsoft Defender SmartScreen helps prevent phishing attacks and malicious downloads.
- PowerShell is a powerful administration tool but is also commonly abused by attackers.
- Task Scheduler automates tasks and can be abused for persistence.
- Windows Services run background processes that support the operating system and applications.
- Windows hardening reduces the attack surface and strengthens system security.

---

# Skills Learned

- Windows Security Features
- Microsoft Defender Antivirus
- Windows Defender Firewall
- Microsoft Defender SmartScreen
- PowerShell Basics
- Task Scheduler
- Windows Services
- Windows Hardening Concepts
- SOC Monitoring of Built-in Security Controls