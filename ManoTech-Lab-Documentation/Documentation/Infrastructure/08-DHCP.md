# DHCP Server

## Overview

Dynamic Host Configuration Protocol (DHCP) is responsible for automatically assigning network configuration parameters to client devices within the ManoTech enterprise network.

In this lab, DHCP provides centralized IP address management and automatically delivers network settings such as IP address, subnet mask, default gateway, DNS server, and DNS domain information to client devices.

---

## Server Information

| Property         | Value               |
| ---------------- | ------------------- |
| Hostname         | DHCP01              |
| IP Address       | `192.168.1.3`       |
| Operating System | Windows Server 2019 |
| Service          | DHCP Server         |
| Domain           | `manotech.local`    |

---

## Objectives

The DHCP infrastructure was implemented to:

* Automatically assign IP addresses to client devices.
* Reduce manual network configuration.
* Provide centralized IP address management.
* Configure the default gateway automatically.
* Configure the internal DNS server automatically.
* Provide DNS domain information to clients.
* Support Active Directory domain clients.

---

## DHCP Configuration

| Setting         | Value            |
| --------------- | ---------------- |
| Network Address | `192.168.1.0/24` |
| Subnet Mask     | `255.255.255.0`  |
| DHCP Server IP  | `192.168.1.3`    |
| Default Gateway | `192.168.1.1`    |
| DNS Server      | `192.168.1.2`    |
| DNS Domain      | `manotech.local` |

Infrastructure servers use static IP addresses and are outside the dynamic client allocation range.

---

## DHCP Scope

A dedicated DHCP scope was configured to provide dynamic IP addresses to client devices.

| Setting          | Value              |
| ---------------- | ------------------ |
| Scope Name       | `MANOTECH-CLIENTS` |
| Start IP Address | `192.168.1.100`    |
| End IP Address   | `192.168.1.200`    |
| Subnet Mask      | `255.255.255.0`    |
| Lease Duration   | Default            |

The scope provides up to 101 addresses for client systems.

The lower portion of the subnet is reserved for infrastructure servers using static addressing.

---

## DHCP Options

The following DHCP options are configured for client network initialization:

| Option | Description     | Value            |
| ------ | --------------- | ---------------- |
| 003    | Default Gateway | `192.168.1.1`    |
| 006    | DNS Server      | `192.168.1.2`    |
| 015    | DNS Domain Name | `manotech.local` |

These options ensure that domain clients receive the correct network configuration automatically.

---

## DHCP Authorization

The DHCP Server is authorized in Active Directory.

Authorization allows the DHCP service to provide IP addresses within the domain environment and prevents unauthorized DHCP servers from operating on the network.

The DHCP server is registered as:

```text id="ux2s8d"
DHCP01
192.168.1.3
```

---

## Client Configuration

Domain clients are configured to obtain their IP configuration automatically.

The Windows 11 client receives:

```text id="2m6ajh"
IP Address       → DHCP
Subnet Mask      → 255.255.255.0
Default Gateway  → 192.168.1.1
DNS Server       → 192.168.1.2
DNS Domain       → manotech.local
```

This allows the client to communicate with the Active Directory infrastructure without manually configuring network parameters.

---

## DHCP Address Allocation

The client address allocation follows the network design:

```text id="i1b0mt"
192.168.1.1       RRAS01
192.168.1.2       DC01
192.168.1.3       DHCP01
192.168.1.4       FILE01
192.168.1.5       WEB01

192.168.1.100     ─┐
                   │
                   │ DHCP Client Range
                   │
192.168.1.200     ─┘
```

This separation prevents infrastructure IP addresses from being dynamically assigned to clients.

---

## Validation

DHCP functionality was validated using the Windows 11 domain client.

### Client IP Configuration

The following command was used:

```text id="9f08lq"
ipconfig /all
```

The client should receive:

* An IP address within the DHCP scope.
* The correct subnet mask.
* `192.168.1.1` as the default gateway.
* `192.168.1.2` as the DNS server.
* `manotech.local` as the DNS suffix.

---

### DHCP Renewal

The following commands can be used to verify DHCP address assignment:

```text id="p3m2sq"
ipconfig /release
ipconfig /renew
```

After renewal, the client should receive an address within:

```text id="sh7u5q"
192.168.1.100 - 192.168.1.200
```

---

### Connectivity Validation

After receiving an address, connectivity can be verified using:

```text id="c4m8r2"
ping 192.168.1.1
ping 192.168.1.2
ping 192.168.1.4
ping 192.168.1.5
```

DNS resolution can then be tested using:

```text id="m98m5d"
nslookup manotech.local
nslookup www.manotech.local
```

---

## Troubleshooting

### Problem: Client Did Not Receive an IP Address

#### Possible Causes

* DHCP Server service stopped.
* DHCP Server not authorized.
* DHCP scope inactive.
* DHCP scope exhausted.
* Incorrect client network adapter configuration.
* Network connectivity problems.
* Client connected to the wrong VMware network.

#### Resolution

The following areas were checked:

1. DHCP Server service status.
2. DHCP authorization status.
3. DHCP scope activation.
4. Available addresses within the scope.
5. Client network adapter configuration.
6. VMware virtual network connectivity.

The client was then forced to request a new DHCP lease using:

```text id="j4p8bw"
ipconfig /release
ipconfig /renew
```

---

### Problem: Client Received an IP Address but Could Not Access the Domain

#### Possible Causes

* Incorrect DNS server.
* Incorrect default gateway.
* DNS resolution failure.
* Network connectivity issue.

#### Resolution

The client's DHCP configuration was verified using:

```text id="x5d8j1"
ipconfig /all
```

The expected DNS server is:

```text id="99pfqi"
192.168.1.2
```

The expected default gateway is:

```text id="w3m1x4"
192.168.1.1
```

DNS resolution was then tested using `nslookup`.

---

## DHCP Validation Checklist

| Test                                | Expected Result  |
| ----------------------------------- | ---------------- |
| DHCP Server service running         | Successful       |
| DHCP Server authorized              | Successful       |
| DHCP scope active                   | Successful       |
| Client receives IP address          | Successful       |
| Client receives correct subnet mask | Successful       |
| Client receives gateway             | `192.168.1.1`    |
| Client receives DNS                 | `192.168.1.2`    |
| Client receives DNS suffix          | `manotech.local` |
| Client communicates with DC01       | Successful       |
| Client resolves internal hostnames  | Successful       |

---

## Notes

DHCP is a fundamental network service that simplifies client management by providing automatic network configuration.

Within the ManoTech environment, DHCP works together with DNS and Active Directory to provide centralized network configuration for domain clients.

Infrastructure servers remain statically configured to ensure predictable addressing for critical services.

---

## Revision History

| Version | Date       | Author        | Description                                                                         |
| ------- | ---------- | ------------- | ----------------------------------------------------------------------------------- |
| 1.0     | 2026-07-26 | Mohamed Osama | Initial documentation                                                               |
| 1.1     | 2026-08-04 | Mohamed Osama | Reviewed and updated documentation                                                  |
| 1.2     | 2026-08-09 | Mohamed Osama | Updated server naming, DHCP scope, network configuration, and validation procedures |
