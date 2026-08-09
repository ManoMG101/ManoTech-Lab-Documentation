# RRAS Validation

## Overview

This document describes the validation tests performed to verify the Routing and Remote Access Service (RRAS) infrastructure within the ManoTech Enterprise Windows Server Lab.

In the current lab, RRAS is used as the internal network gateway and provides routing and NAT functionality between the enterprise network and the external VMware network.

---

## RRAS Server Information

| Property         | Value                     |
| ---------------- | ------------------------- |
| Hostname         | `SRV05-RRAS`              |
| IP Address       | `192.168.1.1`             |
| Operating System | Windows Server 2019       |
| Role             | Routing and Remote Access |
| Internal Network | `192.168.1.0/24`          |
| Domain           | `manotech.local`          |

---

# 1. RRAS Service Validation

The RRAS service was checked to verify that the routing service is operational.

### Test

```powershell id="kz7s5b"
Get-Service RemoteAccess
```

### Expected Result

```text id="r72m2f"
Status: Running
```

### Result

**Passed**

---

# 2. Internal Interface Validation

The internal network interface was verified.

### Expected Configuration

```text id="5em8dy"
Network: 192.168.1.0/24
Gateway: 192.168.1.1
```

### Validation

The RRAS server network configuration was checked using:

```powershell id="q8v8l7"
ipconfig /all
```

### Result

**Passed**

The internal interface is configured with `192.168.1.1`.

---

# 3. Client Default Gateway Validation

The Windows 11 domain client was checked to confirm that DHCP provides RRAS as the default gateway.

### Command

```powershell id="b0k3hc"
ipconfig /all
```

### Expected Result

```text id="3a6u1f"
Default Gateway: 192.168.1.1
```

### Result

**Passed**

The client successfully receives RRAS as its default gateway.

---

# 4. RRAS Server Connectivity

Connectivity between the Windows 11 client and the RRAS server was tested.

### Test

```powershell id="qg3b8w"
ping 192.168.1.1
```

### Expected Result

The RRAS server should respond successfully.

### Result

**Passed**

---

# 5. Internal Network Routing

Communication between the domain client and internal servers was tested through the configured network.

### Tests

```powershell id="7n6c0c"
ping 192.168.1.2
ping 192.168.1.3
ping 192.168.1.4
ping 192.168.1.5
```

### Expected Result

Internal servers should be reachable from the domain client.

### Result

**Passed**

---

# 6. NAT Validation

RRAS NAT functionality was validated to confirm that internal traffic can be translated through the external interface.

### Validation

The RRAS NAT configuration was checked to confirm:

* Internal interface configured for the private network.
* External interface configured for the VMware NAT network.
* NAT enabled on the appropriate interfaces.

### Result

**Passed**

---

# 7. External Connectivity

The Windows 11 client was tested for external network connectivity through RRAS.

### Test

```powershell id="7l8f2d"
ping 8.8.8.8
```

### Expected Result

The client should be able to reach an external IP through the RRAS gateway when external connectivity is available.

### Result

**Passed**

---

# 8. DNS-Based External Connectivity

External connectivity was additionally tested using hostname resolution.

### Test

```powershell id="11g1p0"
nslookup google.com
```

### Expected Result

The client should successfully resolve an external hostname using the configured DNS path.

### Result

**Passed**

---

# 9. Routing Path Validation

The network path from the client toward an external destination was inspected.

### Test

```powershell id="g8a0wo"
tracert 8.8.8.8
```

### Expected Result

The first hop should be the internal RRAS gateway:

```text id="eqv3qk"
192.168.1.1
```

### Result

**Passed**

This confirms that the client uses RRAS as its default gateway for external traffic.

---

# 10. RRAS and DHCP Integration

The integration between DHCP and RRAS was validated.

The DHCP server provides:

```text id="84c7yx"
Default Gateway → 192.168.1.1
DNS Server      → 192.168.1.2
```

The client therefore automatically uses RRAS for routed traffic.

### Result

**Passed**

---

# 11. RRAS Validation Summary

| Test                    | Expected Result   | Status |
| ----------------------- | ----------------- | ------ |
| RRAS Service            | Running           | Passed |
| Internal Interface      | `192.168.1.1`     | Passed |
| Client Gateway          | `192.168.1.1`     | Passed |
| RRAS Connectivity       | Reachable         | Passed |
| Internal Routing        | Functional        | Passed |
| NAT                     | Functional        | Passed |
| External Connectivity   | Successful        | Passed |
| External DNS Resolution | Successful        | Passed |
| Routing Path            | RRAS is first hop | Passed |
| DHCP/RRAS Integration   | Functional        | Passed |

---

# 12. Validation Result

The RRAS infrastructure was successfully validated.

`SRV05-RRAS` operates as the default gateway for the `192.168.1.0/24` enterprise network and provides routing and NAT functionality for internal clients.

The Windows 11 domain client successfully:

* Received `192.168.1.1` as its default gateway.
* Communicated with internal servers.
* Used RRAS for external network traffic.
* Successfully reached external destinations through the configured NAT path.

VPN and remote-access functionality are not included in the current implementation and may be considered for a future phase.

---

## Tools Used

* Routing and Remote Access Console
* PowerShell
* `ipconfig`
* `ping`
* `tracert`
* `nslookup`

---

## Revision History

| Version | Date       | Author        | Description                           |
| ------- | ---------- | ------------- | ------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial RRAS validation documentation |
