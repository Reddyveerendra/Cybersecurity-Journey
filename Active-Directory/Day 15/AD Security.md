# Day 7 – AD Security (Learning)

## Least Privilege

The Principle of Least Privilege (PoLP) means giving users, applications, and services **only the minimum permissions required** to perform their job.

This helps to:

- Reduce the impact if an account is compromised.
- Minimize the attack surface.
- Prevent unauthorized access to sensitive resources.

---

## Privileged Groups

Privileged groups are Active Directory groups whose members have **high-level administrative permissions**.

Examples:

- Domain Admins
- Enterprise Admins
- Schema Admins
- Administrators
- Backup Operators
- Server Operators

If an attacker compromises a member of a privileged group, they can gain extensive control over the Active Directory environment.

---

## Pass-the-Hash (PtH)

Pass-the-Hash is an attack where an attacker uses a **stolen NTLM hash** instead of the user's plaintext password to authenticate.

### Attack Flow

```text
User logs in
      │
      ▼
NTLM hash stored in LSASS memory
      │
      ▼
Attacker steals NTLM hash
      │
      ▼
Uses hash to authenticate
      │
      ▼
Lateral movement
```

The attacker never needs to know the actual password.

---

## Kerberoasting

Kerberoasting is an attack that targets **Active Directory service accounts**.

The attacker first compromises a normal domain user account.

Using that account, the attacker requests a **Kerberos Service Ticket (TGS)** for a service account.

The Domain Controller encrypts the TGS using the **service account's password hash**.

The attacker exports the ticket and performs **offline password cracking** to recover the service account password.

If the password is weak, the attacker gains access to the service account.

### Attack Flow

```text
Normal User Account Compromised
            │
            ▼
Request TGS from Domain Controller
            │
            ▼
TGS encrypted using Service Account Hash
            │
            ▼
Export TGS
            │
            ▼
Offline Password Cracking
            │
            ▼
Service Account Compromised
```

---

## Golden Ticket (High Level)

A Golden Ticket attack occurs when an attacker compromises the **KRBTGT account hash**.

The KRBTGT account signs all **Ticket Granting Tickets (TGTs)** in the domain.

With the KRBTGT hash, the attacker can forge valid TGTs and impersonate any user, including Domain Admins.

### Attack Flow

```text
Compromise Domain Controller
          │
          ▼
Steal KRBTGT Hash
          │
          ▼
Forge TGT
          │
          ▼
Impersonate Any User
          │
          ▼
Long-Term Persistence
```

---

## Silver Ticket (High Level)

A Silver Ticket attack occurs when an attacker compromises a **service account hash**.

The attacker forges a **Kerberos Service Ticket (TGS)** for a specific service.

Unlike a Golden Ticket attack, the attacker does **not** need to contact the Domain Controller after creating the forged ticket.

Examples of targeted services:

- SQL Server
- IIS
- File Server

### Attack Flow

```text
Compromise Service Account Hash
             │
             ▼
Forge Service Ticket (TGS)
             │
             ▼
Access Specific Service
             │
             ▼
Bypass Domain Controller Validation
```

---

## Common Active Directory Attack Path

```text
Phishing / Malware
        │
        ▼
User Account Compromised
        │
        ▼
Credential Theft
        │
        ▼
Kerberoasting
        │
        ▼
Service Account Compromised
        │
        ▼
Privilege Escalation
        │
        ▼
Domain Admin
        │
        ▼
KRBTGT Hash Compromised
        │
        ▼
Golden Ticket
        │
        ▼
Persistence
```

---

# Event IDs Related to AD Security

| Event ID | Description |
|----------|-------------|
| **1102** | Audit log cleared |
| **4624** | Successful logon |
| **4625** | Failed logon |
| **4634** | User logoff |
| **4672** | Special privileges assigned to a new logon |
| **4688** | Process creation |
| **4697** | Service installed |
| **4720** | User account created |
| **4728** | User added to a global security group |
| **4732** | User added to a local security group |
| **4740** | User account locked out |
| **4768** | Kerberos Ticket Granting Ticket (TGT) requested |
| **4769** | Kerberos Service Ticket (TGS) requested |
| **4771** | Kerberos pre-authentication failed |
| **4776** | NTLM authentication |
| **5136** | Active Directory object modified |

---

# Interview Tips

### Difference Between Pass-the-Hash and Kerberoasting

| Pass-the-Hash | Kerberoasting |
|---------------|---------------|
| Uses a stolen NTLM hash | Targets service accounts |
| No password cracking required | Offline password cracking required |
| Used for lateral movement | Used for privilege escalation |
| Targets user accounts | Targets service accounts |

---

### Difference Between Golden Ticket and Silver Ticket

| Golden Ticket | Silver Ticket |
|---------------|---------------|
| Uses KRBTGT hash | Uses service account hash |
| Forges TGT | Forges TGS |
| Access to entire domain | Access to a specific service |
| Requires KRBTGT compromise | Requires service account compromise |
| High impact | Lower impact than Golden Ticket |