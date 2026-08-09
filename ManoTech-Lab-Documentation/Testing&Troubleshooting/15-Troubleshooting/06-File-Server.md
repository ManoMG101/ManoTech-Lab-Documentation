# File Server Troubleshooting

## Overview

This document records the file server and permissions-related issues encountered during the deployment of the ManoTech Enterprise Windows Server Lab.

The troubleshooting focuses on shared folder access, NTFS permissions, Active Directory security groups, and permission inheritance.

---

# 1. User Unable to Access Department Share

## Problem

A domain user was unable to access a departmental shared folder on the File Server.

## Symptoms

The user could access some resources but was denied access to a folder that should have been available to the user's department.

## Possible Causes

* Incorrect NTFS permissions.
* Incorrect Share Permissions.
* Incorrect Active Directory group membership.
* Permission inheritance issues.
* Network connectivity problems.

## Investigation

The following were checked:

* User's Active Directory group membership.
* Folder NTFS permissions.
* Share permissions.
* Folder inheritance.
* Connectivity to the File Server.

The File Server was verified as:

```text id="v1s0uk"
SRV03-FILESRV
192.168.1.4
```

## Resolution

The user's security group membership and the corresponding NTFS permissions were reviewed.

The required security group was assigned the appropriate access to the departmental folder.

The user then re-authenticated and tested the share again.

## Result

**Resolved**

---

# 2. HR User Access Issue

## Problem

An HR user was unable to access a required shared resource while another department's user could access its corresponding folder.

## Investigation

The following were reviewed:

```text id="j9j4c6"
User
 ↓
Active Directory Security Group
 ↓
NTFS Permissions
 ↓
Share Permissions
 ↓
Folder Access
```

The issue was traced to the access configuration rather than the File Server service itself.

## Resolution

The applicable security group and folder permissions were reviewed and corrected.

Access was then tested from the domain-joined Windows 11 client.

## Result

**Resolved**

---

# 3. Unexpected Access Permissions

## Problem

A user had more or less access than expected to a shared folder.

## Possible Causes

* NTFS inheritance.
* Parent folder permissions.
* Incorrect security group membership.
* Direct user permissions.
* Conflicting Allow and Deny entries.

## Investigation

The folder's Security properties were reviewed.

The effective permissions were evaluated based on:

* User permissions.
* Group permissions.
* Inherited permissions.
* Share permissions.

## Resolution

Permissions were adjusted to rely primarily on Active Directory security groups instead of assigning permissions directly to individual users.

## Result

**Resolved**

---

# 4. NTFS and Share Permission Interaction

## Problem

Access behavior did not match the expected NTFS permissions.

## Cause

Network access to a shared folder is affected by both:

* Share Permissions.
* NTFS Permissions.

When accessing a folder over the network, the effective access is determined by the combination of both permission layers.

## Resolution

The lab was configured using:

```text id="oz5ks2"
Share Permissions
        ↓
Network Access
        +
NTFS Permissions
        ↓
Detailed Resource Access
```

The share permissions were kept broad while NTFS permissions were used as the primary layer for detailed access control.

## Result

**Passed**

---

# 5. Permission Inheritance Issue

## Problem

Permissions assigned to a parent directory affected child folders unexpectedly.

## Possible Cause

NTFS inheritance was enabled on the affected folder.

## Investigation

The Advanced Security Settings of the affected folder were reviewed.

The inheritance chain was checked to determine whether permissions were inherited from the parent directory.

## Resolution

Inheritance was reviewed and adjusted where necessary to ensure that departmental folders followed the intended access model.

## Result

**Resolved**

---

# 6. Active Directory Integration

The File Server relies on Active Directory for centralized authentication and authorization.

The access workflow is:

```text id="z8n3eu"
Domain User
     ↓
Active Directory
     ↓
Security Group
     ↓
NTFS Permissions
     ↓
Shared Folder
```

This design avoids managing permissions individually on each workstation.

## Validation

The Windows 11 domain client was used to test access to:

```text id="v6n3dx"
\\SRV03-FILESRV
```

Departmental access was verified according to the user's assigned security group.

## Result

**Passed**

---

# 7. File Server Connectivity Validation

Before troubleshooting permissions, basic network connectivity was verified.

### Test

```powershell id="4n2w4a"
ping 192.168.1.4
```

### Name Resolution

```powershell id="av1rj2"
nslookup SRV03-FILESRV
```

### Share Access

```text id="v29f5p"
\\SRV03-FILESRV
```

## Result

The File Server was reachable and its hostname resolved correctly.

---

# 8. Troubleshooting Methodology

The File Server troubleshooting process followed this sequence:

```text id="d4s4e5"
Check Network Connectivity
        ↓
Verify File Server Availability
        ↓
Check Share Configuration
        ↓
Check User Group Membership
        ↓
Check Share Permissions
        ↓
Check NTFS Permissions
        ↓
Check Inheritance
        ↓
Test Access Again
```

This prevents confusing a network problem with a permissions problem.

---

# 9. Useful Commands

Check network configuration:

```powershell id="3a8k5n"
ipconfig /all
```

Test File Server connectivity:

```powershell id="k0v1g8"
ping 192.168.1.4
```

Test DNS resolution:

```powershell id="u1y7ga"
nslookup SRV03-FILESRV
```

Display current user:

```powershell id="z7h6qq"
whoami
```

Display current user's group memberships:

```powershell id="e8d8s0"
whoami /groups
```

Display mapped network drives:

```powershell id="5r0xgd"
net use
```

---

# 10. Lessons Learned

The File Server troubleshooting process demonstrated several important enterprise file management concepts:

* Active Directory security groups simplify permission management.
* NTFS permissions provide granular resource-level access control.
* Share and NTFS permissions must be considered together.
* Permission inheritance can affect child folders.
* Network connectivity should be verified before investigating permissions.
* Group-based access is preferable to assigning permissions directly to individual users.
* Testing access from a domain-joined client provides a realistic validation of the configuration.

---

## Resolution Summary

| Issue                               | Status   |
| ----------------------------------- | -------- |
| Department Share Access             | Resolved |
| HR Access Issue                     | Resolved |
| Unexpected Permissions              | Resolved |
| NTFS / Share Permission Interaction | Passed   |
| Permission Inheritance              | Resolved |
| Active Directory Integration        | Passed   |
| File Server Connectivity            | Passed   |

---

## Revision History

| Version | Date       | Author        | Description                                       |
| ------- | ---------- | ------------- | ------------------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial File Server troubleshooting documentation |
