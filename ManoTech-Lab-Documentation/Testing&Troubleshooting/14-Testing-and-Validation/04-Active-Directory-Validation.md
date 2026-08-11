# Active Directory Validation

## Overview

This document describes the validation tests performed to verify the Active Directory infrastructure within the ManoTech Enterprise Windows Server Lab.

The purpose of the validation is to confirm that the Active Directory Domain Services infrastructure is operating correctly and that the domain controller provides authentication, directory services, DNS integration, organizational structure, user accounts, security groups, and Group Policy processing for domain clients.

---

## Active Directory Domain

| Property          | Value                            |
| ----------------- | -------------------------------- |
| Domain Name       | `manotech.local`                 |
| NetBIOS Name      | `MANOTECH`                       |
| Domain Controller | `SRV01-DC`                       |
| IP Address        | `192.168.1.2`                    |
| Operating System  | Windows Server 2019              |
| Role              | Active Directory Domain Services |
| DNS Server        | `192.168.1.2`                    |
| Network           | `192.168.1.0/24`                 |

---

# 1. Active Directory Domain Validation

The Active Directory domain configuration was verified to confirm that the domain is correctly configured and available.

### Command

```powershell
Get-ADDomain
```

### Expected Result

The command should return the configured domain information:

```text
DNSRoot     : manotech.local
NetBIOSName : MANOTECH
```

### Result

**Passed**

The `manotech.local` Active Directory domain was successfully verified.

---

# 2. Domain Controller Validation

The domain controller configuration was checked to confirm that `SRV01-DC` is correctly registered as a domain controller.

### Command

```powershell
Get-ADDomainController
```

### Expected Result

The domain controller should be identified as:

```text
Hostname : SRV01-DC
Domain   : manotech.local
```

### Result

**Passed**

`SRV01-DC` was successfully verified as the domain controller for `manotech.local`.

---

# 3. Active Directory Domain Services Validation

The Active Directory Domain Services service was checked to confirm that it is running.

### Command

```powershell
Get-Service NTDS
```

### Expected Result

The service should have the following status:

```text
Status: Running
```

### Result

**Passed**

The Active Directory Domain Services service is running successfully.

---

# 4. DNS Integration Validation

Active Directory DNS integration was verified because DNS is required for domain authentication and Active Directory service discovery.

### Command

```powershell
Resolve-DnsName manotech.local
```

### Expected Result

The domain should resolve to the domain controller:

```text
192.168.1.2
```

### Result

**Passed**

The `manotech.local` domain successfully resolves through the domain controller DNS service.

---

# 5. OU Structure Validation

The Active Directory Organizational Unit structure was verified.

### Command

```powershell
Get-ADOrganizationalUnit -Filter * | Select-Object Name, DistinguishedName
```

### Expected Structure

The following organizational structure was verified:

```text
MANOTECH.LOCAL
│
├── Users
│   ├── Standard Users
│   ├── IT
│   └── Management
│
└── Domain Controllers
```

### Result

**Passed**

The required Organizational Units were successfully created and verified.

---

# 6. User Account Validation

The domain user accounts were checked to confirm that they exist within Active Directory.

### Command

```powershell
Get-ADUser -Filter * | Select-Object Name, SamAccountName, Enabled
```

### Expected Result

Configured domain users should appear with their corresponding account status.

### Result

**Passed**

The required domain user accounts were successfully verified.

---

# 7. Security Group Validation

Active Directory security groups were verified to confirm that users can be organized according to their assigned roles.

### Command

```powershell
Get-ADGroup -Filter * | Select-Object Name, GroupScope, GroupCategory
```

### Expected Result

The configured security groups should be available within the domain.

### Result

**Passed**

The required Active Directory security groups were successfully verified.

---

# 8. Group Membership Validation

User membership within the configured security groups was checked.

### Command

```powershell
Get-ADGroupMember "GroupName"
```

### Expected Result

Users should appear as members of their assigned security groups.

### Result

**Passed**

The configured group memberships were successfully verified.

---

# 9. Domain Controller Health Validation

The overall health of the Active Directory domain controller was checked using the built-in diagnostic utility.

### Command

```powershell
dcdiag
```

### Expected Result

The diagnostic tests should complete without critical errors.

### Result

**Passed**

The domain controller passed the Active Directory diagnostic checks.

---

# 10. Active Directory Replication Validation

Active Directory replication status was checked.

### Command

```powershell
repadmin /replsummary
```

### Expected Result

The replication summary should not report replication failures.

### Result

**Passed**

No critical Active Directory replication issues were detected.

> **Note:** The lab currently contains a single domain controller, therefore there are no additional domain controllers participating in replication.

---

# 11. Domain Client Authentication

The Windows 11 client was used to validate domain authentication.

### Test

The client was logged in using a domain account:

```text
MANOTECH\<username>
```

### Expected Result

The domain user should successfully authenticate against `SRV01-DC`.

### Result

**Passed**

The Windows 11 client successfully authenticated using the `manotech.local` domain.

---

# 12. Domain Membership Validation

The Windows 11 client was checked to confirm that it is joined to the Active Directory domain.

### Command

```powershell
systeminfo | findstr /B /C:"Domain"
```

### Expected Result

The domain should be displayed as:

```text
manotech.local
```

### Result

**Passed**

The Windows 11 client is successfully joined to the `manotech.local` domain.

---

# 13. Group Policy Processing Validation

Group Policy processing was verified on the domain client.

### Command

```powershell
gpresult /r
```

### Expected Result

The command should display the applied Group Policy Objects for the logged-in domain user and computer.

### Result

**Passed**

Group Policy processing was successfully verified on the domain client.

---

# 14. Active Directory Configuration Summary

| Test                         | Expected Result                 | Status |
| ---------------------------- | -------------------------------- | ------- |
| Active Directory Domain      | `manotech.local` available       | Passed |
| Domain Controller            | `SRV01-DC` verified              | Passed |
| AD DS Service                | Running                          | Passed |
| DNS Integration              | Domain resolves correctly        | Passed |
| OU Structure                 | Required OUs available           | Passed |
| User Accounts                | Domain users verified            | Passed |
| Security Groups              | Groups verified                  | Passed |
| Group Membership             | Memberships verified             | Passed |
| Domain Controller Health     | No critical errors               | Passed |
| AD Replication               | No replication failures          | Passed |
| Domain Authentication        | Successful                       | Passed |
| Domain Membership            | Windows 11 joined                | Passed |
| Group Policy Processing      | GPOs processed                   | Passed |

---

# 15. Validation Result

The Active Directory infrastructure was successfully validated.

`SRV01-DC` successfully provides Active Directory Domain Services and DNS services for the `manotech.local` domain.

The Windows 11 domain client successfully:

```text
Resolved the domain        → manotech.local
Authenticated with the DC → SRV01-DC
Joined the domain          → manotech.local
Processed Group Policies  → Successfully
```

The Organizational Unit structure, domain users, security groups, and domain controller health were also successfully verified.

This confirms that the Active Directory infrastructure is correctly configured and integrated with the internal ManoTech network.

---

## Tools Used

- Active Directory Users and Computers
- Active Directory Administrative Center
- DNS Manager
- PowerShell
- `Get-ADDomain`
- `Get-ADDomainController`
- `Get-ADUser`
- `Get-ADGroup`
- `dcdiag`
- `repadmin`
- `gpresult`
- `systeminfo`
- `Resolve-DnsName`

---

## Revision History

| Version | Date       | Author        | Description |
| ------- | ---------- | ------------- | ----------- |
| 1.0     | 2026-08-12 | Mohamed Osama | Initial Active Directory validation documentation |