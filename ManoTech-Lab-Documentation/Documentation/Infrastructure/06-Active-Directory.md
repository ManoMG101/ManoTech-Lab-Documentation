# Active Directory

## 1. Overview

Active Directory Domain Services (AD DS) provides centralized authentication, authorization, identity management, and organizational management for the ManoTech enterprise environment.

The Active Directory infrastructure provides the foundation for:

- Centralized user authentication
- Computer and server management
- Organizational Unit (OU) management
- Security group management
- Centralized access control
- Group Policy deployment
- Integration with DNS
- Integration with enterprise services such as AD CS and WSUS

The domain is structured to separate users, computers, administrative accounts, security groups, and service accounts according to their intended purpose.

---

## 2. Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0 | 2026-07-26 | Mohamed Osama | Initial Active Directory documentation |
| 1.1 | 2026-08-04 | Mohamed Osama | Updated Active Directory and infrastructure documentation |
| 1.2 | 2026-08-09 | Mohamed Osama | Redesigned and implemented the Active Directory OU structure, user account organization, computer organization, security groups, administrative accounts, and service accounts |

---

## 3. Domain Controller Information

| Property | Value |
|---|---|
| Hostname | `SRV01-DC` |
| IP Address | `192.168.1.2` |
| Operating System | Windows Server 2022 |
| Primary Role | Active Directory Domain Services |
| Domain | `MANOTECH.LOCAL` |
| DNS | `192.168.1.2` |

---

## 4. Active Directory Domain

The ManoTech environment uses the following Active Directory domain:

```text
MANOTECH.LOCAL
```

The domain is hosted by `SRV01-DC`.

The Domain Controller also provides DNS services required for Active Directory name resolution and domain functionality.

---

## 5. Active Directory Organizational Structure

The Active Directory structure was designed to separate objects according to their administrative purpose rather than simply organizing users by department.

The final custom structure is:

```text
MANOTECH.LOCAL
│
├── User Accounts
│   ├── Administrators
│   ├── IT
│   ├── Management
│   └── Standard Users
│
├── Domain Computers
│   ├── Domain servers
│   └── Workstations
│
├── Domain Controllers
│   └── SRV01-DC
│
├── Groups
│   ├── Distribution Groups
│   ├── GG-Administrators
│   ├── GG-IT
│   ├── GG-Management
│   ├── GG-Server-Admins
│   ├── GG-Standard-Users
│   └── GG-Workstation-Admins
│
└── Service Accounts
```

> **Note:** The default Active Directory containers created by Windows Server, such as `Users`, `Computers`, `Builtin`, and `ForeignSecurityPrincipals`, remain in the domain and are not part of the custom organizational structure.

---

## 6. User Account Organization

User accounts are organized according to their role and required level of access.

### 6.1 Administrators

```text
User Accounts
└── Administrators
```

This OU contains dedicated administrative accounts used for administrative tasks.

The lab currently contains ten administrative accounts:

```text
admin1
admin2
admin3
admin4
admin5
admin6
admin7
admin8
admin9
admin10
```

Administrative accounts are separated from normal user accounts to support a clear distinction between daily user activity and privileged administrative activity.

### 6.2 IT

```text
User Accounts
└── IT
```

Contains accounts belonging to IT personnel.

These accounts are intended for technical and infrastructure-related activities.

### 6.3 Management

```text
User Accounts
└── Management
```

Contains management-level user accounts.

Management users are separated from standard users because their security and access requirements may differ.

### 6.4 Standard Users

```text
User Accounts
└── Standard Users
```

Contains normal enterprise users who do not require administrative privileges.

This OU represents the primary user population within the ManoTech environment.

---

## 7. Computer Organization

Computer objects are separated from user accounts to allow computer-based policies and security configurations to be managed independently.

```text
Domain Computers
├── Domain servers
└── Workstations
```

### 7.1 Workstations

```text
Domain Computers
└── Workstations
```

Contains domain-joined Windows client machines used by employees.

Workstation-specific security configurations and policies will be associated with this OU.

### 7.2 Domain Servers

```text
Domain Computers
└── Domain servers
```

Contains domain-joined member servers that provide infrastructure and application services.

Examples include:

```text
SRV02-DHCP
SRV03-FileServer
SRV04-WebServer
SRV05-RRAS
```

Server-specific security and configuration policies will be separated from workstation policies.

---

## 8. Domain Controllers

Domain Controllers remain inside the default Active Directory OU:

```text
Domain Controllers
└── SRV01-DC
```

The `Domain Controllers` OU is maintained separately because Domain Controllers require specialized security configurations and administrative policies that should not be applied to normal member servers.

---

## 9. Security Groups

Security groups are located under:

```text
Groups
```

The current security group structure is:

```text
Groups
│
├── Distribution Groups
├── GG-Administrators
├── GG-IT
├── GG-Management
├── GG-Server-Admins
├── GG-Standard-Users
└── GG-Workstation-Admins
```

The `GG-` naming convention identifies Global Groups used for security and access management.

### 9.1 GG-Standard-Users

Contains standard enterprise users.

This group can be used when applying access permissions or user-specific configurations intended for normal employees.

### 9.2 GG-Management

Contains management users.

The group provides a centralized method of assigning permissions or policies intended specifically for management personnel.

### 9.3 GG-IT

Contains IT personnel.

The group is used to identify technical staff separately from normal users.

### 9.4 GG-Administrators

Contains the dedicated administrative accounts used for privileged administrative operations.

This provides a centralized identity for administrative access control.

### 9.5 GG-Workstation-Admins

Contains accounts that are authorized to perform administrative operations on workstation computers.

This allows workstation administration to be separated from server administration.

### 9.6 GG-Server-Admins

Contains accounts authorized to perform administrative operations on member servers.

Server administration is intentionally separated from workstation administration to follow the principle of least privilege.

---

## 10. Service Accounts

Dedicated service accounts are organized under:

```text
Service Accounts
```

Service accounts are separated from normal employee accounts because they are intended to be used by applications, services, or infrastructure components rather than interactive users.

This separation simplifies account management and allows service accounts to be subject to different security and lifecycle requirements.

---

## 11. Active Directory Design Principles

The ManoTech Active Directory structure follows several design principles.

### 11.1 Separation of Users and Computers

User accounts and computer accounts are stored in separate OU structures.

```text
User Accounts
        ≠
Domain Computers
```

This allows user and computer policies to be managed independently.

### 11.2 Separation of Workstations and Servers

Workstations and servers are placed into separate OUs because their security requirements and operational roles are different.

### 11.3 Separation of Administrative Accounts

Dedicated administrative accounts are separated from standard user accounts.

### 11.4 Group-Based Access Control

Security groups are used to manage access and administrative permissions instead of assigning permissions individually to every user.

### 11.5 Least Privilege

Administrative access is separated according to the resource being managed:

```text
GG-Workstation-Admins
        ↓
Workstation administration

GG-Server-Admins
        ↓
Server administration
```

Users are not automatically granted Domain Administrator privileges simply because they belong to the IT department.

---

## 12. Active Directory Validation

The Active Directory structure was validated using **Active Directory Users and Computers (ADUC)**.

The following components were verified:

- Domain `MANOTECH.LOCAL`
- Domain Controller `SRV01-DC`
- User Accounts OUs
- Domain Computers OUs
- Security Groups
- Service Accounts OU
- Administrative accounts
- Group memberships

### 12.1 List Organizational Units

```powershell
Get-ADOrganizationalUnit -Filter * |
Select-Object Name, DistinguishedName
```

### 12.2 List Users

```powershell
Get-ADUser -Filter * |
Select-Object Name, SamAccountName, Enabled
```

### 12.3 List Computers

```powershell
Get-ADComputer -Filter * |
Select-Object Name, DNSHostName
```

### 12.4 List Security Groups

```powershell
Get-ADGroup -Filter * |
Select-Object Name, GroupScope, GroupCategory
```

### 12.5 Check Administrative Group Membership

```powershell
Get-ADGroupMember "GG-Administrators" |
Select-Object Name, SamAccountName
```

---

## 13. Current AD Status

| Component | Status |
|---|---|
| Active Directory Domain | Completed |
| Domain Controller | Completed |
| User Account OUs | Completed |
| Computer OUs | Completed |
| Security Groups | Completed |
| Administrative Accounts | Completed |
| Service Accounts OU | Completed |
| Group Memberships | Completed |
| Group Policy | Planned / Next Phase |

---

## 14. Documentation Screenshot

The final Active Directory structure was verified using **Active Directory Users and Computers (ADUC)**.

The screenshot documents the implemented OU structure, security groups, and user account organization.

Suggested location:

```text
Documentation/
└── Screenshots/
    └── Active-Directory/
        └── ADUC-Final-Structure.png
```

---

## 15. Summary

The ManoTech Active Directory environment has been structured to provide a clear separation between:

```text
Users
    ↓
User Accounts
    ├── Standard Users
    ├── Management
    ├── IT
    └── Administrators

Computers
    ↓
Domain Computers
    ├── Workstations
    └── Domain servers

Security
    ↓
Groups
    ├── User Groups
    ├── Workstation Administration
    └── Server Administration

Services
    ↓
Service Accounts
```

This structure provides a scalable foundation for the next stage of the project, including **Group Policy, resource permissions, workstation security, server security, and centralized administration**.

## 16. Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0 | 2026-07-26 | Mohamed Osama | Initial Active Directory documentation |
| 1.1 | 2026-08-04 | Mohamed Osama | Updated Active Directory and infrastructure documentation |
| 1.2 | 2026-08-09 | Mohamed Osama | Redesigned and implemented the Active Directory OU structure, user account organization, computer organization, security groups, administrative accounts, and service accounts |