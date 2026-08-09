# DHCP Troubleshooting

## Overview

This document records the DHCP-related troubleshooting scenarios encountered during the deployment of the ManoTech Enterprise Windows Server Lab.

The purpose of this document is to document DHCP configuration issues, client address assignment problems, and the troubleshooting steps used to restore proper network configuration.

---

# 1. Client Receiving Incorrect IP Configuration

## Problem

The Windows 11 client did not always receive the expected network configuration from the Windows DHCP Server.

## Symptoms

The client could receive an unexpected IP configuration instead of an address from the ManoTech DHCP scope.

This affected:

* Default gateway configuration.
* DNS configuration.
* Domain connectivity.
* Active Directory communication.

## Root Cause

The VMware virtual network had its own DHCP service enabled.

This created a conflict between:

```text
VMware DHCP
```

and:

```text
SRV02-DHCP
192.168.1.3
```

The lab was designed to use the Windows Server DHCP service as the DHCP authority for internal clients.

## Resolution

The VMware DHCP service was disabled for the internal lab network.

The Windows Server DHCP service was then used to provide client network configuration.

---

# 2. DHCP Scope Validation

The DHCP scope on `SRV02-DHCP` was checked to ensure that it was active and configured for the correct network.

### Scope

```text
Scope Name: MANOTECH-CLIENTS
Network: 192.168.1.0/24
Start: 192.168.1.100
End: 192.168.1.200
```

### Validation

The DHCP scope was checked from the DHCP Management Console.

The scope was confirmed to be active.

## Result

**Passed**

---

# 3. DHCP Server Authorization

## Problem

A Windows DHCP Server operating inside an Active Directory environment must be authorized before it can provide leases to domain clients.

## Validation

The DHCP server authorization status was checked from the DHCP Management Console.

### Server

```text
SRV02-DHCP
192.168.1.3
```

The server was authorized in Active Directory.

## Result

**Passed**

---

# 4. Incorrect Gateway or DNS Configuration

## Problem

A client may receive an IP address successfully while still having incorrect gateway or DNS settings.

## Expected DHCP Options

| Option                | Value            |
| --------------------- | ---------------- |
| 003 – Default Gateway | `192.168.1.1`    |
| 006 – DNS Server      | `192.168.1.2`    |
| 015 – DNS Domain Name | `manotech.local` |

## Validation

The client configuration was checked using:

```powershell id="20j48g"
ipconfig /all
```

The client was expected to receive:

```text
Gateway → 192.168.1.1
DNS     → 192.168.1.2
Domain  → manotech.local
```

## Result

**Passed**

---

# 5. DHCP Lease Renewal

After correcting the VMware DHCP conflict, the client needed to obtain a fresh DHCP lease.

### Release Current Lease

```powershell id="wm5b2t"
ipconfig /release
```

### Request New Lease

```powershell id="u9k5t0"
ipconfig /renew
```

### Verify Configuration

```powershell id="3w2q8n"
ipconfig /all
```

The client successfully received an address from the configured DHCP scope.

## Result

**Resolved**

---

# 6. DHCP and Active Directory Integration

DHCP was validated together with the Active Directory environment.

The client needed to receive:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
DNS Domain
```

Correct DNS configuration was particularly important because the client uses the Domain Controller for Active Directory services.

## Validation

After receiving the DHCP lease, the client was able to:

* Resolve `manotech.local`.
* Communicate with `SRV01-DC`.
* Join the Active Directory domain.
* Authenticate using domain credentials.

## Result

**Passed**

---

# 7. DHCP and RRAS Integration

The DHCP server provides the RRAS server as the default gateway.

### DHCP Option 003

```text
192.168.1.1
```

This corresponds to:

```text
SRV05-RRAS
```

The client therefore uses RRAS for routed and external traffic.

### Validation

```powershell id="5f5u3m"
ipconfig /all
```

Expected:

```text
Default Gateway: 192.168.1.1
```

## Result

**Passed**

---

# 8. Troubleshooting Methodology

The DHCP troubleshooting process followed a layered approach:

```text
Check VMware Network
        ↓
Check DHCP Service
        ↓
Check DHCP Authorization
        ↓
Check DHCP Scope
        ↓
Check DHCP Options
        ↓
Renew Client Lease
        ↓
Verify Client Configuration
        ↓
Test DNS / Gateway / Domain Connectivity
```

This approach helps determine whether the issue originates from VMware networking, the DHCP service, the scope, or the client.

---

# 9. Useful Commands

Check complete network configuration:

```powershell id="5n2q3h"
ipconfig /all
```

Release DHCP lease:

```powershell id="7q3v9g"
ipconfig /release
```

Renew DHCP lease:

```powershell id="3f2r8j"
ipconfig /renew
```

Display DNS cache:

```powershell id="d6y4i7"
ipconfig /displaydns
```

Clear DNS cache:

```powershell id="t3j9sk"
ipconfig /flushdns
```

Test gateway connectivity:

```powershell id="s2m0lq"
ping 192.168.1.1
```

Test Domain Controller connectivity:

```powershell id="l7a1kf"
ping 192.168.1.2
```

---

# 10. Lessons Learned

The DHCP troubleshooting process demonstrated several important concepts:

* Virtualization platforms can provide DHCP services that conflict with an enterprise DHCP server.
* Only the intended DHCP service should provide addresses on the internal enterprise network.
* DHCP configuration includes more than IP address assignment.
* Incorrect gateway or DNS options can cause higher-level services such as Active Directory to fail.
* DHCP should be validated together with DNS and RRAS.
* Renewing the client lease is important after changing DHCP or network configuration.

---

## Resolution Summary

| Issue                               | Status   |
| ----------------------------------- | -------- |
| VMware DHCP Conflict                | Resolved |
| DHCP Scope Validation               | Passed   |
| DHCP Authorization                  | Passed   |
| Gateway Configuration               | Passed   |
| DNS Configuration                   | Passed   |
| Client Lease Renewal                | Resolved |
| DHCP → Active Directory Integration | Passed   |
| DHCP → RRAS Integration             | Passed   |

---

## Revision History

| Version | Date       | Author        | Description                                |
| ------- | ---------- | ------------- | ------------------------------------------ |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial DHCP troubleshooting documentation |
