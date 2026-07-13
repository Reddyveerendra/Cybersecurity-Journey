# Day 2 – Active Directory Installation Lab

## 🎯 Objective

Install Active Directory Domain Services (AD DS), promote the Windows Server to a Domain Controller, create a new forest, and verify the Active Directory environment.

---

## 🛠️ Lab Environment

- Hypervisor: Oracle VirtualBox
- Operating System: Windows Server 2022
- Server Name: **DC01**
- Network: NAT Network
- Static IP Address: Configured

---

## 🧪 Lab Tasks

### 1. Install Active Directory Domain Services (AD DS)

1. Open **Server Manager**.
2. Click **Add Roles and Features**.
3. Select **Role-based or feature-based installation**.
4. Select the local server (**DC01**).
5. Select **Active Directory Domain Services (AD DS)**.
6. Click **Add Features** when prompted.
7. Continue with the default settings.
8. Click **Install**.

---

### 2. Promote the Server to a Domain Controller

1. After the installation completes, click the notification flag in **Server Manager**.
2. Select **Promote this server to a domain controller**.
3. Select **Add a new forest**.
4. Enter the root domain name:

```text
corp.local
```

5. Configure the following:
   - Forest Functional Level: Windows Server 2022 (or latest available)
   - Domain Functional Level: Windows Server 2022 (or latest available)
   - Enable **DNS Server**
   - Enable **Global Catalog (GC)**
6. Enter and confirm the **Directory Services Restore Mode (DSRM)** password.
7. Continue using the default settings.
8. Click **Install**.
9. Allow the server to restart automatically.

---

### 3. Verify the Domain Controller

After restarting:

- Sign in using the domain account.
- Open **Server Manager**.
- Verify the server is functioning as a **Domain Controller**.

---

### 4. Verify Active Directory

Open the following management consoles:

- Active Directory Users and Computers
- Active Directory Administrative Center
- Active Directory Sites and Services
- DNS Manager

Verify:

- **corp.local** domain exists.
- Default Organizational Units (OUs) are created.
- Default Users and Groups are present.
- DNS Forward Lookup Zone is created.

---

### 5. Verify Domain Information

Open **Command Prompt** and run:

```cmd
hostname
echo %USERDOMAIN%
echo %LOGONSERVER%
```

Expected Output:

- Hostname: **DC01**
- User Domain: **CORP**
- Logon Server: **\\DC01**

---

## ✅ Outcome

- Active Directory Domain Services installed.
- Server promoted to a Domain Controller.
- New forest **corp.local** created.
- DNS installed and configured.
- Active Directory management tools verified.

---