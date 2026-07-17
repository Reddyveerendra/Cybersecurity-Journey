# Day 4 – Authentication Lab

## Objective

- Join a Windows client to an Active Directory domain.
- Log in using domain accounts.
- Observe Kerberos authentication.
- Compare domain authentication with local authentication.

---

# Lab Environment

| Machine | Role | IP Address |
|----------|------|------------|
| DC01 | Windows Server 2022 Domain Controller | 192.168.10.10 |
| Windows 7 | Domain Client | 192.168.10.20 |
| Domain | lab.local | - |

---

# Prerequisites

Before starting, ensure:

- Windows Server is already promoted to a **Domain Controller**.
- Active Directory Domain Services (AD DS) is installed.
- Domain **lab.local** is created.
- Windows 7 client is connected to the same network.
- Both machines can communicate with each other.

Verify the Domain Controller:

```powershell
Get-WindowsFeature AD-Domain-Services
```

Verify the domain:

```cmd
echo %USERDNSDOMAIN%
```

Expected Output:

```
lab.local
```

---

# Step 1 – Configure Static Network

## Domain Controller (DC01)

Configure a static IP address.

```
IP Address : 192.168.10.10
Subnet Mask: 255.255.255.0
Gateway    : 192.168.10.1
DNS Server : 127.0.0.1
```

Verify:

```cmd
ipconfig /all
```

---

## Windows 7 Client

Configure the network settings.

```
IP Address : 192.168.10.20
Subnet Mask: 255.255.255.0
Gateway    : 192.168.10.1
DNS Server : 192.168.10.10
```

Verify connectivity.

```cmd
ping 192.168.10.10
```

Verify DNS resolution.

```cmd
nslookup lab.local
```

---

# Step 2 – Create a Domain User

On **DC01**

```
Server Manager
    └── Tools
            └── Active Directory Users and Computers
```

Navigate to:

```
lab.local
    └── Users
```

Right Click

```
New
    └── User
```

Provide:

- First Name
- Last Name
- User Logon Name
- Password

Click **Next** and **Finish**.

Example:

```
User Name : john.doe
Password  : ********
```

---

# Step 3 – Join Windows 7 to the Domain

Open:

```
System Properties
    └── Computer Name
            └── Change
```

Select:

```
Domain
```

Enter:

```
lab.local
```

Provide the Domain Administrator credentials.

Restart the Windows 7 machine after successfully joining the domain.

---

# Step 4 – Log In with a Domain Account

Log in using:

```
LAB\john.doe
```

or

```
john.doe@lab.local
```

Verify:

```cmd
whoami
```

Expected Output:

```
lab\john.doe
```

---

# Step 5 – Verify Kerberos Authentication

Display cached Kerberos tickets.

```cmd
klist
```

Expected Output:

```
krbtgt/LAB.LOCAL
```

This confirms that the user successfully authenticated using Kerberos and received a **Ticket Granting Ticket (TGT)**.

---

# Step 6 – Observe Kerberos Events

On **DC01**

```
Event Viewer
    └── Windows Logs
            └── Security
```

Observe the following Kerberos Event IDs.

| Event ID | Description |
|----------|-------------|
| 4768 | Kerberos Authentication Ticket (TGT) Requested |
| 4769 | Kerberos Service Ticket Requested |
| 4770 | Kerberos Service Ticket Renewed |
| 4771 | Kerberos Pre-authentication Failed |

> **Note:** During this lab, Kerberos tickets were successfully generated (`klist`), but Event IDs 4768 and 4769 were not present because Kerberos auditing was not enabled on the Domain Controller.

---

# Step 7 – Compare Domain vs Local Login

| Domain Login | Local Login |
|---------------|------------|
| Uses Active Directory | Uses Local SAM Database |
| Authenticates with Domain Controller | Authenticates Locally |
| Uses Kerberos | Uses Local Authentication |
| Receives Kerberos Tickets | No Kerberos Tickets |
| Can Access Domain Resources | Limited to Local Machine |

---

# Understanding Authentication Logs

## Domain Controller (DC01)

The Domain Controller records authentication events for **all domain users**.

Examples:

- User logon
- Kerberos Ticket Requests
- Failed Domain Logins
- Password Validation

Common Event IDs:

- 4768
- 4769
- 4770
- 4771

---

## Windows Client

The client machine records authentication events that occur **only on that computer**.

Examples:

- Successful Logon (4624)
- Failed Logon (4625)
- Logoff (4634)
- Account Logon Events

The client does **not** contain authentication logs for users logging into other computers.

---

# Key Learnings

- Configured static IP addresses for both Domain Controller and Windows 7 client.
- Verified DNS resolution using the Domain Controller.
- Created domain users using Active Directory Users and Computers.
- Successfully joined Windows 7 to the Active Directory domain.
- Logged in using a domain account.
- Verified Kerberos authentication using `klist`.
- Learned the purpose of Kerberos Event IDs (4768, 4769, 4770, and 4771).
- Compared domain authentication with local authentication.
- Understood the difference between authentication logs stored on the Domain Controller and the Windows client.

---

# Conclusion

In this lab, I successfully configured a Windows client to join an Active Directory domain, authenticated using a domain account, verified Kerberos authentication with `klist`, and explored Kerberos-related Event IDs. I also learned how authentication logs differ between the Domain Controller and the client machine, providing a strong foundation for investigating Windows authentication events as a SOC Analyst.