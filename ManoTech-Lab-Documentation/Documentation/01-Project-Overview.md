# Project Overview

## Project Name

**ManoTech Enterprise Windows Server Lab**

---

## Introduction

This project is a simulated enterprise Windows infrastructure built entirely using virtual machines in VMware Workstation.

The objective is to design, deploy, secure, manage, and troubleshoot a realistic Windows Server environment while following enterprise administration practices.

The lab was built from scratch and includes multiple interconnected infrastructure services, centralized identity management, security policies, certificate services, update management, and internal network services.

The project also focuses on documenting the implementation process, configuration decisions, troubleshooting scenarios, and validation results.

---

## Project Objectives

The main objectives of this project are to:

* Build a complete Active Directory environment from scratch.
* Configure and manage core Windows Server roles and services.
* Deploy an internal Public Key Infrastructure (PKI).
* Manage Windows Updates using WSUS.
* Secure enterprise resources using certificates and Group Policy.
* Implement centralized authentication and access control.
* Practice enterprise administration using Microsoft technologies.
* Gain hands-on experience with PowerShell and Windows Server management.
* Develop troubleshooting and problem-solving skills.
* Document the infrastructure using professional technical documentation.
* Build a portfolio-ready enterprise homelab.

---

## Technologies Used

* Windows Server 2019
* Windows 11 Client
* Kali Linux
* Active Directory Domain Services (AD DS)
* DNS
* DHCP
* Group Policy
* File Services
* IIS Web Server
* Active Directory Certificate Services (AD CS)
* Windows Server Update Services (WSUS)
* Routing and Remote Access (RRAS)
* PowerShell
* VMware Workstation Pro
* draw.io

---

## Lab Environment

The lab consists of multiple virtual machines connected through a VMware virtual network simulating a small enterprise infrastructure.

The implemented environment includes:

* Domain Controller
* Active Directory Domain Services
* DNS Server
* DHCP Server
* File Server
* IIS Web Server
* Active Directory Certificate Authority
* WSUS Server
* RRAS Router / NAT
* Windows 11 Domain Client
* Kali Linux Client for Linux integration and security testing

The infrastructure is based on the following internal domain:

```text
manotech.local
```

and the primary lab network:

```text
192.168.1.0/24
```

---

## Project Goals

By completing this lab, the following skills are demonstrated:

* Active Directory Administration
* DNS and DHCP Configuration
* Group Policy Management
* File Server Administration
* NTFS and Share Permissions
* IIS Deployment and Configuration
* Active Directory Certificate Services (PKI)
* Certificate Template Management
* Certificate Enrollment
* Windows Server Update Services (WSUS)
* Windows Update Management
* RRAS and NAT Configuration
* Enterprise Security Configuration
* Windows Server Troubleshooting
* PowerShell Administration
* Enterprise Infrastructure Documentation

---

## Documentation

This repository contains professional documentation covering the design, implementation, security, testing, and troubleshooting of the infrastructure.

Documentation includes:

* Project Overview
* Requirements
* Architecture
* Network Design
* Server Inventory
* Active Directory
* DNS
* DHCP
* File Server
* IIS
* Active Directory Certificate Services
* WSUS
* Group Policy
* Security Baseline
* RRAS
* Testing and Validation
* Troubleshooting
* Linux Integration
* Lessons Learned

Each implementation phase documents the relevant objectives, configuration steps, important commands, validation procedures, and troubleshooting scenarios.

---

## Project Status

**Project Status: Infrastructure Completed — Documentation and Refinement Ongoing**

### Completed

* Active Directory Domain Services (AD DS)
* DNS
* DHCP
* File Server
* NTFS and Share Permissions
* IIS Web Server
* Group Policy
* Security Configuration
* Active Directory Certificate Services (AD CS)
* Internal Root Certificate Authority
* Certificate Templates
* Certificate Enrollment
* Windows Server Update Services (WSUS)
* RRAS / NAT
* Windows 11 Domain Client
* Network and Infrastructure Diagrams

### Tested / Partially Completed

* Kali Linux Network Integration
* Linux DNS Integration
* Linux Active Directory Integration using `realmd` / `SSSD`

The Linux domain integration was tested but was not successfully completed. The encountered issues and troubleshooting process are documented separately.

### Ongoing

* Project Documentation
* Testing and Validation Documentation
* Troubleshooting Documentation
* Final Project Refinement

---

## Future Improvements

Potential future improvements include:

* Windows Deployment Services (WDS)
* Microsoft Deployment Toolkit (MDT)
* Backup and Disaster Recovery
* Advanced Group Policy Configuration
* PowerShell Automation
* Centralized Logging
* Additional Enterprise Security Features
* Additional Domain Clients
* Advanced Linux/Windows Integration

---

## Revision History

| Version | Date       | Author        | Description                                                                               |
| ------- | ---------- | ------------- | ----------------------------------------------------------------------------------------- |
| 1.0     | 2026-07-26 | Mohamed Osama | Initial documentation                                                                     |
| 1.1     | 2026-08-04 | Mohamed Osama | Reviewed and updated documentation                                                        |
| 1.2     | 2026-08-09 | Mohamed Osama | Updated project status, infrastructure, documentation structure, and implemented services |
