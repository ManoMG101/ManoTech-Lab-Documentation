# Virtualization Troubleshooting

## Overview

This document records the virtualization-related issues encountered while building and operating the ManoTech Enterprise Windows Server Lab using VMware Workstation Pro.

The troubleshooting focused on VMware networking, virtualization support, and virtual machine startup issues.

---

# 1. VMware Network Services Issue

## Problem

VMware virtual networking was not functioning correctly because required VMware network services were unavailable.

## Symptoms

The virtual machines experienced network connectivity problems and the expected VMware virtual networking behavior was not available.

The affected VMware services included:

```text
VMware DHCP Service
VMware NAT Service
```

An attempt to start the services returned an error indicating that the specified service was not installed.

## Error

```text
The specified service does not exist as an installed service.
```

## Investigation

The Windows Services console was checked to verify the VMware networking services.

The VMware installation and virtual network configuration were reviewed.

## Resolution

VMware Workstation was repaired to restore the required networking components and services.

After the repair, the VMware networking services became available again.

## Result

**Resolved**

---

# 2. VMware Virtual Network Configuration

## Problem

The VMware virtual network configuration did not initially match the network design of the enterprise lab.

## Symptoms

* Virtual machines were not consistently communicating.
* DHCP behavior was incorrect.
* Different virtual machines could end up on an unexpected network.
* Domain and server connectivity was affected.

## Resolution

The VMware virtual network was reviewed and configured to use the lab subnet:

```text id="7v5s4a"
192.168.1.0/24
```

The internal Windows DHCP Server was used for client IP assignment rather than relying on VMware DHCP for the enterprise network.

## Result

**Resolved**

The virtual machines were successfully placed on the intended lab network.

---

# 3. Intel VT-x / EPT Virtualization Issue

## Problem

A virtual machine failed to start because VMware could not use the required hardware virtualization features.

## Error

```text id="5df8k2"
Virtualized Intel VT-x/EPT is not supported on this platform
```

## Possible Causes

* Intel VT-x disabled in BIOS.
* Hardware virtualization unavailable to VMware.
* Hypervisor conflicts.
* VMware virtual machine processor configuration.

## Investigation

The host system virtualization configuration was reviewed.

BIOS virtualization settings were checked to verify that Intel virtualization technology was enabled.

VMware virtual machine settings were also reviewed.

## Resolution

Hardware virtualization support was verified and the VMware configuration was adjusted as required.

The virtual machine was then tested again.

## Result

**Resolved / Validated**

---

# 4. Virtual Machine Network Adapter Configuration

Each virtual machine was reviewed to ensure that the correct VMware network adapter was assigned.

The lab uses the virtual network to connect:

```text id="r8f2d4"
Windows Servers
Windows 11 Client
Kali Linux
RRAS
```

The network adapter configuration was verified when troubleshooting connectivity problems.

---

# 5. Virtualization Troubleshooting Methodology

The troubleshooting process followed this sequence:

```text id="x8q5nz"
Identify VM Problem
        ↓
Check VMware Configuration
        ↓
Check Windows Services
        ↓
Check VM Network Adapter
        ↓
Check Host Virtualization Support
        ↓
Check BIOS / Hypervisor Settings
        ↓
Apply Fix
        ↓
Restart / Test VM
        ↓
Validate Network and Services
```

This approach helped distinguish between VMware host issues, virtual machine configuration issues, and guest operating system problems.

---

# 6. Useful Checks

### Windows Services

VMware-related services can be checked through:

```text
services.msc
```

### VMware Network Configuration

The virtual network configuration can be reviewed through:

```text
VMware Workstation
→ Edit
→ Virtual Network Editor
```

### Windows Network Configuration

```powershell id="8z4y7f"
ipconfig /all
```

### Virtualization Status

Hardware virtualization support can be verified through the system BIOS/UEFI and Windows system configuration.

---

# 7. Lessons Learned

The virtualization troubleshooting process demonstrated several important concepts:

* VMware networking must be configured consistently across all virtual machines.
* VMware DHCP should not conflict with the Windows DHCP Server used by the lab.
* Hardware virtualization is required for running virtual machines efficiently.
* BIOS virtualization settings can directly affect VMware.
* VMware services are essential for virtual networking functionality.
* Troubleshooting should begin by separating host, hypervisor, VM, and guest OS issues.

---

## Resolution Summary

| Issue                                | Status               |
| ------------------------------------ | -------------------- |
| VMware DHCP/NAT Services             | Resolved             |
| VMware Virtual Network Configuration | Resolved             |
| Intel VT-x / EPT Issue               | Resolved / Validated |
| VM Network Adapter Configuration     | Validated            |
| Virtual Machine Connectivity         | Passed               |

---

## Revision History

| Version | Date       | Author        | Description                                          |
| ------- | ---------- | ------------- | ---------------------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial virtualization troubleshooting documentation |
