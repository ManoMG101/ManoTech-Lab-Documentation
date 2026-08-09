# Network Connectivity Validation

## Overview

This document describes the network connectivity tests performed within the ManoTech Enterprise Windows Server Lab.

The purpose of these tests is to verify that the virtual machines can communicate correctly across the internal VMware network and that the basic network infrastructure is functioning before validating higher-level services.

---

## Network Environment

The lab uses an isolated VMware virtual network based on the following addressing scheme:

| Setting         | Value            |
| --------------- | ---------------- |
| Network Address | `192.168.1.0/24` |
| Subnet Mask     | `255.255.255.0`  |
| Default Gateway | `192.168.1.1`    |
| DNS Server      | `192.168.1.2`    |

---

## Infrastructure Devices

| Hostname        |    IP Address | Role                    |
| --------------- | ------------: | ----------------------- |
| SRV05-RRAS      | `192.168.1.1` | Gateway / Router        |
| SRV01-DC        | `192.168.1.2` | Domain Controller / DNS |
| SRV02-DHCP      | `192.168.1.3` | DHCP Server             |
| SRV03-FILESRV   | `192.168.1.4` | File Server             |
| SRV04-WEBSERVER | `192.168.1.5` | IIS Web Server          |
| WIN11-CLIENT    |          DHCP | Domain Client           |
| KALI-LINUX      |          DHCP | Security Testing        |

---

# 1. Basic IP Configuration

The IP configuration of each system was verified to ensure that devices were operating within the expected subnet.

### Command

```powershell
ipconfig /all
```

### Validation

The following parameters were checked:

* IPv4 address.
* Subnet mask.
* Default gateway.
* DNS server.
* Network adapter status.

### Expected Result

Infrastructure servers should use their assigned static IP addresses, while domain clients should receive their configuration through DHCP.

### Result

**Passed**

---

# 2. Server-to-Server Connectivity

Connectivity between the main infrastructure servers was tested using ICMP.

### Tests

```powershell
ping 192.168.1.1
ping 192.168.1.2
ping 192.168.1.3
ping 192.168.1.4
ping 192.168.1.5
```

### Expected Result

The infrastructure servers should be able to communicate across the internal `192.168.1.0/24` network.

### Result

**Passed**

---

# 3. Client-to-Server Connectivity

The Windows 11 domain client was tested against the main infrastructure servers.

### Tests

```powershell
ping 192.168.1.1
ping 192.168.1.2
ping 192.168.1.3
ping 192.168.1.4
ping 192.168.1.5
```

### Expected Result

The client should be able to reach the infrastructure servers over the internal network.

### Result

**Passed**

---

# 4. Default Gateway Validation

The Windows 11 client configuration was checked to verify that RRAS was being used as the default gateway.

### Command

```powershell
ipconfig /all
```

### Expected Configuration

```text
Default Gateway: 192.168.1.1
```

### Connectivity Test

```powershell
ping 192.168.1.1
```

### Result

**Passed**

The client successfully communicated with the RRAS gateway.

---

# 5. Network Path Testing

Network paths were tested using `tracert` where required.

### Command

```powershell
tracert 192.168.1.1
```

Additional destinations can be tested using:

```powershell
tracert <destination>
```

### Purpose

This test helps identify:

* Routing problems.
* Incorrect gateways.
* Unexpected network paths.
* Connectivity interruptions.

### Result

**Passed**

---

# 6. Network Adapter Validation

The network adapters of the virtual machines were checked to ensure that they were connected to the correct VMware virtual network.

The following were verified:

* Correct VMware network adapter.
* Adapter enabled.
* Correct IP configuration.
* Correct subnet.
* No conflicting network configuration.

### Result

**Passed**

All required virtual machines were connected to the intended internal lab network.

---

# 7. Connectivity Test Summary

| Test                          | Expected Result          | Status |
| ----------------------------- | ------------------------ | ------ |
| Server IP Configuration       | Correct addressing       | Passed |
| Server-to-Server Connectivity | Communication successful | Passed |
| Client-to-Server Connectivity | Communication successful | Passed |
| Default Gateway               | `192.168.1.1` reachable  | Passed |
| Network Path                  | Correct routing          | Passed |
| VMware Network Adapter        | Correct virtual network  | Passed |

---

# 8. Validation Result

The internal VMware network was successfully validated.

The infrastructure systems operate within the `192.168.1.0/24` network and are able to communicate with each other.

The Windows 11 domain client can communicate with the core infrastructure and uses `192.168.1.1` as its default gateway.

This confirms that the basic network layer is functioning correctly before testing higher-level services such as DNS, DHCP, Active Directory, IIS, AD CS, and WSUS.

---

## Tools Used

* `ipconfig`
* `ping`
* `tracert`
* VMware Workstation
* Windows Network Configuration

---

## Revision History

| Version | Date       | Author        | Description                             |
| ------- | ---------- | ------------- | --------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial network connectivity validation |
