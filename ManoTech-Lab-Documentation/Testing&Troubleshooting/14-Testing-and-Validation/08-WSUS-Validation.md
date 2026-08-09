# WSUS Validation

## Overview

This document describes the validation tests performed to verify the Windows Server Update Services (WSUS) infrastructure within the ManoTech Enterprise Windows Server Lab.

WSUS is hosted on the Domain Controller and provides centralized Windows Update management for domain-joined clients.

---

## WSUS Server Information

| Property         | Value                          |
| ---------------- | ------------------------------ |
| Hostname         | `SRV01-DC`                     |
| IP Address       | `192.168.1.2`                  |
| Operating System | Windows Server 2019            |
| Service          | Windows Server Update Services |
| WSUS Port        | `8530`                         |
| Domain           | `manotech.local`               |

---

# 1. WSUS Service Validation

The WSUS service was checked to verify that it is running.

### Test

```powershell
Get-Service WsusService
```

### Expected Result

```text
Status: Running
```

### Result

**Passed**

---

# 2. WSUS Administration Console

The WSUS Administration Console was opened to verify server availability and configuration.

### Validation

The following were verified:

* WSUS server is available.
* Update classifications are configured.
* Synchronization configuration is available.
* Computer groups are accessible.
* Client reporting is available.

### Result

**Passed**

---

# 3. WSUS Synchronization Validation

WSUS synchronization with Microsoft Update was verified.

### Validation

The synchronization status was checked from the WSUS Administration Console.

### Expected Result

WSUS should successfully communicate with Microsoft Update and retrieve update metadata.

### Result

**Passed**

---

# 4. WSUS Group Policy Validation

The Windows 11 domain client was configured to use the internal WSUS server through Group Policy.

### Configuration Path

```text
Computer Configuration
→ Policies
→ Administrative Templates
→ Windows Components
→ Windows Update
→ Specify intranet Microsoft update service location
```

### Configured Server

```text
http://SRV01-DC:8530
```

### Client Validation

The applied Group Policy was verified on the Windows 11 client.

```powershell
gpresult /r
```

### Result

**Passed**

The client received the WSUS configuration through Group Policy.

---

# 5. WSUS Server Connectivity

Connectivity between the Windows 11 client and the WSUS server was tested.

### Test

```powershell
Test-NetConnection 192.168.1.2 -Port 8530
```

### Expected Result

```text
TcpTestSucceeded : True
```

### Result

**Passed**

The client can communicate with the WSUS service on the configured port.

---

# 6. Windows Update Service Validation

The Windows Update service on the client was checked.

### Test

```powershell
Get-Service wuauserv
```

### Expected Result

The Windows Update service should be available and operational.

### Result

**Passed**

---

# 7. WSUS Client Registration

The Windows 11 domain client was verified in the WSUS Administration Console.

### Validation

The following information was checked:

* Client hostname.
* Last status report.
* Operating system information.
* Update status.

### Expected Result

The domain client should appear in the WSUS computer list.

### Result

**Passed**

---

# 8. Update Detection

The Windows 11 client was tested to verify communication with the WSUS infrastructure and update detection.

### Validation

The client was instructed to check for updates after receiving the WSUS Group Policy configuration.

### Expected Result

The client should communicate with the configured WSUS server and perform an update detection cycle.

### Result

**Passed**

---

# 9. Update Approval and Deployment

WSUS update management was validated using the configured update approval process.

### Validation

The following workflow was verified:

```text
Microsoft Update
        │
        ↓
     WSUS
        │
        ↓
Update Approval
        │
        ↓
Windows 11 Client
        │
        ↓
Update Installation
```

### Result

**Passed**

The WSUS infrastructure successfully provides centralized update management for the domain client.

---

# 10. WSUS Client Reporting

Client reporting was checked from the WSUS Administration Console.

### Expected Result

The Windows 11 client should report its update status to the WSUS server.

### Result

**Passed**

---

# 11. WSUS Validation Summary

| Test                   | Expected Result        | Status |
| ---------------------- | ---------------------- | ------ |
| WSUS Service           | Running                | Passed |
| WSUS Console           | Available              | Passed |
| Synchronization        | Successful             | Passed |
| WSUS Group Policy      | Applied                | Passed |
| Port 8530 Connectivity | Successful             | Passed |
| Windows Update Service | Operational            | Passed |
| Client Registration    | Client visible in WSUS | Passed |
| Update Detection       | Successful             | Passed |
| Update Management      | Functional             | Passed |
| Client Reporting       | Functional             | Passed |

---

# 12. Validation Result

The WSUS infrastructure was successfully validated.

WSUS is hosted on `SRV01-DC` and is integrated with the `manotech.local` Active Directory environment.

The Windows 11 domain client successfully received the WSUS configuration through Group Policy, communicated with the WSUS server over port `8530`, registered with the WSUS console, and reported its update status.

This confirms that centralized Windows Update management is operational within the lab environment.

---

## Tools Used

* WSUS Administration Console
* Group Policy Management Console
* PowerShell
* `gpresult`
* `Test-NetConnection`
* Windows Update
* Event Viewer

---

## Revision History

| Version | Date       | Author        | Description                           |
| ------- | ---------- | ------------- | ------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial WSUS validation documentation |
