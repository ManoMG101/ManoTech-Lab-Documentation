# Requirements

## 1. Overview

This document defines the hardware, virtualization, operating system, networking, and software requirements used to build the ManoTech Enterprise Windows Server Lab.

The environment is deployed as a virtualized enterprise infrastructure using VMware Workstation.

---

## 2. Host Hardware Requirements

The lab requires a host machine capable of running multiple virtual machines simultaneously.

### Recommended Host Specifications

| Component      | Requirement                                             |
| -------------- | ------------------------------------------------------- |
| CPU            | Modern multi-core processor with virtualization support |
| RAM            | 16 GB minimum, 32 GB recommended                        |
| Storage        | SSD with sufficient free space for multiple VMs         |
| Network        | Ethernet or Wi-Fi connectivity                          |
| Virtualization | Intel VT-x / AMD-V enabled                              |

Actual resource allocation depends on the number of virtual machines running simultaneously.

---

## 3. Virtualization Requirements

The lab is deployed using:

* VMware Workstation Pro
* VMware virtual machines
* VMware virtual networking
* NAT / Host-only networking as required by the lab design

Hardware virtualization must be enabled in the host system BIOS/UEFI.

---

## 4. Operating System Requirements

### Server Operating System

The primary server operating system used in the lab is:

* Windows Server 2019

Windows Server provides the core enterprise infrastructure roles and services.

### Client Operating System

* Windows 11

The Windows 11 client is joined to the `manotech.local` Active Directory domain.

### Linux Operating System

* Kali Linux

Kali Linux is used for Linux networking, DNS, interoperability, and Active Directory integration testing.

---

## 5. Virtual Machine Requirements

The lab consists of multiple virtual machines. Server roles are distributed according to the lab architecture rather than requiring a dedicated virtual machine for every individual service.

| VM                | Operating System    | Primary Role              |
| ----------------- | ------------------- | ------------------------- |
| DC01              | Windows Server 2019 | AD DS / DNS               |
| DHCP01            | Windows Server 2019 | DHCP                      |
| FILE01            | Windows Server 2019 | File Server               |
| WEB01             | Windows Server 2019 | IIS                       |
| RRAS01            | Windows Server 2019 | Routing / NAT             |
| Windows 11 Client | Windows 11          | Domain Client             |
| Kali Linux        | Kali Linux          | Linux Integration Testing |

Additional Windows Server roles such as **AD CS and WSUS are implemented on the existing server infrastructure rather than being represented as separate dedicated virtual machines**.

---

## 6. Network Requirements

The lab uses a private IPv4 network:

```text
192.168.1.0/24
```

The Active Directory domain is:

```text
manotech.local
```

The infrastructure requires:

* Internal DNS resolution
* DHCP services
* Static IP addresses for infrastructure servers
* Domain connectivity between servers and clients
* Routing and NAT through RRAS
* Communication between required infrastructure services

### Core Infrastructure Addresses

| Device |  IP Address | Role         |
| ------ | ----------: | ------------ |
| RRAS01 | 192.168.1.1 | Router / NAT |
| DC01   | 192.168.1.2 | AD DS / DNS  |
| DHCP01 | 192.168.1.3 | DHCP         |
| FILE01 | 192.168.1.4 | File Server  |
| WEB01  | 192.168.1.5 | IIS          |

---

## 7. Active Directory Requirements

The Active Directory environment requires:

* Windows Server 2019 Domain Controller
* Active Directory Domain Services
* Integrated DNS
* Configured Active Directory domain
* Organizational Units
* Security Groups
* Domain Users
* Domain-joined Windows clients

Domain:

```text
MANOTECH.LOCAL
```

---

## 8. Security Requirements

The environment requires centralized security controls through:

* Group Policy
* Password Policy
* Account Lockout Policy
* Windows Firewall
* Windows Defender
* User access restrictions
* NTFS and Share Permissions
* Certificate-based security

Security requirements are documented in greater detail in the **Security** section.

---

## 9. PKI Requirements

The internal PKI requires:

* Active Directory Certificate Services
* Internal Root Certification Authority
* Certificate Templates
* Active Directory integration
* Certificate enrollment
* Certificate management

The PKI is used to demonstrate enterprise certificate management and provide certificates for internal services.

---

## 10. WSUS Requirements

The centralized update management environment requires:

* Windows Server 2019
* WSUS Server Role
* IIS
* Required WSUS database components
* Network connectivity to Microsoft Update services
* Domain Group Policy configuration for client update management

WSUS is implemented as part of the existing Windows Server infrastructure rather than as a separate dedicated virtual machine.

WSUS provides centralized Windows Update administration, including update synchronization, approval, and client update management.

---

## 11. File Services Requirements

The File Server requires:

* Windows Server 2019
* Storage for shared folders
* SMB file sharing
* NTFS permissions
* Share permissions
* Active Directory Security Groups

Departmental access is controlled through Active Directory groups and permissions.

---

## 12. Web Server Requirements

The internal web environment requires:

* Windows Server 2019
* IIS
* Internal DNS records
* Network connectivity
* Internal certificates for HTTPS where required

Example internal hostname:

```text
www.manotech.local
```

---

## 13. Administration Tools

The following tools are used throughout the project:

* Server Manager
* Active Directory Users and Computers
* Group Policy Management
* DNS Manager
* DHCP Manager
* IIS Manager
* Certification Authority
* Certificate Templates Console
* WSUS Administration Console
* PowerShell
* Windows Administrative Tools
* VMware Workstation
* draw.io

---

## 14. Documentation Requirements

The project requires documentation for:

* Architecture
* Network design
* Server roles
* Configuration procedures
* Security policies
* Testing and validation
* Troubleshooting
* Lessons learned

Infrastructure diagrams are created using draw.io to visually document the environment.

---

## 15. Prerequisites

Before deployment, the following should be available:

1. VMware Workstation Pro installed.
2. Hardware virtualization enabled.
3. Windows Server 2019 installation media.
4. Windows 11 installation media.
5. Kali Linux installation media.
6. Sufficient RAM and storage for the virtual machines.
7. A configured VMware virtual network.
8. A defined IP addressing plan.
9. A defined Active Directory domain name.
10. Administrative access to the virtual machines.

---

## 16. Requirement Summary

The lab requires a virtualized environment capable of supporting:

```text
Virtualization
      │
      ├── Windows Server Infrastructure
      │     ├── Active Directory
      │     ├── DNS
      │     ├── DHCP
      │     ├── File Services
      │     ├── IIS
      │     ├── AD CS
      │     ├── WSUS
      │     └── RRAS
      │
      ├── Windows 11 Domain Client
      │
      └── Kali Linux
```

The services are distributed across the available virtual machines according to the project architecture, with multiple infrastructure roles deployed where appropriate rather than requiring one dedicated VM per service.

These requirements provide the foundation for the architecture and deployment phases documented in the following sections.
