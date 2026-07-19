# Day 6 – Active Directory Administration Lab

## Objective

Learn how to perform common Active Directory administration tasks, including:

- Creating users
- Creating groups
- Adding users to groups
- Creating Organizational Units (OUs)
- Applying Group Policy Objects (GPOs)
- Resetting user passwords
- Disabling and unlocking user accounts

---

# 1. Creating a User

A **User Account** is an identity that allows a person or service to authenticate to Active Directory and access resources based on assigned permissions.

## Steps

1. Open **Server Manager**.
2. Click **Tools → Active Directory Users and Computers (ADUC)**.
3. Expand your domain (e.g., `lab.local`).
4. Right-click the **Users** container or an Organizational Unit (OU).
5. Select **New → User**.
6. Enter the following details:
   - First Name
   - Last Name
   - User Logon Name (UPN)
7. Click **Next**.
8. Set a password.
9. Select the required options:
   - User must change password at next logon
   - User cannot change password
   - Password never expires
   - Account is disabled
10. Click **Finish**.

---

# 2. Creating a Group

A **Group** is a collection of users or computers that simplifies permission management.

Instead of assigning permissions to individual users, permissions are assigned to groups.

## Steps

1. Open **Active Directory Users and Computers**.
2. Right-click the desired OU.
3. Select **New → Group**.
4. Enter a group name.

Example:

```text
IT-Team
```

5. Choose the Group Scope:
   - Global
   - Domain Local
   - Universal
6. Choose the Group Type:
   - Security
   - Distribution
7. Click **OK**.

---

# 3. Adding Users to Groups

Adding users to groups allows them to inherit the permissions assigned to that group.

## Steps

1. Open **Active Directory Users and Computers**.
2. Right-click the user.
3. Select **Properties**.
4. Open the **Member Of** tab.
5. Click **Add**.
6. Enter the group name.

Example:

```text
IT-Team
```

7. Click **Check Names**.
8. Click **OK**.

The user is now a member of the selected group.

---

# 4. Creating an Organizational Unit (OU)

An **Organizational Unit (OU)** is a logical container used to organize users, groups, and computers within Active Directory.

OUs help administrators:

- Organize departments
- Apply Group Policies
- Delegate administrative control

Example:

```text
Company
│
├── HR
├── IT
├── Finance
└── Sales
```

## Steps

1. Right-click the domain.
2. Select **New → Organizational Unit**.
3. Enter the OU name.

Example:

```text
IT
```

4. Click **OK**.

---

# 5. Moving Users into an OU

Users can be moved into an OU for better organization and policy management.

## Steps

1. Right-click the user.
2. Select **Move**.
3. Choose the destination OU.
4. Click **OK**.

---

# 6. Creating a Group Policy Object (GPO)

A **Group Policy Object (GPO)** is used to centrally manage security settings and system configurations across Active Directory.

Examples of GPO settings include:

- Password Policy
- Account Lockout Policy
- Windows Defender
- Firewall
- Windows Updates
- Desktop Restrictions
- USB Device Control

## Steps

1. Open **Group Policy Management**.
2. Expand:

```text
Forest
└── Domains
    └── lab.local
```

3. Right-click the desired OU.
4. Select **Create a GPO in this domain, and Link it here**.
5. Enter a GPO name.

Example:

```text
IT Security Policy
```

6. Click **OK**.

---

# 7. Editing a GPO

## Steps

1. Right-click the GPO.
2. Select **Edit**.

Navigate to:

```text
Computer Configuration
```

or

```text
User Configuration
```

Example:

```text
Computer Configuration
    └── Policies
        └── Windows Settings
            └── Security Settings
```

Common policies:

- Password Complexity
- Account Lockout
- Windows Firewall
- Windows Defender
- Audit Policy
- User Rights Assignment

---

# 8. Resetting a User Password

Administrators can reset a user's password without knowing the current password.

## Steps

1. Right-click the user.
2. Select **Reset Password**.
3. Enter a new password.
4. Select:

```text
User must change password at next logon
```

5. Click **OK**.

---

# 9. Disabling a User Account

Disable user accounts when an employee leaves the organization or the account is no longer required.

## Steps

1. Right-click the user.
2. Select **Disable Account**.

The user will no longer be able to authenticate until the account is re-enabled.

---

# 10. Unlocking a Locked Account

Accounts may become locked after multiple failed login attempts.

## Steps

1. Right-click the user.
2. Select **Properties**.
3. Open the **Account** tab.
4. Select **Unlock Account**.
5. Click **Apply**.

---

# 11. Deleting a User

## Steps

1. Right-click the user.
2. Select **Delete**.
3. Confirm the deletion.

> **Note:** Deleted objects can be recovered if the **Active Directory Recycle Bin** is enabled.

---

# Common Active Directory Administrative Tools

| Tool | Purpose |
|------|---------|
| Active Directory Users and Computers (ADUC) | Manage users, groups, computers, and OUs |
| Group Policy Management | Create and manage Group Policies |
| Active Directory Administrative Center | Modern AD management console |
| Server Manager | Manage Windows Server roles and features |
| Windows PowerShell | Automate Active Directory administration |

---

# Common PowerShell Commands

### List all users

```powershell
Get-ADUser -Filter *
```

### List all groups

```powershell
Get-ADGroup -Filter *
```

### Create a new user

```powershell
New-ADUser
```

### Create a new group

```powershell
New-ADGroup
```

### Add a user to a group

```powershell
Add-ADGroupMember
```

### Unlock a user account

```powershell
Unlock-ADAccount
```

### Reset a user's password

```powershell
Set-ADAccountPassword
```

---

# Interview Questions

## What is an Organizational Unit (OU)?

An Organizational Unit (OU) is a container in Active Directory used to organize users, groups, and computers. OUs simplify administration, allow delegation of control, and enable Group Policy Objects (GPOs) to be applied to specific sets of objects.

---

## Why are Groups used?

Groups simplify permission management by allowing administrators to assign permissions to a group instead of individual users.

---

## What is a Group Policy Object (GPO)?

A Group Policy Object (GPO) is a collection of settings used to configure user and computer behavior across the Active Directory domain.

---

## Difference Between an OU and a Group

| Organizational Unit (OU) | Group |
|---------------------------|-------|
| Organizes Active Directory objects | Organizes permissions |
| Used to apply Group Policies | Used to assign access rights |
| Supports delegation of administration | Used for access control |
| Cannot be used to assign permissions directly | Permissions are assigned to groups |

---

## What is ADUC?

**Active Directory Users and Computers (ADUC)** is a Microsoft Management Console (MMC) snap-in used to create, manage, and administer users, groups, computers, and Organizational Units within an Active Directory environment.