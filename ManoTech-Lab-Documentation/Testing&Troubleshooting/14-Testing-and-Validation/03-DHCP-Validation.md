# DHCP Validation

## Overview

This document describes the validation tests performed to verify the DHCP infrastructure within the ManoTech Enterprise Windows Server Lab.

The purpose of the validation is to confirm that DHCP01 successfully provides dynamic network configuration to domain clients, including IP addressing, subnet mask, default gateway, DNS server, and domain information.

---

## DHCP Server

| Property         | Value               |
| ---------------- | ------------------- |
| Hostname         | `SRV02-DHCP`        |
| IP Address       | `192.168.1.3`       |
| Operating System | Windows Server 2019 |
| Service          | DHCP Server         |
| DHCP Scope       | `MANOTECH-CLIENTS`  |
| Network          | `192.168.1.0/24`    |

---

# 1. DHCP Service Validation

The DHCP Server service was checked to confirm that it is running.

### Command

```powershell
Get-Service DHCPServer
```

### Expected Result

The service should have the following status:

```text
Status: Running
```

### Result

**Passed**

---

# 2. DHCP Scope Validation

The configured DHCP scope was verified.

### Scope Configuration

| Setting       | Value              |
| ------------- | ------------------ |
| Scope Name    | `MANOTECH-CLIENTS` |
| Start Address | `192.168.1.100`    |
| End Address   | `192.168.1.200`    |
| Subnet Mask   | `255.255.255.0`    |
| Network       | `192.168.1.0/24`   |

### Expected Result

The scope should be active and available for client address assignment.

### Result

**Passed**

---

# 3. DHCP Authorization Validation

Because the DHCP server operates inside an Active Directory domain, its authorization status was verified.

### Validation

The DHCP console was checked to confirm that:

* The DHCP server is authorized.
* The DHCP scope is active.
* The DHCP service is operational.

### Result

**Passed**

---

# 4. Client IP Address Assignment

The Windows 11 domain client was used to validate dynamic IP assignment.

### Command

```powershell
ipconfig /release
ipconfig /renew
```

The resulting configuration was then checked using:

```powershell
ipconfig /all
```

### Expected Result

The client should receive an address within the configured DHCP scope:

```text
192.168.1.100 - 192.168.1.200
```

### Result

**Passed**

The Windows 11 client successfully received its network configuration from DHCP01.

---

# 5. Subnet Mask Validation

The client's subnet mask was verified.

### Expected Configuration

```text
255.255.255.0
```

### Command

```powershell
ipconfig /all
```

### Result

**Passed**

The client received the correct subnet mask for the `192.168.1.0/24` network.

---

# 6. Default Gateway Validation

The DHCP-provided default gateway was verified.

### Expected Configuration

```text
192.168.1.1
```

### Command

```powershell
ipconfig /all
```

### Result

**Passed**

The client received `192.168.1.1`, which is the RRAS server and internal network gateway.

---

# 7. DNS Server DHCP Option Validation

The DNS server provided through DHCP was verified.

### Expected Configuration

```text
192.168.1.2
```

### Command

```powershell
ipconfig /all
```

### Result

**Passed**

The client received the Domain Controller DNS server address from DHCP.

---

# 8. DNS Domain Name Validation

The DHCP-provided DNS domain configuration was checked.

### Expected Configuration

```text
manotech.local
```

### Command

```powershell
ipconfig /all
```

### Result

**Passed**

The client received the expected domain configuration.

---

# 9. DHCP Lease Validation

The active lease was checked through the DHCP management console.

The following information was verified:

* Client IP address.
* Client hostname.
* Lease status.
* Lease duration.
* Client identifier.

### Expected Result

The Windows 11 client should appear as an active DHCP lease.

### Result

**Passed**

---

# 10. Client-to-DHCP Server Connectivity

Connectivity between the client and DHCP server was verified after obtaining the lease.

### Test

```powershell
ping 192.168.1.3
```

### Expected Result

DHCP01 should be reachable from the domain client.

### Result

**Passed**

---

# 11. DHCP Configuration Summary

| Test                     | Expected Result         | Status |
| ------------------------ | ----------------------- | ------ |
| DHCP Service             | Running                 | Passed |
| DHCP Scope               | Active                  | Passed |
| DHCP Authorization       | Authorized              | Passed |
| IP Assignment            | Address from DHCP scope | Passed |
| Subnet Mask              | `255.255.255.0`         | Passed |
| Default Gateway          | `192.168.1.1`           | Passed |
| DNS Server               | `192.168.1.2`           | Passed |
| DNS Domain               | `manotech.local`        | Passed |
| DHCP Lease               | Client lease created    | Passed |
| DHCP Server Connectivity | DHCP01 reachable        | Passed |

---

# 12. Validation Result

The DHCP infrastructure was successfully validated.

SRV02-DHCP successfully provides dynamic network configuration to domain clients using the `MANOTECH-CLIENTS` scope.

The Windows 11 client successfully received:

```text
IP Address      → DHCP Scope
Subnet Mask     → 255.255.255.0
Default Gateway → 192.168.1.1
DNS Server      → 192.168.1.2
Domain          → manotech.local
```

This confirms that DHCP is correctly integrated with the internal network and provides the required configuration for domain clients.

---

## Tools Used

* DHCP Manager
* `ipconfig`
* `ping`
* PowerShell
* Windows 11 Network Configuration

---

## Revision History

| Version | Date       | Author        | Description                           |
| ------- | ---------- | ------------- | ------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial DHCP validation documentation |
