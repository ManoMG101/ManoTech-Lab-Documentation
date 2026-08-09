# Architecture

## 1. Architecture Overview

The ManoTech Enterprise Windows Server Lab is designed as a small enterprise infrastructure running entirely on virtual machines using VMware Workstation.

The architecture distributes the main infrastructure services across multiple Windows Server virtual machines while maintaining centralized identity, security, and management through Active Directory.

The environment is built around the `manotech.local` Active Directory domain and uses the `192.168.1.0/24` internal network.

The implemented infrastructure includes:

* Active Directory Domain Services (AD DS)
* DNS
* DHCP
* File Services
* IIS
* Active Directory Certificate Services (AD CS)
* Windows Server Update Services (WSUS)
* Routing and Remote Access (RRAS)
* Windows 11 Domain Client
* Kali Linux for integration testing

---

## 2. Physical and Virtual Architecture

The entire environment is hosted on a single physical machine using VMware Workstation Pro.

VMware provides the virtualization layer and virtual networking required to connect the servers and clients.

```text
Physical Host
│
└── VMware Workstation Pro
    │
    ├── Windows Server VMs
    │
    ├── Windows 11 VM
    │
    └── Kali Linux VM
```

The virtual machines communicate through the internal VMware network using the `192.168.1.0/24` subnet.

---

## 3. Logical Architecture

```text
                         External Network
                                │
                                │
                            RRAS01
                         192.168.1.1
                         Routing / NAT
                                │
                                │
                    ┌───────────┴───────────┐
                    │    Internal Network   │
                    │     192.168.1.0/24    │
                    └───────────┬───────────┘
                                │
       ┌────────────────────────┼────────────────────────┐
       │                        │                        │
     DC01                    DHCP01                   FILE01
 192.168.1.2              192.168.1.3             192.168.1.4
 AD DS / DNS                  DHCP                File Server
 AD CS
 WSUS
       │
       │
       ├───────────────────────┐
       │                       │
     WEB01                 Domain Clients
 192.168.1.5              Windows 11 / Kali
    IIS
```

DC01 provides the core identity and management services for the environment.

---

## 4. Server Roles

### DC01 — Domain Controller

**IP Address:**

```text
192.168.1.2
```

**Roles:**

* Active Directory Domain Services
* DNS Server
* Active Directory Certificate Services
* WSUS

DC01 is the central infrastructure server of the environment.

It provides:

* Centralized authentication
* Active Directory management
* Internal DNS resolution
* Certificate services
* Centralized Windows Update management

The internal Certificate Authority is:

```text
ManoTech-ROOT-CA
```

WSUS is also installed on DC01 and is used to centrally manage Windows Updates for domain clients.

---

### DHCP01 — DHCP Server

**IP Address:**

```text
192.168.1.3
```

**Role:**

* DHCP Server

DHCP01 provides automatic network configuration to clients.

It distributes:

* IP addresses
* Subnet mask
* Default gateway
* DNS server information
* Domain information

---

### FILE01 — File Server

**IP Address:**

```text
192.168.1.4
```

**Role:**

* File Services

FILE01 provides centralized departmental file storage.

Access is controlled through:

* Active Directory Security Groups
* NTFS Permissions
* Share Permissions

---

### WEB01 — IIS Web Server

**IP Address:**

```text
192.168.1.5
```

**Role:**

* IIS Web Server

WEB01 hosts the internal ManoTech website.

Internal DNS provides access through hostnames such as:

```text
www.manotech.local
```

Certificates issued by the internal PKI can be used to secure internal web services.

---

### RRAS01 — Router

**IP Address:**

```text
192.168.1.1
```

**Roles:**

* Routing
* NAT

RRAS01 provides connectivity between the internal virtual network and the external network.

---

## 5. Client Architecture

### Windows 11 Client

The Windows 11 client is joined to the:

```text
MANOTECH.LOCAL
```

domain.

The client receives centralized services and configuration from the Windows infrastructure, including:

* DNS
* DHCP
* Active Directory
* Group Policy
* WSUS

The Windows 11 client is used to validate domain authentication, security policies, resource access, and Windows Update management.

---

### Kali Linux

Kali Linux is included as a Linux client for interoperability and Active Directory integration testing.

The system was successfully configured for:

* Network connectivity
* DNS communication
* Communication with the Windows infrastructure

Active Directory integration using:

```text
realmd
SSSD
```

was tested but was not successfully completed.

The troubleshooting process is documented separately.

---

## 6. Active Directory Architecture

The Active Directory environment is organized using Organizational Units and Security Groups.

```text
MANOTECH.LOCAL
│
├── Domain Controllers
│   └── DC01
│
├── Standard Users
│   ├── IT
│   ├── HR
│   ├── Finance
│   ├── Sales
│   └── Management
│
└── Computers
```

Active Directory provides the foundation for:

* Authentication
* Authorization
* User management
* Security Groups
* Group Policy
* Resource access
* Domain management

---

## 7. Network Architecture

The internal network uses:

```text
192.168.1.0/24
```

Infrastructure servers use static IP addresses.

| Device |  IP Address | Main Function              |
| ------ | ----------: | -------------------------- |
| RRAS01 | 192.168.1.1 | Routing / NAT              |
| DC01   | 192.168.1.2 | AD DS / DNS / AD CS / WSUS |
| DHCP01 | 192.168.1.3 | DHCP                       |
| FILE01 | 192.168.1.4 | File Services              |
| WEB01  | 192.168.1.5 | IIS                        |

Client machines receive their network configuration through DHCP.

DNS services are provided by DC01.

---

## 8. Service Dependencies

The infrastructure services are interconnected and depend on several core components.

### Active Directory

```text
Active Directory
│
├── DNS
├── Group Policy
├── Security Groups
├── File Server Permissions
├── WSUS Client Management
└── AD CS
```

Active Directory and DNS form the foundation of the Windows environment.

---

### DNS

DNS is required for:

* Active Directory
* Domain authentication
* Domain Clients
* IIS
* WSUS
* AD CS
* Linux integration

Correct DNS configuration is therefore critical to the environment.

---

### Group Policy

Group Policy provides centralized configuration for domain clients.

Policies include:

* Password Policy
* Account Lockout
* Windows Defender
* Windows Firewall
* Screen Lock
* USB restrictions
* CMD restrictions
* Registry restrictions
* Drive Mapping
* Software Deployment
* WSUS configuration

---

### AD CS

AD CS depends on:

* Active Directory
* DNS
* Domain connectivity

It provides the internal PKI and certificate infrastructure.

---

### WSUS

WSUS depends on:

* Windows Server
* IIS
* Network connectivity
* Domain infrastructure
* Group Policy

Domain clients receive their WSUS configuration through Group Policy.

---

## 9. Security Architecture

Security is implemented through multiple layers.

```text
                    Security
                       │
        ┌──────────────┼──────────────┐
        │              │              │
   Active Directory  Group Policy   AD CS / PKI
        │              │              │
        │              │              │
 Authentication     Hardening      Certificates
        │              │              │
        └──────────────┼──────────────┘
                       │
                Resource Access
                       │
                NTFS / SMB
```

The environment combines:

* Centralized authentication
* Group Policy
* Host security
* File permissions
* Certificate-based services
* Controlled access

---

## 10. Design Decisions

### Centralized Identity

Active Directory was selected as the central identity platform to provide:

* Centralized authentication
* User management
* Security Groups
* Group Policy
* Resource access control

---

### Separation of Major Server Roles

Major infrastructure services are distributed across separate virtual machines where appropriate.

This provides:

* Role isolation
* Easier troubleshooting
* More realistic enterprise architecture
* Independent service management

However, services such as **AD CS and WSUS are hosted on DC01** rather than requiring dedicated virtual machines.

---

### Static Server IP Addresses

Infrastructure servers use static IP addresses because services such as:

* Active Directory
* DNS
* DHCP
* IIS
* File Services
* RRAS

require predictable network addressing.

---

### Centralized Security

Group Policy was selected as the primary mechanism for applying consistent security configurations across domain clients.

This reduces the need to configure each workstation independently.

---

### Internal PKI

AD CS was implemented to demonstrate enterprise certificate management and provide an internal trust infrastructure for services such as HTTPS.

---

### Centralized Update Management

WSUS was implemented to provide centralized Windows Update management and administrative control over client updates.

WSUS is hosted on DC01 as part of the existing infrastructure.

---

## 11. Architecture Summary

The final architecture can be summarized as:

```text
VMware Workstation
│
├── RRAS01
│   └── Routing / NAT
│
├── DC01
│   ├── Active Directory
│   ├── DNS
│   ├── AD CS
│   └── WSUS
│
├── DHCP01
│   └── DHCP
│
├── FILE01
│   └── File Services
│
├── WEB01
│   └── IIS
│
├── Windows 11
│   └── Domain Client
│
└── Kali Linux
    └── Integration Testing
```

This architecture provides a realistic enterprise Windows environment for practicing infrastructure deployment, centralized management, security configuration, certificate services, update management, troubleshooting, and system administration.
