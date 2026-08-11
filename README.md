# ManoTech Enterprise Windows Server Lab

A complete enterprise Windows Server lab built using virtual machines to simulate a real-world corporate IT infrastructure.

This project demonstrates the deployment, configuration, administration, security hardening, Group Policy management, troubleshooting, validation, and documentation of a Windows-based enterprise environment using Active Directory, Windows Server roles, centralized file services, IIS, WSUS, Active Directory Certificate Services, and RRAS.

---

## Project Overview

The lab was created to gain practical hands-on experience in enterprise system administration and infrastructure management.

The project covers the design, deployment, configuration, security, troubleshooting, validation, and documentation of a small enterprise network environment.

The implemented infrastructure includes:

- Active Directory Domain Services
- DNS
- DHCP
- File Server
- IIS Web Server
- RRAS / NAT
- Active Directory Certificate Services
- WSUS
- Group Policy
- Windows Defender
- Windows Firewall
- Windows 11 Domain Client
- Kali Linux for Linux integration testing

---

## Infrastructure

The current environment includes:

- **Domain Controller** — `SRV01-DC`
- **DHCP Server** — `SRV02-DHCP`
- **File Server** — `SRV03-FILESRV`
- **IIS Web Server** — `SRV04-WEB`
- **RRAS / NAT** — `RRAS01`
- **Active Directory Certificate Authority** — `ManoTech-ROOT-CA`
- **WSUS**
- **Windows 11 Domain Client**
- **Kali Linux Client**

---

## Technologies

- Windows Server 2019
- Windows 11
- Kali Linux
- Active Directory Domain Services (AD DS)
- DNS Server
- DHCP Server
- IIS
- Active Directory Certificate Services (AD CS)
- Windows Server Update Services (WSUS)
- Group Policy
- Windows Defender
- Windows Firewall
- RRAS / NAT
- PowerShell
- VMware Workstation
- draw.io

---

## Network

| Device | Role | IP Address |
| ------ | ---- | ----------: |
| RRAS01 | Router / NAT | `192.168.1.1` |
| SRV01-DC | Domain Controller / AD DS / DNS | `192.168.1.2` |
| SRV02-DHCP | DHCP Server | `192.168.1.3` |
| SRV03-FILESRV | File Server | `192.168.1.4` |
| SRV04-WEB | IIS Web Server | `192.168.1.5` |

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

The Organizational Unit structure separates users according to their roles and administrative requirements.

### Structure

```text
MANOTECH.LOCAL
│
├── Domain Controllers
│   └── SRV01-DC
│
├── Standard Users
│   ├── IT
│   ├── HR
│   ├── Finance
│   └── Sales
│
└── Management
```

Active Directory is used for:

- Centralized authentication
- User management
- Security group management
- Organizational Units
- Access control
- Group Policy deployment
- Domain-joined computers
- Centralized administration

---

## DNS

DNS is integrated with Active Directory and provides name resolution for the internal domain.

### Domain

```text
manotech.local
```

### DNS Server

```text
SRV01-DC
192.168.1.2
```

Internal DNS records are used for infrastructure services and internal applications.

Example:

```text
www.manotech.local
```

---

## DHCP

A dedicated DHCP server provides automatic IP configuration to domain clients.

```text
SRV02-DHCP
192.168.1.3
```

### DHCP Scope

```text
MANOTECH-CLIENTS
192.168.1.100 - 192.168.1.200
```

DHCP provides:

- IP address assignment
- Subnet mask
- Default gateway
- DNS server information
- Domain information

The DHCP infrastructure was validated using the Windows 11 domain client.

---

## File Server

The File Server provides centralized departmental file storage.

### Server

```text
SRV03-FILESRV
192.168.1.4
```

### Folder Structure

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

- Active Directory Security Groups
- NTFS Permissions
- Share Permissions
- Permission inheritance

Departmental users receive access according to their assigned security groups and organizational roles.

---

## Group Policy

Group Policy is used to centrally manage security and configuration throughout the domain.

The final Group Policy design separates domain-wide policies from user-specific and computer-specific policies.

### Default Domain Policy

The Default Domain Policy contains domain-wide account and authentication policies:

- Password Policy
- Account Lockout Policy
- Kerberos Policy

### Default Domain Controllers Policy

The Domain Controllers policy contains security settings specifically intended for domain controllers, including:

- User Rights Assignment
- Security Options
- Account Security
- Interactive Logon Security
- Audit Policies

### User Security Baseline

Common user security settings are managed through the User Security Baseline.

This includes:

- Screen Lock / Inactivity Settings
- Common security configuration applied to users

The screen lock configuration is set to **5 minutes**.

### Standard User Restrictions

Standard departmental users are subject to additional restrictions, including:

- Command Prompt disabled
- PowerShell disabled
- Windows Terminal disabled
- Registry Editor disabled
- Run Command disabled
- Control Panel access restricted
- USB storage access denied
- Computer Management restricted
- Protected application modification restricted
- Managed shortcuts protected from modification or deletion

### Management

Management users are not assigned the Standard User restriction policy.

This prevents unnecessary restrictions from being applied to users who require a less restrictive working environment.

### Security Policies

Common computer security controls include:

- Microsoft Defender configuration
- Windows Firewall configuration

Policies are assigned according to their required scope rather than applying every restriction globally.

This allows the environment to demonstrate a more realistic enterprise Group Policy design.

---

## Active Directory Certificate Services

Active Directory Certificate Services was deployed to provide an internal Public Key Infrastructure (PKI).

The environment includes an internal Root Certification Authority:

```text
ManoTech-ROOT-CA
```

AD CS is used to demonstrate:

- Internal certificate issuance
- Certificate templates
- Computer certificates
- Web server certificates
- Certificate-based infrastructure
- Integration with Active Directory
- IIS HTTPS configuration

The PKI was also used as part of the IIS HTTPS configuration.

---

## WSUS

Windows Server Update Services was implemented to provide centralized Windows update management.

WSUS allows the administrator to:

- Synchronize Microsoft updates
- Manage update approvals
- Control updates for domain clients
- Centralize Windows update administration
- Reduce unmanaged client-side update configuration

The WSUS environment is integrated with the Windows domain infrastructure.

WSUS configuration and connectivity troubleshooting are documented as part of the project.

---

## IIS Web Server

An internal IIS web server is deployed on:

```text
SRV04-WEB
192.168.1.5
```

The server hosts the internal ManoTech website.

### DNS Hostname

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

RRAS provides the default gateway and network connectivity for the internal virtual network.

---

## Security

Security hardening is implemented through Active Directory, Group Policy, and Windows Server security controls.

The environment includes:

- Password security
- Account lockout protection
- Kerberos configuration
- Windows Firewall configuration
- Windows Defender configuration
- Restricted administrative tools
- USB storage restrictions
- Automatic workstation locking
- Department-based file permissions
- Centralized security policies
- Controlled user access
- Internal PKI and certificate management
- Centralized Windows update management through WSUS

The goal is to demonstrate how centralized security controls can be implemented in an enterprise Windows environment.

---

## Linux Integration Testing

Kali Linux was introduced into the environment to test interoperability between Linux and the Windows Active Directory infrastructure.

The Linux system was successfully configured for:

- Network connectivity
- DNS communication with the domain environment

Linux domain integration using:

```text
realmd
SSSD
```

was also tested.

However, the full Linux domain join was **not successfully completed**.

The encountered DNS, package, and domain integration issues are documented as part of the project's troubleshooting documentation.

---

## Troubleshooting

Troubleshooting is an important part of the project.

The documentation includes real configuration and troubleshooting scenarios encountered during deployment, including:

- Active Directory configuration issues
- DNS resolution problems
- Group Policy behavior
- Group Policy scope and inheritance
- File and NTFS permission issues
- Certificate request issues
- IIS HTTPS configuration
- WSUS configuration and connectivity
- Linux DNS configuration
- Linux domain integration
- VMware networking issues

These troubleshooting scenarios demonstrate practical problem-solving rather than only documenting successful configurations.

---

## Validation

Validation documentation was created to verify that the implemented infrastructure operates as expected.

Validation includes:

- Active Directory validation
- DHCP validation
- Domain client authentication
- DNS integration
- Group Policy processing
- File Server connectivity
- Security configuration
- Infrastructure service verification

The validation documents record the expected results and final status of each tested component.

---

## Documentation

Detailed project documentation is available in the **Documentation** folder.

Documentation includes:

- Project Overview
- Requirements
- Architecture
- Network Design
- Server Inventory
- Active Directory
- DNS
- DHCP
- File Server
- NTFS and Share Permissions
- IIS
- Active Directory Certificate Services
- WSUS
- Group Policy
- Security Configuration
- RRAS
- Linux Integration Testing
- Validation
- Troubleshooting

---

## Diagrams

The project includes infrastructure diagrams created using **draw.io**.

Current diagrams include:

### 1. Physical Network Topology

Shows:

- Router / RRAS
- Switches
- Servers
- Clients
- Network connections
- IP addressing

### 2. Active Directory Structure

Shows:

- Domain
- Organizational Units
- User groups
- Domain Controllers
- Administrative structure

### 3. Group Policy Structure

Shows:

- Default Domain Policy
- Default Domain Controllers Policy
- User Security Baseline
- Standard User Restrictions
- Computer security policies
- Policy scope and organization

---

## Project Structure

```text
MANOTECH-LAB-DOCUMENTATION
│
├── Diagrams
│   ├── AD-Structure.png
│   ├── GP-Structure.png
│   └── Physical-Topology.png
│
└── Documentation
│    │
│    ├── Infrastructure
│    │   └── 06-Active Directory
│    │   ├── 07-DNS
│    │   ├── 08-DHCP
│    │   ├── 09-File Server
│    │   ├── 10-IIS
│    │   ├── 11-AD CS
│    │   ├── 12-WSUS
│    │   └── 13-RRAS
│    │
│    ├── 01-Project-Overview.md
│    ├── 02-Requirements.md
│    ├── 03-Architecture.md
│    ├── 04-Network-Design.md
│    ├── 05-Server-Inventory.md
│    │
│    └── Testing&Troubleshooting
│        │
│        ├── 14-Testing-and-Validation
│        │   ├── 01-Network-Connectivity.md
│        │   ├── 02-DNS-Validation.md
│        │   ├── 03-DHCP-Validation.md
│        │   ├── 04-Active-Directory-Validation.md
│        │   ├── 05-File-Server-Validation.md
│        │   ├── 06-IIS-Validation.md
│        │   ├── 07-AD-CS-Validation.md
│        │   ├── 08-WSUS-Validation.md
│        │   ├── 09-RRAS-Validation.md
│        │   └── 10-Integration-Testing.md
│        │
│        └── 15-Troubleshooting
│            ├── 01-VMware-Networking.md
│            ├── 02-Active-Directory.md
│            ├── 03-DNS.md
│            ├── 04-DHCP.md
│            ├── 05-Group-Policy.md
│            ├── 06-File-Server.md
│            ├── 07-AD-CS.md
│            └── 08-WSUS.md
│
│   
└──────── 16-Lessons-Learned


---

## Project Status

### Implemented

- Active Directory Domain Services
- DNS
- DHCP
- File Server
- NTFS and Share Permissions
- IIS Web Server
- RRAS / NAT
- Active Directory Certificate Services
- Internal Root CA
- WSUS
- Group Policy
- Windows Defender Configuration
- Windows Firewall Configuration
- Windows 11 Domain Client
- Enterprise Documentation
- Infrastructure Diagrams
- Validation Documentation
- Troubleshooting Documentation

### Tested / Partially Completed

- Kali Linux integration
- Linux DNS integration
- Linux domain integration using `realmd` / `SSSD`

The Linux domain integration was tested but was not successfully completed.

---

## Future Improvements

Potential future improvements include:

- PowerShell automation for infrastructure administration
- Advanced Active Directory security
- Additional domain clients
- Advanced certificate management
- Improved WSUS administration
- Backup and disaster recovery
- DFS and file replication
- VPN implementation
- Centralized logging
- Additional security auditing
- Further Linux / Windows interoperability

---

## Author

**Mohamed Osama**

Junior System Administrator

---

## License

This project is published for educational and portfolio purposes.