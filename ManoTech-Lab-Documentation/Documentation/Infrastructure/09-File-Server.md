# File Server

## Overview

The File Server provides centralized storage and resource sharing for users and departments within the ManoTech enterprise network.

In this lab, the File Server is used to host departmental shared folders and manage access permissions using NTFS permissions, Share Permissions, and Active Directory Security Groups.

---

## Server Information

| Property         | Value               |
| ---------------- | ------------------- |
| Hostname         | FILE01              |
| IP Address       | `192.168.1.4`       |
| Operating System | Windows Server 2019 |
| Role             | File Server         |
| Domain           | `manotech.local`    |

---

## Objectives

The File Server was implemented to:

* Provide centralized file storage.
* Organize resources by department.
* Control access using Active Directory security groups.
* Implement NTFS and Share Permissions.
* Separate enterprise resources from user workstations.
* Demonstrate centralized file access management.

---

## Folder Structure

The departmental folder structure was created under:

```text id="e9jv4h"
D:\Shares
│
├── HR
├── IT
├── Finance
├── Sales
├── Management
├── Public
└── Software
```

Each folder represents a separate logical resource within the enterprise environment.

---

## Shared Folders

The following folders are configured as network shares:

| Folder     | Share Name   | Purpose                      |
| ---------- | ------------ | ---------------------------- |
| HR         | `HR`         | HR Department Files          |
| IT         | `IT`         | IT Department Files          |
| Finance    | `Finance`    | Finance Department Files     |
| Sales      | `Sales`      | Sales Department Files       |
| Management | `Management` | Management Files             |
| Public     | `Public`     | General Shared Resources     |
| Software   | `Software`   | Internal Software Repository |

Users access these resources through SMB network shares.

Example:

```text id="f7b1a3"
\\FILE01\IT
\\FILE01\HR
\\FILE01\Finance
\\FILE01\Sales
\\FILE01\Management
\\FILE01\Public
\\FILE01\Software
```

---

## Permissions Management

Access control is implemented using:

* Active Directory Security Groups
* NTFS Permissions
* Share Permissions

Active Directory groups provide centralized membership management, while NTFS permissions control access to the underlying folders.

Share permissions provide the network-level access boundary.

---

## Permission Design

The intended departmental access model is based on security groups.

| Department       | Resource   | Intended Access            |
| ---------------- | ---------- | -------------------------- |
| IT Users         | IT         | Modify / Department Access |
| HR Users         | HR         | Modify / Department Access |
| Finance Users    | Finance    | Modify / Department Access |
| Sales Users      | Sales      | Modify / Department Access |
| Management Users | Management | Department Access          |
| Domain Users     | Public     | General Access             |
| Authorized Users | Software   | Read / Install Access      |

Actual access is determined by the combination of the user's Active Directory group membership and the configured NTFS/Share permissions.

---

## Share Permissions

Share permissions provide the first level of network access control.

The detailed security model is enforced through NTFS permissions.

The permission structure follows the principle that network access and filesystem access are controlled separately.

For example:

```text id="6t8y2n"
User
 │
 ▼
Active Directory Security Group
 │
 ▼
Share Permissions
 │
 ▼
NTFS Permissions
 │
 ▼
Folder / File
```

---

## Active Directory Integration

FILE01 is joined to the `manotech.local` domain and uses Active Directory for centralized authentication and authorization.

Active Directory provides:

* User authentication.
* Security Group membership.
* Centralized access control.
* Department-based authorization.
* Domain-based resource access.

This allows administrators to manage access through group membership instead of configuring individual users whenever possible.

---

## Access Testing

File Server access was validated from the Windows 11 domain client.

Testing included:

* Accessing authorized departmental folders.
* Verifying denied access to unauthorized resources.
* Testing access based on Active Directory group membership.
* Verifying NTFS permissions.
* Verifying Share Permissions.
* Confirming SMB connectivity to FILE01.

Example access path:

```text id="s4e7z2"
\\FILE01\IT
```

The results were used to confirm that users receive the expected level of access according to their assigned security groups.

---

## Troubleshooting

### Problem: User Cannot Access Shared Folder

#### Possible Causes

* Incorrect NTFS permissions.
* Incorrect Share Permissions.
* User is not a member of the required Security Group.
* Authentication issue.
* Network connectivity problem.
* Permission inheritance issue.

#### Resolution

The following were checked:

1. User's Active Directory group membership.
2. NTFS permissions.
3. Share Permissions.
4. Folder inheritance.
5. Network connectivity to FILE01.
6. Authentication status of the domain user.

---

### Problem: User Has Unexpected Access

#### Possible Causes

* Incorrect Security Group membership.
* NTFS inheritance.
* Conflicting permissions.
* Excessive permissions assigned to a parent folder.

#### Resolution

The user's group membership and effective permissions were reviewed.

NTFS inheritance and folder-level permissions were then checked to identify the source of the unexpected access.

---

### Problem: Access Denied Despite Correct Group Membership

#### Possible Causes

* NTFS permission missing.
* Share Permission restricting access.
* Cached authentication credentials.
* Incorrect folder inheritance.

#### Resolution

Both Share Permissions and NTFS Permissions were reviewed because the effective access is determined by their combined configuration.

---

## Security Considerations

The File Server follows centralized access-control principles:

* Users are managed through Active Directory.
* Department access is based on Security Groups.
* NTFS permissions provide granular filesystem access.
* Share Permissions control network-level access.
* Unauthorized users are denied access to restricted departmental resources.
* Permissions should be assigned to groups rather than individual users whenever possible.

---

## Validation Checklist

| Test                                             | Expected Result |
| ------------------------------------------------ | --------------- |
| FILE01 reachable over network                    | Successful      |
| SMB share accessible                             | Successful      |
| Authorized IT user accesses IT share             | Allowed         |
| Unauthorized user accesses restricted share      | Denied          |
| Public share accessible to authorized users      | Allowed         |
| NTFS permissions enforced                        | Successful      |
| Active Directory group membership affects access | Successful      |

---

## Notes

The File Server provides centralized departmental storage and demonstrates enterprise file-access management using Active Directory, SMB, Share Permissions, and NTFS Permissions.

The final permission structure will be aligned with the finalized Active Directory Organizational Unit and Security Group design.

---

## Revision History

| Version | Date       | Author        | Description                                                                                 |
| ------- | ---------- | ------------- | ------------------------------------------------------------------------------------------- |
| 1.0     | 2026-07-26 | Mohamed Osama | Initial documentation                                                                       |
| 1.1     | 2026-08-04 | Mohamed Osama | Reviewed and updated documentation                                                          |
| 1.2     | 2026-08-09 | Mohamed Osama | Updated server naming, file-sharing structure, permissions model, and validation procedures |
