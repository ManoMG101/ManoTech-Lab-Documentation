# VMware Networking Troubleshooting

## Overview

This document describes the VMware networking issues encountered during the deployment of the ManoTech Enterprise Windows Server Lab.

The issue affected communication between virtual machines and initially caused incorrect network configuration and connectivity problems within the lab environment.

---

## Environment

| Property                | Value                    |
| ----------------------- | ------------------------ |
| Virtualization Platform | VMware Workstation Pro   |
| Virtual Network         | VMware Host-Only Network |
| Network Address         | `192.168.1.0/24`         |
| Gateway                 | `192.168.1.1`            |
| DNS Server              | `192.168.1.2`            |
| DHCP Server             | `192.168.1.3`            |

The Windows Server infrastructure uses its own DHCP server to provide IP configuration to domain clients.

---

# 1. Problem

The virtual machines initially experienced network communication problems.

Some machines were receiving unexpected network configurations or were unable to communicate correctly with the rest of the enterprise environment.

This affected:

* Server-to-server communication.
* Client connectivity.
* DHCP operation.
* Active Directory communication.
* Domain joining.
* DNS resolution.

---

# 2. Symptoms

The following symptoms were observed:

* Virtual machines were not consistently placed on the expected subnet.
* Clients could receive IP addresses from an unintended DHCP source.
* Communication between virtual machines failed in some configurations.
* Domain services were not consistently reachable.
* Network-dependent services such as Active Directory and DNS were affected.

---

# 3. Root Cause

The main cause was an incorrect VMware virtual network configuration combined with VMware's own DHCP service interfering with the lab's dedicated Windows DHCP Server.

The enterprise lab was designed to use:

```text
SRV02-DHCP
192.168.1.3
```

as the DHCP server for internal clients.

However, VMware DHCP could assign addresses independently if it remained enabled on the virtual network.

---

# 4. Resolution

The VMware virtual network configuration was reviewed and corrected.

### Step 1 – Verify VMware Virtual Network

The virtual machines were configured to use the same VMware Host-Only network.

The subnet was configured as:

```text
192.168.1.0/24
```

---

### Step 2 – Disable VMware DHCP

VMware's built-in DHCP service was disabled for the internal lab network.

This prevented VMware from competing with the Windows DHCP Server.

The Windows DHCP Server became the authoritative DHCP service for the enterprise clients.

---

### Step 3 – Verify Server Network Configuration

The infrastructure servers were configured with their intended addresses:

```text
RRAS01 → 192.168.1.1
DC01   → 192.168.1.2
DHCP01 → 192.168.1.3
FILE01 → 192.168.1.4
WEB01  → 192.168.1.5
```

---

### Step 4 – Verify Client DHCP

The Windows 11 client was configured to obtain its network configuration automatically.

The client was then renewed:

```powershell id="4hx2al"
ipconfig /release
ipconfig /renew
```

The resulting configuration was checked:

```powershell id="efr3gn"
ipconfig /all
```

---

# 5. Validation

After correcting the VMware network configuration, the following tests were performed.

### Network Connectivity

```powershell id="q8x1d0"
ping 192.168.1.2
ping 192.168.1.3
ping 192.168.1.4
ping 192.168.1.5
```

### DNS

```powershell id="3c0xla"
nslookup manotech.local
```

### Client Configuration

```powershell id="6qf1at"
ipconfig /all
```

### Expected Result

The client should receive:

```text
IP Address      → DHCP Scope
Subnet Mask     → 255.255.255.0
Gateway         → 192.168.1.1
DNS Server      → 192.168.1.2
Domain          → manotech.local
```

### Result

**Resolved**

The virtual machines successfully communicated using the intended `192.168.1.0/24` network.

---

# 6. Final Network Design

After troubleshooting, the VMware network operates as follows:

```text
                    VMware Host-Only Network
                         192.168.1.0/24
                                │
          ┌─────────────────────┼─────────────────────┐
          │                     │                     │
          ↓                     ↓                     ↓
     SRV05-RRAS              SRV01-DC             SRV02-DHCP
     192.168.1.1             192.168.1.2           192.168.1.3
     Gateway                 AD DS / DNS           DHCP
          │
          │
          ├────────────── SRV03-FILESRV
          │               192.168.1.4
          │
          ├────────────── SRV04-WEBSERVER
          │               192.168.1.5
          │
          └────────────── Windows 11 Client
                          DHCP
```

---

# 7. Lessons Learned

The troubleshooting process demonstrated several important virtualization and networking concepts:

* VMware virtual networking must be planned before deploying network services.
* Only one DHCP authority should normally serve the internal lab network.
* Server infrastructure should use predictable static IP addresses.
* Domain environments depend heavily on correct network connectivity and DNS.
* Virtual network configuration should be validated before troubleshooting higher-level services.
* DHCP, DNS, Active Directory, and RRAS must be considered together when diagnosing connectivity problems.

---

## Resolution Status

**Resolved**

The VMware networking configuration was corrected and the enterprise lab successfully operates on the intended `192.168.1.0/24` network.

---

## Revision History

| Version | Date       | Author        | Description                                             |
| ------- | ---------- | ------------- | ------------------------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial VMware networking troubleshooting documentation |
