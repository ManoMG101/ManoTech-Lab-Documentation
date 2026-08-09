# Active Directory Troubleshooting

## Overview

This document records the Active Directory troubleshooting scenarios encountered during the deployment of the ManoTech Enterprise Windows Server Lab.

The documented issues include domain join failures, DNS-related Active Directory problems, and PowerShell automation issues encountered during user creation.

---

# 1. Active Directory Domain Join Failure

## Problem

The Windows 11 client initially failed to join the Active Directory domain.

### Domain

```text
manotech.local
```

## Symptoms

* Domain join operation failed.
* The client could not consistently locate the Domain Controller.
* Active Directory services were not reachable from the client.

## Possible Causes

The main areas investigated were:

* Incorrect DNS configuration.
* Incorrect network configuration.
* Domain Controller connectivity.
* Incorrect client IP configuration.
* Time synchronization.

## Investigation

The client network configuration was checked:

```powershell
ipconfig /all
```

The client needed to use the Domain Controller as its DNS server.

### Correct DNS Configuration

```text
DNS Server: 192.168.1.2
```

Connectivity to the Domain Controller was tested:

```powershell
ping 192.168.1.2
```

DNS resolution was also tested:

```powershell
nslookup manotech.local
```

## Resolution

The client was configured to use:

```text
192.168.1.2
```

as its preferred DNS server.

After correcting the network and DNS configuration, the client was successfully joined to:

```text
manotech.local
```

## Result

**Resolved**

---

# 2. Active Directory DNS Dependency

## Problem

Active Directory domain services were not functioning correctly when the client was using an incorrect DNS server.

## Cause

Active Directory relies heavily on DNS for:

* Domain Controller discovery.
* Kerberos authentication.
* LDAP communication.
* Domain joining.
* Group Policy processing.
* Service discovery.

Using an external DNS server instead of the internal Domain Controller DNS can prevent these services from working correctly.

## Resolution

The Windows 11 domain client was configured to use:

```text
192.168.1.2
```

as its DNS server.

Validation was performed using:

```powershell
nslookup manotech.local
```

and:

```powershell
nslookup SRV01-DC
```

## Result

**Resolved**

---

# 3. PowerShell User Creation Failure

## Problem

A PowerShell script used to create multiple Active Directory users failed during bulk account creation.

## Symptoms

The script generated an error when creating users with long usernames.

## Root Cause

The generated `SamAccountName` exceeded the Active Directory naming limitation.

The `SamAccountName` attribute has a maximum length of 20 characters.

## Example

A username longer than the supported limit could cause the account creation operation to fail.

## Resolution

The PowerShell script was modified to generate shorter `SamAccountName` values while maintaining unique usernames.

The updated naming approach ensured that generated accounts remained within the Active Directory limitation.

## Validation

User creation was tested again after modifying the script.

### Result

**Resolved**

---

# 4. Active Directory Connectivity Validation

After resolving the configuration issues, Active Directory connectivity was validated.

### Domain Controller Connectivity

```powershell
ping 192.168.1.2
```

### DNS Resolution

```powershell
nslookup manotech.local
```

### Domain Information

```powershell
whoami
```

### Group Policy Refresh

```powershell
gpupdate /force
```

## Result

The domain client successfully communicated with the Active Directory environment.

---

# 5. Troubleshooting Methodology

The following troubleshooting process was used for Active Directory issues:

```text
Identify Problem
       ↓
Check Network Connectivity
       ↓
Verify DNS Configuration
       ↓
Test Domain Controller
       ↓
Check Active Directory Configuration
       ↓
Apply Corrective Action
       ↓
Re-test
       ↓
Document Result
```

This approach helps isolate whether the problem originates from the network layer, DNS, Active Directory, or client configuration.

---

# 6. Useful Commands

## Network Configuration

```powershell
ipconfig /all
```

## DNS Testing

```powershell
nslookup manotech.local
```

```powershell
nslookup SRV01-DC
```

## Connectivity Testing

```powershell
ping 192.168.1.2
```

## Group Policy Refresh

```powershell
gpupdate /force
```

## Group Policy Results

```powershell
gpresult /r
```

## Current Domain User

```powershell
whoami
```

---

# 7. Lessons Learned

The troubleshooting process demonstrated several important Active Directory administration principles:

* Active Directory depends heavily on correctly configured DNS.
* Domain clients should use the internal Domain Controller DNS server.
* Network connectivity should be verified before troubleshooting Active Directory itself.
* PowerShell automation must respect Active Directory attribute limitations.
* Testing after every configuration change helps isolate the actual cause of an issue.
* Documenting the root cause is as important as documenting the solution.

---

## Resolution Status

| Issue                                  | Status   |
| -------------------------------------- | -------- |
| Domain Join Failure                    | Resolved |
| Incorrect DNS Configuration            | Resolved |
| Active Directory Connectivity          | Resolved |
| PowerShell `SamAccountName` Limitation | Resolved |

---

## Revision History

| Version | Date       | Author        | Description                                            |
| ------- | ---------- | ------------- | ------------------------------------------------------ |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial Active Directory troubleshooting documentation |
