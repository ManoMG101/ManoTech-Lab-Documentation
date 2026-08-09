# ManoTech Enterprise Windows Server Lab

A complete enterprise Windows Server lab built using virtual machines to simulate a real-world corporate IT infrastructure.

This project demonstrates the deployment, configuration, administration, security hardening, and troubleshooting of a Windows-based enterprise environment using Active Directory, Windows Server roles, Group Policy, centralized file services, IIS, WSUS, and Active Directory Certificate Services.

---

## Project Overview

The lab was created to gain practical hands-on experience in enterprise system administration and infrastructure management.

The project covers the design, deployment, configuration, security, and documentation of a small enterprise network environment.

The implemented infrastructure includes:

* Active Directory Domain Services
* DNS
* DHCP
* File Server
* IIS Web Server
* RRAS / NAT
* Active Directory Certificate Services
* WSUS
* Group Policy
* Windows 11 Domain Client
* Kali Linux for Linux integration testing

---

## Infrastructure

The current environment includes:

* **Domain Controller**
* **DNS Server**
* **DHCP Server**
* **File Server**
* **IIS Web Server**
* **RRAS Router / NAT**
* **Active Directory Certificate Authority**
* **WSUS Server**
* **Windows 11 Domain Client**
* **Kali Linux Client**

---

## Technologies

* Windows Server 2019
* Windows 11
* Kali Linux
* Active Directory Domain Services (AD DS)
* DNS Server
* DHCP Server
* IIS
* Active Directory Certificate Services (AD CS)
* Windows Server Update Services (WSUS)
* Group Policy
* Windows Defender
* Windows Firewall
* PowerShell
* VMware Workstation
* draw.io

---

## Network

| Device | Role                            |  IP Address |
| ------ | ------------------------------- | ----------: |
| RRAS01 | Router / NAT                    | 192.168.1.1 |
| DC01   | Domain Controller / AD DS / DNS | 192.168.1.2 |
| DHCP01 | DHCP Server                     | 192.168.1.3 |
| FILE01 | File Server                     | 192.168.1.4 |
| WEB01  | IIS Web Server                  | 192.168.1.5 |

### Network

```text
192.168.1.0/24
```

### Domain

```text
manotech.local
```

---

## Active Directory

Active Directory provides centralized identity and access management across the environment.

The domain is:

```text
MANOTECH.LOCAL
```

The organizational structure separates users according to their departments and administrative requirements.

Example structure:

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

Active Directory is used for:

* Centralized authentication
* User management
* Security group management
* Organizational Units
* Access control
* Group Policy deployment
* Domain-joined computers

---

## DNS

DNS is integrated with Active Directory and provides name resolution for the internal domain.

Domain:

```text
manotech.local
```

DNS is hosted on:

```text
DC01
192.168.1.2
```

Internal DNS records are used for infrastructure services such as the IIS web server.

Example:

```text
www.manotech.local
```

---

## DHCP

A dedicated DHCP server provides automatic IP configuration to domain clients.

```text
DHCP01
192.168.1.3
```

DHCP provides:

* IP address assignment
* Subnet mask
* Default gateway
* DNS server information
* Domain information

---

## File Server

The File Server provides centralized departmental file storage.

Example folder structure:

```text
D:\Shares
│
├── HR
├── IT
├── Finance
├── Sales
├── Management
├── Public
└── Software
```

Access is controlled using:

* Active Directory Security Groups
* NTFS Permissions
* Share Permissions

Departmental users receive access according to their assigned groups and organizational roles.

---

## Group Policy

Group Policy is used to centrally manage security and user configuration throughout the domain.

Implemented policies include:

* Password Policy
* Account Lockout Policy
* Guest Account Restrictions
* Windows Firewall Configuration
* Windows Defender Configuration
* Screen Lock / Inactivity Settings
* Disable CMD
* Disable Registry Tools
* USB Storage Restrictions
* Drive Mapping
* Software Deployment
* Security-related user restrictions

Policies are assigned according to their required scope.

Administrative and IT users are not necessarily subject to the same restrictions as standard departmental users.

This allows the environment to demonstrate more realistic enterprise Group Policy design rather than applying every policy globally.

---

## Active Directory Certificate Services

Active Directory Certificate Services was deployed to provide an internal Public Key Infrastructure (PKI).

The environment includes an internal Root Certification Authority:

```text
ManoTech-ROOT-CA
```

AD CS is used to demonstrate:

* Internal certificate issuance
* Certificate templates
* Computer certificates
* Web server certificates
* Certificate-based infrastructure
* Integration with Active Directory

The PKI was also used as part of the IIS HTTPS configuration.

---

## WSUS

Windows Server Update Services was implemented to provide centralized Windows update management.

WSUS allows the administrator to:

* Synchronize Microsoft updates
* Manage update approvals
* Control updates for domain clients
* Centralize Windows update administration
* Reduce the need for unmanaged client-side update configuration

The WSUS environment is integrated with the Windows domain infrastructure.

---

## IIS Web Server

An internal IIS web server is deployed on:

```text
WEB01
192.168.1.5
```

The server hosts the internal ManoTech website.

Example DNS hostname:

```text
www.manotech.local
```

IIS was also used to demonstrate internal HTTPS configuration using certificates issued by the ManoTech internal Certificate Authority.

---

## RRAS

Routing and Remote Access Service is used to provide routing and NAT functionality for the lab network.

```text
RRAS01
192.168.1.1
```

RRAS provides connectivity between the internal virtual network and external network access.

---

## Security

Security hardening is implemented through Active Directory and Group Policy.

The environment includes:

* Password security
* Account lockout protection
* Windows Firewall configuration
* Windows Defender configuration
* Restricted administrative tools
* USB storage restrictions
* Automatic workstation locking
* Department-based file permissions
* Centralized security policies
* Controlled user access
* Internal PKI and certificate management
* Centralized Windows update management through WSUS

The goal is to demonstrate how centralized security controls can be implemented in an enterprise Windows environment.

---

## Linux Integration Testing

Kali Linux was introduced into the environment to test interoperability between Linux and the Windows Active Directory infrastructure.

The Linux system was successfully configured for network connectivity and DNS communication with the domain environment.

Linux domain integration using:

```text
realmd
SSSD
```

was also tested.

However, the full Linux domain join was **not successfully completed**.

The troubleshooting process and encountered issues are documented as part of the project.

---

## Troubleshooting

Troubleshooting is an important part of the project.

The documentation includes real configuration and troubleshooting scenarios encountered during deployment, including:

* Active Directory configuration issues
* DNS resolution problems
* Group Policy behavior
* File and NTFS permission issues
* Certificate request issues
* IIS HTTPS configuration
* WSUS configuration and connectivity
* Linux DNS configuration
* Linux domain integration
* VMware networking issues

These troubleshooting scenarios are documented to demonstrate practical problem-solving rather than only showing successful configurations.

---

## Documentation

Detailed project documentation is available in the **Documentation** folder.

Documentation includes:

* Project Overview
* Network Design
* Server Inventory
* Active Directory
* DNS
* DHCP
* File Server
* NTFS and Share Permissions
* IIS
* Active Directory Certificate Services
* WSUS
* Group Policy
* Security Configuration
* RRAS
* Linux Integration Testing
* Troubleshooting

---

## Diagrams

The project includes infrastructure diagrams created using **draw.io**.

Current diagrams include:

### 1. Network Topology

Shows:

* Router
* Switches
* Servers
* Clients
* Network connections
* IP addressing

### 2. Active Directory Structure

Shows:

* Domain
* Organizational Units
* Users
* Groups
* Domain Controllers

### 3. Server Roles / Infrastructure Diagram

Shows the servers and the roles/services provided by each server.

---

## Project Structure

```text
ManoTech-Enterprise-Lab
│
├── Documentation
│   ├── Project Overview
│   ├── Network Design
│   ├── Server Inventory
│   ├── Active Directory
│   ├── DNS
│   ├── DHCP
│   ├── File Server
│   ├── IIS
│   ├── AD CS
│   ├── WSUS
│   ├── Group Policy
│   ├── RRAS
│   ├── Linux Integration
│   └── Troubleshooting
│
├── Screenshots
│
├── Diagrams
│   ├── Network Topology
│   ├── Active Directory Structure
│   └── Server Infrastructure
│
├── Group Policies
│
└── README.md
```

---

## Project Status

### Implemented

* Active Directory Domain Services
* DNS
* DHCP
* File Server
* NTFS and Share Permissions
* IIS Web Server
* RRAS / NAT
* Active Directory Certificate Services
* Internal Root CA
* WSUS
* Group Policy
* Windows Defender Configuration
* Windows Firewall Configuration
* Windows 11 Domain Client
* Enterprise Documentation
* Network and Infrastructure Diagrams

### Tested / Partially Completed

* Kali Linux integration
* Linux DNS integration
* Linux domain integration using `realmd` / `SSSD`

The Linux domain integration was tested but was not successfully completed.

---

## Future Improvements

Potential future improvements include:

* PowerShell automation for infrastructure administration
* Advanced Active Directory security
* Additional domain clients
* Advanced certificate management
* Improved WSUS administration
* Backup and disaster recovery
* DFS and file replication
* VPN implementation
* Centralized logging
* Additional security auditing
* Further Linux/Windows interoperability

---

## Author

**Mohamed Osama**

Junior System Administrator

---

## License

This project is published for educational and portfolio purposes.
