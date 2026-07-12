# Day 9 – Active Directory Fundamentals (Learning)

## Objective

Understand the basic components of Active Directory (AD), how it organizes resources, and why it is widely used in enterprise environments.

---

# What is Active Directory (AD)?

Active Directory (AD) is Microsoft's centralized directory service used to store and manage information about network resources such as:

- Users
- Computers
- Groups
- Printers
- Shared folders
- Policies

It provides centralized authentication, authorization, and management for an organization's Windows environment.

---

# Why Organizations Use Active Directory

Organizations use Active Directory to centrally manage users, computers, and security policies.

## Benefits

- Centralized user and computer management
- Authentication and Authorization
- Single Sign-On (SSO)
- Group Policy Management
- Security and Audit Compliance
- Resource Sharing
- Simplified administration

---

# Active Directory Components

## 1. Domain

A **Domain** is the basic administrative and security boundary in Active Directory.

It contains users, computers, groups, and other resources that share the same security policies and database.

### Example

```
company.com
```

Inside the domain:

```
company.com
│
├── Users
├── Computers
├── Servers
└── Groups
```

### Real-World Analogy

A **Domain** is like a **city** where everyone follows the same local rules and administration.

---

## 2. Tree

A **Tree** is a collection of one or more related domains that share a common namespace.

All domains within a tree trust each other automatically.

### Example

```
company.com
│
├── sales.company.com
├── hr.company.com
└── it.company.com
```

### Real-World Analogy

A **Tree** is like a **state** containing multiple cities (domains) that belong to the same region.

---

## 3. Forest

A **Forest** is the highest-level structure in Active Directory.

It contains one or more trees and provides trust relationships between them.

Domains in different trees can securely share resources through the forest.

### Example

```
Forest
│
├── company.com
│     ├── sales.company.com
│     └── hr.company.com
│
└── partner.com
      └── support.partner.com
```

### Real-World Analogy

A **Forest** is like a **country** that contains multiple states (trees).

---

## 4. Organizational Unit (OU)

An Organizational Unit (OU) is a container inside a domain used to organize users, computers, and groups.

Administrators apply Group Policies and delegate administrative control at the OU level.

### Example

```
company.com
│
├── HR
├── IT
├── Finance
└── Sales
```

Each OU contains users and computers belonging to that department.

### Benefits

- Organize resources
- Apply Group Policies
- Delegate administration

---

## 5. Domain Controller (DC)

A Domain Controller is a server that hosts the Active Directory database.

It is responsible for:

- Authenticating users
- Authorizing access to resources
- Applying Group Policies
- Replicating AD data with other Domain Controllers

### Authentication Process

```
User Login
      │
      ▼
Domain Controller
      │
      ▼
Verify Username & Password
      │
      ▼
Access Granted
```

---

## 6. LDAP (Lightweight Directory Access Protocol)

LDAP is the protocol used to communicate with Active Directory.

Applications and computers use LDAP to search, read, and update directory information.

### Common Uses

- Search for users
- Search for groups
- Verify user information
- Retrieve organizational data

### Example

When a user logs into an application:

```
Application
      │
      ▼
LDAP Query
      │
      ▼
Active Directory
      │
      ▼
Return User Information
```

---

## 7. Global Catalog (GC)

The Global Catalog is a special role performed by a Domain Controller.

It stores:

- A complete copy of all objects in its own domain.
- A partial copy of objects from every other domain in the forest.

This allows users and applications to quickly search for objects across the entire forest.

### Functions

- Forest-wide searches
- Universal Group Membership lookup
- User logon support
- Cross-domain object searches

> **Note:** The Global Catalog is **not** a "Global Admin" account. It is a directory service that helps locate objects across the forest.

---

# Active Directory Structure

```
Forest
│
├── Tree
│      ├── Domain
│      │      ├── OU
│      │      ├── Users
│      │      └── Computers
│      │
│      └── Domain
│
└── Tree
       └── Domain
```

---

# Summary

| Component | Description |
|-----------|-------------|
| Active Directory | Centralized directory service for managing users, computers, and resources |
| Domain | Administrative and security boundary |
| Tree | Collection of related domains sharing a namespace |
| Forest | Collection of one or more trees with trust relationships |
| Organizational Unit (OU) | Container used to organize resources and apply Group Policies |
| Domain Controller (DC) | Server that authenticates users and manages Active Directory |
| LDAP | Protocol used to access and manage Active Directory information |
| Global Catalog | Enables forest-wide searches and stores partial information about all domains |

---

# Key Takeaways

- Active Directory centralizes authentication, authorization, and management of Windows resources.
- A **Domain** is the primary security boundary in Active Directory.
- Multiple domains form a **Tree**, and multiple trees form a **Forest**.
- Organizational Units (OUs) help organize resources and simplify policy management.
- Domain Controllers authenticate users and maintain the Active Directory database.
- LDAP is the protocol used to communicate with Active Directory.
- The Global Catalog enables fast searches across all domains in a forest.

---

# Skills Learned

- Active Directory Fundamentals
- Domain, Tree, and Forest Concepts
- Organizational Units (OU)
- Domain Controllers (DC)
- LDAP Basics
- Global Catalog
- Enterprise Identity Management