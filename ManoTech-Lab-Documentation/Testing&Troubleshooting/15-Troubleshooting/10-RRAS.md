# RRAS Troubleshooting

## Overview

This document records the Routing and Remote Access Service (RRAS) troubleshooting scenarios encountered during the deployment of the ManoTech Enterprise Windows Server Lab.

The troubleshooting focused on network interfaces, routing, NAT, default gateway configuration, and connectivity between the internal lab network and external networks.

---

# 1. VMware Network Configuration Issue

## Problem

The virtual machines initially experienced network connectivity problems because the VMware virtual networking configuration was not correctly aligned with the lab network design.

## Symptoms

* Virtual machines could not communicate consistently.
* Clients received incorrect network configuration.
* Internal services were not always reachable.
* Routing and domain communication were affected.

## Possible Causes

* Incorrect VMware virtual network selection.
* Incorrect subnet configuration.
* VMware DHCP service interfering with the Windows DHCP Server.
* Incorrect network adapter configuration.

## Resolution

The VMware virtual network configuration was reviewed and corrected.

The internal lab network was configured as:

```text id="v6u6a8"
192.168.1.0/24
```

The internal RRAS interface was configured as:

```text id="4yujf5"
192.168.1.1
```

The VMware DHCP service was disabled for the internal network so that `SRV02-DHCP` could provide DHCP services.

## Result

**Resolved**

---

# 2. RRAS Default Gateway Configuration

## Problem

Domain clients required a consistent default gateway to communicate with networks outside the local subnet.

## Configuration

The RRAS server was configured as:

```text id="r0h8v3"
SRV05-RRAS
192.168.1.1
```

The DHCP Server was configured to provide:

```text id="2cb9v0"
Default Gateway: 192.168.1.1
```

## Validation

The Windows 11 client configuration was checked using:

```powershell id="u8z3cm"
ipconfig /all
```

Expected result:

```text id="e5b0qv"
Default Gateway: 192.168.1.1
```

## Result

**Passed**

---

# 3. NAT Configuration

## Problem

Internal clients needed access to external networks through the RRAS server.

## Possible Causes

* Incorrect external interface.
* Incorrect NAT configuration.
* Incorrect default gateway.
* Incorrect VMware external network configuration.

## Investigation

The RRAS configuration was reviewed to verify:

* Internal interface.
* External interface.
* NAT configuration.
* Routing configuration.
* Client default gateway.

## Resolution

RRAS NAT was configured with the internal interface and the external VMware network interface.

The internal network uses:

```text id="t9d2s4"
192.168.1.0/24
```

with RRAS acting as the gateway:

```text id="p2v3m7"
192.168.1.1
```

## Result

**Validated**

---

# 4. Client Receiving Incorrect Gateway

## Problem

A client may receive an incorrect gateway when DHCP options or an old lease are still being used.

## Investigation

The client DHCP configuration was reviewed:

```powershell id="t9e1ag"
ipconfig /all
```

The DHCP configuration was expected to provide:

```text id="7r0m3k"
Gateway: 192.168.1.1
DNS:     192.168.1.2
```

## Resolution

The DHCP lease was renewed:

```powershell id="2h0d3z"
ipconfig /release
```

```powershell id="1x0f5a"
ipconfig /renew
```

The resulting configuration was then verified.

## Result

**Resolved**

---

# 5. Internal Connectivity Testing

Before troubleshooting external routing, internal connectivity was verified.

### RRAS

```powershell id="l0j3qx"
ping 192.168.1.1
```

### Domain Controller

```powershell id="0n0wz5"
ping 192.168.1.2
```

### DHCP Server

```powershell id="0t4s5d"
ping 192.168.1.3
```

### File Server

```powershell id="d6f7k3"
ping 192.168.1.4
```

### Web Server

```powershell id="e5j7z9"
ping 192.168.1.5
```

Successful internal communication confirmed that the basic internal routing path was operational.

---

# 6. RRAS and DHCP Integration

RRAS and DHCP were validated together because DHCP provides the default gateway used by clients.

The expected traffic path is:

```text id="w7x1hm"
Windows 11 Client
        ↓
DHCP Configuration
        ↓
Default Gateway
192.168.1.1
        ↓
SRV05-RRAS
        ↓
External Network
```

The client configuration was checked using:

```powershell id="x4x3z1"
ipconfig /all
```

## Result

**Passed**

---

# 7. Troubleshooting Methodology

The RRAS troubleshooting process followed this sequence:

```text id="x4s7de"
Check VMware Network
        ↓
Check Network Adapters
        ↓
Check IP Configuration
        ↓
Check DHCP Gateway
        ↓
Check RRAS Service
        ↓
Check NAT Configuration
        ↓
Test Internal Connectivity
        ↓
Test External Connectivity
```

This approach helps identify whether the problem originates from VMware networking, DHCP, RRAS, NAT, or the client configuration.

---

# 8. Useful Commands

Display network configuration:

```powershell id="q3f6h2"
ipconfig /all
```

Release DHCP lease:

```powershell id="r7b1m0"
ipconfig /release
```

Renew DHCP lease:

```powershell id="h3v8p2"
ipconfig /renew
```

Test RRAS gateway:

```powershell id="j5g9d1"
ping 192.168.1.1
```

Trace network path:

```powershell id="v2k4s8"
tracert 8.8.8.8
```

Check RRAS-related services:

```powershell id="m4z6q0"
Get-Service RemoteAccess
```

---

# 9. Lessons Learned

The RRAS troubleshooting process demonstrated several important networking concepts:

* VMware virtual networking must match the physical design of the lab.
* Only the intended DHCP server should provide addresses on the internal network.
* RRAS depends on correct interface configuration.
* DHCP Option 003 must point clients to the correct gateway.
* NAT requires a correctly configured internal and external interface.
* Internal connectivity should be verified before troubleshooting external routing.
* DHCP, DNS, and RRAS must work together for reliable client connectivity.

---

## Resolution Summary

| Issue                        | Status    |
| ---------------------------- | --------- |
| VMware Network Configuration | Resolved  |
| RRAS Default Gateway         | Passed    |
| NAT Configuration            | Validated |
| Incorrect Client Gateway     | Resolved  |
| Internal Connectivity        | Passed    |
| RRAS + DHCP Integration      | Passed    |

---

## Revision History

| Version | Date       | Author        | Description                                |
| ------- | ---------- | ------------- | ------------------------------------------ |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial RRAS troubleshooting documentation |
