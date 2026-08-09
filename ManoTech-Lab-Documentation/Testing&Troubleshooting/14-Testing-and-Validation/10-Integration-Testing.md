# Integration Testing

## Overview

This document describes the end-to-end integration tests performed to verify that the major infrastructure services within the ManoTech Enterprise Windows Server Lab operate correctly together.

The purpose of integration testing is to validate the dependencies and communication between core enterprise services rather than testing each service independently.

---

# 1. Network Initialization

The Windows 11 domain client was started without a manually configured IP address.

The client was expected to obtain its network configuration automatically from DHCP.

### Expected Flow

```text
Windows 11 Client
        │
        ↓
DHCP Server
(SRV02-DHCP)
        │
        ↓
IP / Gateway / DNS
        │
        ↓
Client Network Configuration
```

### Expected Configuration

```text
IP Address      → DHCP Scope
Subnet Mask     → 255.255.255.0
Gateway         → 192.168.1.1
DNS Server      → 192.168.1.2
Domain          → manotech.local
```

### Result

**Passed**

---

# 2. DNS and Active Directory Integration

The client was tested to verify that DNS allows it to locate and communicate with the Active Directory environment.

### Tests

```powershell
nslookup manotech.local
nslookup SRV01-DC
```

### Expected Result

The domain and Domain Controller should resolve through the internal DNS server.

### Result

**Passed**

---

# 3. Active Directory Authentication

A domain user was used to authenticate against the Active Directory domain.

### Domain

```text
manotech.local
```

### Expected Result

The user should successfully authenticate using domain credentials.

### Result

**Passed**

---

# 4. Active Directory and File Server Integration

The authenticated domain user was tested against the File Server.

### Test

```text
\\SRV03-FILESRV
```

Departmental shares were accessed according to the user's Active Directory security group membership.

### Expected Result

* Authorized resources are accessible.
* Unauthorized resources are denied.
* NTFS permissions are respected.

### Result

**Passed**

---

# 5. Active Directory and IIS Integration

The domain client was tested for access to the internal IIS website.

### Test

```text
http://www.manotech.local
```

### Expected Flow

```text
Windows 11 Client
        │
        ↓
DNS
        │
        ↓
www.manotech.local
        │
        ↓
192.168.1.5
        │
        ↓
IIS
```

### Result

**Passed**

The client successfully resolved the website hostname and accessed the internal website.

---

# 6. Active Directory Certificate Services Integration

Certificate enrollment was tested from the domain environment.

### Expected Flow

```text
Domain User / Computer
        │
        ↓
Certificate Template
        │
        ↓
Active Directory
        │
        ↓
ManoTech-ROOT-CA
        │
        ↓
Issued Certificate
```

### Expected Result

The domain environment should allow eligible users or computers to request and receive certificates.

### Result

**Passed**

---

# 7. AD CS and Group Policy Integration

Certificate auto-enrollment configuration was validated through Group Policy.

### Test

```powershell
gpupdate /force
```

The applicable certificate auto-enrollment settings were then verified.

### Expected Result

Eligible domain users or computers should process the certificate enrollment policy.

### Result

**Passed**

---

# 8. Active Directory and WSUS Integration

The Windows 11 domain client was tested to verify that it receives WSUS configuration through Group Policy.

### WSUS Server

```text
http://SRV01-DC:8530
```

### Expected Flow

```text
Active Directory
        │
        ↓
Group Policy
        │
        ↓
Windows 11 Client
        │
        ↓
WSUS on SRV01-DC
        │
        ↓
Windows Updates
```

### Result

**Passed**

The client successfully received the WSUS configuration and communicated with the WSUS server.

---

# 9. DHCP and RRAS Integration

The DHCP configuration was validated together with RRAS.

### DHCP Configuration

```text
Default Gateway → 192.168.1.1
DNS Server      → 192.168.1.2
```

### Expected Flow

```text
DHCP
 │
 ├── Gateway → RRAS
 │
 └── DNS → Domain Controller
```

### Result

**Passed**

The client automatically received RRAS as its default gateway and the Domain Controller as its DNS server.

---

# 10. RRAS and External Connectivity

External connectivity was tested through the RRAS gateway.

### Test

```powershell
ping 8.8.8.8
```

### Routing Path

```text
Windows 11 Client
        │
        ↓
192.168.1.1
        │
        ↓
RRAS
        │
        ↓
NAT
        │
        ↓
External Network
```

### Result

**Passed**

The client successfully used RRAS for external network connectivity.

---

# 11. Complete End-to-End Test

A complete client workflow was tested from network initialization through access to enterprise resources.

### Test Workflow

```text
                    ┌─────────────────┐
                    │ Windows 11      │
                    │ Domain Client   │
                    └────────┬────────┘
                             │
                             ↓
                    ┌─────────────────┐
                    │ DHCP01          │
                    │ 192.168.1.3     │
                    └────────┬────────┘
                             │
                   IP / Gateway / DNS
                             │
                             ↓
                    ┌─────────────────┐
                    │ DC01            │
                    │ 192.168.1.2     │
                    │ AD DS / DNS     │
                    │ AD CS / WSUS    │
                    └─────┬─────┬─────┘
                          │     │
                ┌─────────┘     └─────────┐
                ↓                         ↓
        ┌───────────────┐         ┌───────────────┐
        │ FILE01        │         │ WEB01         │
        │ File Services │         │ IIS           │
        └───────────────┘         └───────────────┘
                          │
                          ↓
                    ┌───────────────┐
                    │ RRAS01        │
                    │ 192.168.1.1   │
                    │ NAT / Routing │
                    └───────┬───────┘
                            │
                            ↓
                     External Network
```

### End-to-End Validation

The following operations were verified:

* Client receives an IP address from DHCP.
* Client receives the correct DNS server.
* Client receives RRAS as its default gateway.
* DNS resolves the internal domain.
* Domain authentication works.
* File Server resources are accessible according to permissions.
* Internal IIS website is accessible.
* Certificate enrollment works.
* WSUS configuration is received through Group Policy.
* External connectivity works through RRAS.

### Result

**Passed**

---

# 12. Integration Test Summary

| Integration                       | Result |
| --------------------------------- | ------ |
| DHCP → Windows 11 Client          | Passed |
| DHCP → DNS Configuration          | Passed |
| DHCP → RRAS Gateway               | Passed |
| DNS → Active Directory            | Passed |
| Active Directory → Authentication | Passed |
| Active Directory → File Server    | Passed |
| DNS → IIS                         | Passed |
| AD CS → Active Directory          | Passed |
| AD CS → Group Policy              | Passed |
| Active Directory → WSUS           | Passed |
| Group Policy → WSUS Client        | Passed |
| RRAS → External Network           | Passed |
| Complete End-to-End Workflow      | Passed |

---

# 13. Final Integration Result

The major infrastructure components of the ManoTech Enterprise Windows Server Lab were successfully integrated and validated.

The testing confirmed that the services operate together as an enterprise environment rather than as isolated server roles.

The current integrated architecture provides:

* Centralized identity management through Active Directory.
* Automatic network configuration through DHCP.
* Internal name resolution through DNS.
* Centralized file services.
* Internal web hosting through IIS.
* Enterprise PKI through AD CS.
* Centralized Windows Update management through WSUS.
* Network routing and NAT through RRAS.

The integration testing confirms that the core infrastructure is operational and ready for the remaining Active Directory, Group Policy, and security hardening phases.

---

## Tools Used

* PowerShell
* `ipconfig`
* `ping`
* `nslookup`
* `tracert`
* `gpupdate`
* `gpresult`
* File Explorer
* Web Browser
* DHCP Manager
* DNS Manager
* Active Directory Users and Computers
* Certification Authority Console
* WSUS Administration Console
* RRAS Management Console
* IIS Manager

---

## Revision History

| Version | Date       | Author        | Description                               |
| ------- | ---------- | ------------- | ----------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial integration testing documentation |
