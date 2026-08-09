# Server Inventory

## Overview

This document provides an inventory of all servers, clients, and major systems deployed in the ManoTech Enterprise Windows Server Lab.

The inventory identifies each system, its operating system, IP address, primary role, installed services, and current status.

---

## Server Inventory

| Hostname     | Operating System    | IP Address  | Primary Role      | Installed Services                                | Status                          |
| ------------ | ------------------- | ----------- | ----------------- | ------------------------------------------------- | ------------------------------- |
| DC01         | Windows Server 2019 | 192.168.1.2 | Domain Controller | AD DS, DNS, AD CS, WSUS                           | Active                          |
| DHCP01       | Windows Server 2019 | 192.168.1.3 | DHCP Server       | DHCP Server                                       | Active                          |
| FILE01       | Windows Server 2019 | 192.168.1.4 | File Server       | File Services, SMB, Shared Folders                | Active                          |
| WEB01        | Windows Server 2019 | 192.168.1.5 | Web Server        | IIS                                               | Active                          |
| RRAS01       | Windows Server 2019 | 192.168.1.1 | Router            | RRAS, Routing, NAT                                | Active                          |
| WIN11-CLIENT | Windows 11 Pro      | DHCP        | Domain Client     | Group Policy, WSUS Client, Certificate Enrollment | Active                          |
| KALI-LINUX   | Kali Linux          | DHCP        | Linux Client      | Network and AD Integration Testing                | Tested / Integration Incomplete |

---

## Domain Information

| Setting         | Value            |
| --------------- | ---------------- |
| Domain Name     | `manotech.local` |
| DNS Server      | `192.168.1.2`    |
| Network Address | `192.168.1.0/24` |
| Default Gateway | `192.168.1.1`    |

---

## Server Responsibilities

### DC01

**IP Address:** `192.168.1.2`

DC01 is the central infrastructure server for the Windows domain environment.

Responsibilities include:

* Active Directory Domain Services (AD DS)
* DNS Server
* Group Policy Management
* Active Directory Certificate Services (AD CS)
* Enterprise Root Certification Authority
* Windows Server Update Services (WSUS)
* User and Computer Management
* Certificate Authority Management
* Centralized authentication and authorization

**Certificate Authority:**

```text
ManoTech-ROOT-CA
```

**WSUS** is installed directly on DC01 and provides centralized Windows Update management for domain clients.

---

### DHCP01

**IP Address:** `192.168.1.3`

Responsibilities include:

* Dynamic IP Address Assignment
* DHCP Scope Management
* Network Configuration Distribution
* DNS and Gateway Configuration for Clients

Infrastructure servers use static IP addresses and are not dependent on DHCP for their primary network configuration.

---

### FILE01

**IP Address:** `192.168.1.4`

Responsibilities include:

* Department Shared Folders
* SMB File Sharing
* NTFS Permissions
* Share Permissions
* Access Control through Active Directory Security Groups

The File Server provides centralized storage for departmental resources.

---

### WEB01

**IP Address:** `192.168.1.5`

Responsibilities include:

* Internet Information Services (IIS)
* Internal Website Hosting
* Internal Web Service Configuration
* HTTPS support using certificates issued by the internal PKI where applicable

Example internal hostname:

```text
www.manotech.local
```

---

### RRAS01

**IP Address:** `192.168.1.1`

Responsibilities include:

* Default Gateway
* Routing
* Network Address Translation (NAT)
* Connectivity between the internal network and external network

RRAS01 provides the routing infrastructure for the lab network.

---

## Client Systems

### WIN11-CLIENT

**Operating System:** Windows 11 Pro

**IP Configuration:** DHCP

Responsibilities include:

* Domain-Joined Workstation
* Active Directory Authentication
* Group Policy Testing
* Security Policy Validation
* File Access Testing
* Certificate Enrollment
* WSUS Client Testing
* Windows Update Testing

The client is joined to:

```text
MANOTECH.LOCAL
```

---

### KALI-LINUX

**Operating System:** Kali Linux

**IP Configuration:** DHCP

Kali Linux is included in the environment for:

* Network Connectivity Testing
* DNS Testing
* Linux/Windows Interoperability Testing
* Active Directory Integration Testing

Linux Active Directory integration using `realmd` and `SSSD` was tested but was not successfully completed.

The integration issues and troubleshooting process are documented separately.

---

## Infrastructure Summary

The final server architecture is:

```text
DC01
├── Active Directory Domain Services
├── DNS
├── AD CS
└── WSUS

DHCP01
└── DHCP

FILE01
└── File Services

WEB01
└── IIS

RRAS01
└── Routing / NAT
```

---

## Network Addressing Summary

| Hostname     |  IP Address | Address Type |
| ------------ | ----------: | ------------ |
| RRAS01       | 192.168.1.1 | Static       |
| DC01         | 192.168.1.2 | Static       |
| DHCP01       | 192.168.1.3 | Static       |
| FILE01       | 192.168.1.4 | Static       |
| WEB01        | 192.168.1.5 | Static       |
| WIN11-CLIENT |        DHCP | Dynamic      |
| KALI-LINUX   |        DHCP | Dynamic      |

---

## Current Status

| System       | Status                 |
| ------------ | ---------------------- |
| DC01         | Active                 |
| DHCP01       | Active                 |
| FILE01       | Active                 |
| WEB01        | Active                 |
| RRAS01       | Active                 |
| WIN11-CLIENT | Active                 |
| KALI-LINUX   | Integration Incomplete |

---

## Notes

The server inventory reflects the current state of the ManoTech Enterprise Windows Server Lab.

AD CS and WSUS are implemented on **DC01** and are not deployed as separate virtual machines.

The Zabbix monitoring server was removed from the final project architecture and is therefore not included in this inventory.

Future changes to the infrastructure will be reflected in subsequent revisions of this document.

---

## Revision History

| Version | Date       | Author        | Description                                                               |
| ------- | ---------- | ------------- | ------------------------------------------------------------------------- |
| 1.0     | 2026-07-26 | Mohamed Osama | Initial documentation                                                     |
| 1.1     | 2026-08-04 | Mohamed Osama | Reviewed and updated documentation                                        |
| 1.2     | 2026-08-09 | Mohamed Osama | Updated server roles, infrastructure services, and current project status |
