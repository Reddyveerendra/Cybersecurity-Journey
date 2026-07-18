# Day 5 – Active Directory Objects (Learning)

## Users

A **User** is an Active Directory object that represents a person, service, or application. It provides a unique identity that can authenticate to the domain and access resources based on the permissions assigned to it.

**Types of User Accounts**

- Standard User
- Administrator
- Guest
- Service Account
- Managed Service Account (MSA)
- Group Managed Service Account (gMSA)

---

## Groups

A **Group** is a collection of users, computers, or other groups that simplifies permission management. Instead of assigning permissions to individual users, permissions are assigned to groups, and members inherit those permissions.

**Purpose**

- Simplify administration
- Implement Role-Based Access Control (RBAC)
- Reduce permission management overhead
- Improve security

---

## Computers

A **Computer** object represents a domain-joined device in Active Directory. It allows the device to authenticate with the Domain Controller, receive Group Policies, and securely access domain resources.

Examples:

- Windows 10/11 Client
- Windows Server
- Domain Controller

---

## Organizational Units (OUs)

An **Organizational Unit (OU)** is a logical container used to organize users, computers, groups, and other Active Directory objects. OUs help administrators delegate control and apply Group Policies to specific parts of the organization.

**Example**

```
lab.local
│
├── IT
├── HR
├── Finance
└── Servers
```

---

## Group Policy Objects (GPOs)

A **Group Policy Object (GPO)** is a collection of centralized configuration settings used to manage users and computers in an Active Directory environment. GPOs enforce security settings, system configurations, and administrative policies across the domain.

**Examples**

- Password Policy
- Windows Firewall Rules
- USB Device Restrictions
- Software Installation
- Login Scripts
- Windows Defender Settings

---

## Security Groups vs Distribution Groups

### Security Groups

Used to assign permissions and control access to resources such as files, folders, printers, and applications.

### Distribution Groups

Used only for email distribution (Microsoft Exchange) and cannot be used to assign security permissions.

| Security Group | Distribution Group |
|---------------|--------------------|
| Used for access control | Used for email distribution |
| Can assign permissions | Cannot assign permissions |
| Used in RBAC | Used by Exchange email |
| Has a Security Identifier (SID) | No security permissions |