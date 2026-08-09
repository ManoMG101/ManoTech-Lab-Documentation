# Routing and Remote Access (RRAS)

## Overview

Routing and Remote Access Service (RRAS) is a Windows Server role that provides routing and remote access capabilities.

In the ManoTech Enterprise Windows Server Lab, RRAS is deployed as the network gateway for the internal enterprise network.

RRAS provides routing and Network Address Translation (NAT), allowing internal clients and servers to communicate with external networks.

---

## Server Information

| Property         | Value                     |
| ---------------- | ------------------------- |
| Hostname         | RRAS01                    |
| IP Address       | `192.168.1.1`             |
| Operating System | Windows Server 2019       |
| Role             | Routing and Remote Access |
| Domain           | `manotech.local`          |

---

## Objectives

RRAS was deployed to:

* Provide routing services for the internal network.
* Act as the default gateway for internal clients.
* Provide NAT for outbound network connectivity.
* Forward traffic between network interfaces.
* Simulate an enterprise network gateway.
* Provide a foundation for potential future VPN functionality.

---

## Network Configuration

RRAS uses separate network interfaces for internal and external connectivity.

| Interface    | Network            | Addressing    | Purpose                  |
| ------------ | ------------------ | ------------- | ------------------------ |
| Internal NIC | `192.168.1.0/24`   | `192.168.1.1` | Internal Network Gateway |
| External NIC | VMware NAT Network | DHCP          | External Network Access  |

The internal interface provides the default gateway for domain clients and infrastructure systems.

---

## RRAS Architecture

The current network flow is:

```text id="g8r2mk"
                    External Network
                           │
                           ▼
                    External NIC
                           │
                    ┌─────────────┐
                    │   RRAS01    │
                    │ 192.168.1.1 │
                    │     NAT     │
                    └──────┬──────┘
                           │
                    Internal Network
                    192.168.1.0/24
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
       DC01              FILE01             WEB01
   192.168.1.2        192.168.1.4        192.168.1.5
        │
      DHCP01
   192.168.1.3
```

RRAS01 acts as the gateway between the internal enterprise network and the external VMware network.

---

## RRAS Configuration

The following configuration was implemented:

* Remote Access role installed.
* Routing and Remote Access enabled.
* NAT configured.
* Internal network interface configured.
* External network interface configured.
* Internal routing verified.
* Default gateway functionality verified.

The RRAS configuration allows internal systems to use `192.168.1.1` as their default gateway.

---

## Routing Function

RRAS provides the following network functions:

* Default gateway services.
* Network Address Translation (NAT).
* Packet forwarding.
* Routing between network interfaces.
* External connectivity for internal systems.

The internal network uses:

```text id="m5q8s1"
192.168.1.0/24
```

with RRAS01 as the gateway:

```text id="z2v6pd"
192.168.1.1
```

---

## DHCP Integration

DHCP01 provides the default gateway information to domain clients.

The relevant DHCP configuration is:

| DHCP Option     | Value            |
| --------------- | ---------------- |
| Default Gateway | `192.168.1.1`    |
| DNS Server      | `192.168.1.2`    |
| DNS Domain      | `manotech.local` |

This allows clients to automatically use RRAS01 as their network gateway.

---

## Client Network Flow

A typical client connection follows:

```text id="w7h3kx"
Windows 11 Client
        │
        ▼
   DHCP01
        │
        │ Network Configuration
        ▼
Default Gateway: 192.168.1.1
        │
        ▼
     RRAS01
        │
        ▼
      NAT
        │
        ▼
 External Network
```

Internal communication between domain resources does not require NAT and remains within the internal network.

---

## Validation

RRAS functionality was validated using network connectivity tests.

### Gateway Validation

The Windows 11 client was checked to verify that it received:

```text id="t3n8vp"
Default Gateway: 192.168.1.1
```

This can be verified using:

```powershell id="s6q2md"
ipconfig /all
```

### Internal Connectivity

Internal connectivity was tested against infrastructure servers:

```powershell id="c8r5wa"
ping 192.168.1.2
ping 192.168.1.3
ping 192.168.1.4
ping 192.168.1.5
```

### Gateway Connectivity

The client can test communication with RRAS01 using:

```powershell id="f2k9xb"
ping 192.168.1.1
```

### External Connectivity

Where external connectivity is available, outbound communication can be tested from the internal client to verify NAT functionality.

---

## Troubleshooting

### Issue 1 — Client Cannot Access External Networks

#### Possible Causes

* Incorrect default gateway.
* RRAS service stopped.
* NAT configuration incorrect.
* External NIC unavailable.
* Incorrect VMware network configuration.
* Firewall restrictions.

#### Resolution

The following were verified:

1. Client default gateway.
2. RRAS service status.
3. Internal NIC configuration.
4. External NIC configuration.
5. NAT configuration.
6. VMware network connectivity.

The client configuration was checked using:

```powershell id="g9m3tv"
ipconfig /all
```

---

### Issue 2 — Client Receives an Incorrect Gateway

#### Possible Causes

* Incorrect DHCP Option 003.
* Old DHCP lease.
* Incorrect DHCP scope configuration.

#### Resolution

DHCP Option 003 was verified to contain:

```text id="h6c1wr"
192.168.1.1
```

The client lease was then renewed:

```powershell id="r4x8nb"
ipconfig /release
ipconfig /renew
```

---

### Issue 3 — Internal Servers Are Unreachable

#### Possible Causes

* Incorrect IP configuration.
* RRAS routing issue.
* Network adapter configuration.
* VMware virtual network mismatch.
* Firewall configuration.

#### Resolution

Internal connectivity was tested using direct IP addresses and the network configuration of the affected systems was verified.

---

## Security Considerations

RRAS is a critical network boundary component.

Recommended security practices include:

* Restrict administrative access.
* Disable unnecessary routing services.
* Configure firewall rules appropriately.
* Use secure VPN protocols for remote access.
* Monitor network traffic.
* Keep Windows Server updated.
* Separate internal and external network interfaces.
* Avoid exposing internal services directly to external networks.

---

## Future Expansion

Potential future RRAS improvements include:

* VPN Remote Access.
* Site-to-Site VPN.
* Additional network segments.
* VLAN-based network segmentation.
* Advanced firewall integration.
* More complex routing scenarios.

These features are not part of the current implemented architecture.

---

## Notes

RRAS01 provides the gateway and NAT functionality for the ManoTech internal network.

The current network architecture uses:

```text id="q1v7km"
RRAS01 → 192.168.1.1
DC01   → 192.168.1.2
DHCP01 → 192.168.1.3
FILE01 → 192.168.1.4
WEB01  → 192.168.1.5
```

RRAS therefore acts as the network gateway between the isolated enterprise network and the external VMware network.

---

## Revision History

| Version | Date       | Author        | Description                                                                                |
| ------- | ---------- | ------------- | ------------------------------------------------------------------------------------------ |
| 1.0     | 2026-07-26 | Mohamed Osama | Initial documentation                                                                      |
| 1.1     | 2026-08-04 | Mohamed Osama | Reviewed and updated documentation                                                         |
| 1.2     | 2026-08-09 | Mohamed Osama | Updated server naming, RRAS architecture, NAT, DHCP integration, and validation procedures |
