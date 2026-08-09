# File Server Validation

## Overview

This document describes the validation tests performed to verify the functionality and access control of the File Server within the ManoTech Enterprise Windows Server Lab.

The validation focuses on shared folder availability, network access, NTFS permissions, Active Directory security group integration, and authorized versus unauthorized access.

---

## File Server Information

| Property         | Value               |
| ---------------- | ------------------- |
| Hostname         | `SRV03-FILESRV`     |
| IP Address       | `192.168.1.4`       |
| Operating System | Windows Server 2019 |
| Role             | File Server         |
| Domain           | `manotech.local`    |

---

## Folder Structure

The File Server contains the following departmental and shared resources:

```text
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

---

# 1. File Server Connectivity

The Windows 11 domain client was tested for connectivity with the File Server.

### Test

```powershell
ping 192.168.1.4
```

### Expected Result

The File Server should respond successfully.

### Result

**Passed**

---

# 2. SMB Share Availability

The available network shares were verified from the domain client.

### Test

```powershell
net view \\SRV03-FILESRV
```

The server can also be accessed directly using:

```text
\\SRV03-FILESRV
```

### Expected Result

The configured shared folders should be accessible from the domain client.

### Result

**Passed**

---

# 3. Departmental Share Access

The departmental shares were tested using authorized domain users.

### Resources Tested

```text
\\SRV03-FILESRV\HR
\\SRV03-FILESRV\IT
\\SRV03-FILESRV\Finance
\\SRV03-FILESRV\Sales
\\SRV03-FILESRV\Management
```

### Expected Result

Users should be able to access the resources assigned to their department.

### Result

**Passed**

---

# 4. Public Share Access

The Public share was tested using domain users.

### Resource

```text
\\SRV03-FILESRV\Public
```

### Expected Result

Authorized domain users should be able to access the Public shared folder.

### Result

**Passed**

---

# 5. Software Share Access

The Software share was tested from the domain client.

### Resource

```text
\\SRV03-FILESRV\Software
```

### Expected Result

Authorized users should be able to access the internal software repository according to the configured permissions.

### Result

**Passed**

---

# 6. NTFS Permission Validation

NTFS permissions were tested to verify that access is controlled according to the configured security groups.

### Validation

The following were checked:

* User group membership.
* Folder NTFS permissions.
* Permission inheritance.
* Allow permissions.
* Deny/Access Denied behavior where applicable.

### Expected Result

Users should receive only the level of access assigned through the security model.

### Result

**Passed**

---

# 7. Department-Based Access Testing

Access was tested using users belonging to different departmental groups.

### Example

An IT user was tested against:

```text
\\SRV03-FILESRV\IT
```

The user was expected to have the permissions assigned to the IT security group.

Similarly, users from other departments were tested against their respective resources.

### Result

**Passed**

---

# 8. Unauthorized Access Testing

Users were tested against resources outside their assigned department.

### Example

A user without the required permissions attempted to access a restricted departmental folder.

### Expected Result

Access should be denied.

### Result

**Passed**

The configured permission model successfully restricted unauthorized access.

---

# 9. File Creation and Modification Testing

Authorized users were tested to verify that their assigned permissions allow the expected operations.

### Tests

* Create a file.
* Modify a file.
* Rename a file.
* Delete a file where permitted.

### Expected Result

Users should be able to perform only the operations allowed by their NTFS permissions.

### Result

**Passed**

---

# 10. Share and NTFS Permission Integration

Both Share Permissions and NTFS Permissions were validated together.

The effective network access is determined by the combination of:

```text
Share Permissions
        +
NTFS Permissions
        ↓
Effective Access
```

### Expected Result

The configured permission model should provide the intended level of access when users connect through the network share.

### Result

**Passed**

---

# 11. File Server Validation Summary

| Test                     | Expected Result             | Status |
| ------------------------ | --------------------------- | ------ |
| File Server Connectivity | Server reachable            | Passed |
| SMB Shares               | Shares accessible           | Passed |
| HR Share                 | Authorized access           | Passed |
| IT Share                 | Authorized access           | Passed |
| Finance Share            | Authorized access           | Passed |
| Sales Share              | Authorized access           | Passed |
| Management Share         | Authorized access           | Passed |
| Public Share             | Authorized users can access | Passed |
| Software Share           | Authorized users can access | Passed |
| NTFS Permissions         | Correct access control      | Passed |
| Unauthorized Access      | Access denied               | Passed |
| File Operations          | Correct permissions applied | Passed |

---

# 12. Validation Result

The File Server was successfully validated as a centralized enterprise file storage and sharing platform.

The Windows 11 domain client was able to access the configured SMB shares, while NTFS permissions and Active Directory security groups were used to control departmental access.

The validation confirmed that authorized users can access and modify resources according to their assigned permissions, while unauthorized users are prevented from accessing restricted resources.

---

## Tools Used

* File Server Resource Manager
* File Explorer
* PowerShell
* `ping`
* `net view`
* Active Directory Users and Computers
* NTFS Security Properties

---

## Revision History

| Version | Date       | Author        | Description                                  |
| ------- | ---------- | ------------- | -------------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial File Server validation documentation |
