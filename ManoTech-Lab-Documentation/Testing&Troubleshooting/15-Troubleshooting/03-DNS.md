# DNS Troubleshooting

## Overview

This document records the DNS-related troubleshooting scenarios encountered during the deployment of the ManoTech Enterprise Windows Server Lab.

DNS is a critical dependency for Active Directory, domain authentication, internal service discovery, and communication between enterprise systems.

---

# 1. Active Directory Client DNS Configuration

## Problem

The Windows 11 client experienced problems locating and communicating with the Active Directory Domain Controller.

## Symptoms

* Domain join problems.
* Domain services were not consistently reachable.
* Internal hostname resolution failed in some configurations.

## Root Cause

The client was not consistently using the internal Domain Controller as its DNS server.

Active Directory requires internal DNS resolution to locate domain services.

## Resolution

The client was configured to use:

```text
DNS Server: 192.168.1.2
```

The configuration was verified using:

```powershell
ipconfig /all
```

DNS resolution was tested using:

```powershell
nslookup manotech.local
```

and:

```powershell
nslookup SRV01-DC
```

## Result

**Resolved**

The Windows 11 client successfully resolved internal domain resources.

---

# 2. Linux DNS Resolution Issue

## Problem

The Monitor Server running Ubuntu could communicate with the Active Directory DNS server directly, but normal hostname resolution was failing.

> Note: The Monitor Server was later removed from the final project scope. This incident is retained as a troubleshooting record because it occurred during the lab implementation.

## Symptoms

Direct DNS queries to the Domain Controller were successful:

```bash
nslookup manotech.local 192.168.1.2
```

However, normal hostname resolution failed when using commands such as:

```bash
ping manotech.local
```

## Investigation

The DNS server itself was reachable and responding correctly.

The problem was identified in the Linux name-service configuration.

The `/etc/nsswitch.conf` file contained:

```text
hosts: files mdns4_minimal [NOTFOUND=return] dns
```

The `mdns4_minimal` lookup mechanism could interfere with normal DNS resolution before the DNS resolver was reached.

## Resolution

The `hosts` configuration was changed to:

```text
hosts: files dns
```

After modifying the configuration, hostname resolution was tested again.

## Validation

```bash
ping manotech.local
```

The domain hostname resolved successfully through:

```text
192.168.1.2
```

## Result

**Resolved**

---

# 3. DNS and IIS Integration

## Problem

The internal IIS website needed to be accessible using a hostname instead of its IP address.

## Configuration

An A record was configured for:

```text
www.manotech.local
```

The record points to:

```text
192.168.1.5
```

## Validation

```powershell
nslookup www.manotech.local
```

Expected result:

```text
www.manotech.local → 192.168.1.5
```

The website was then accessed using:

```text
http://www.manotech.local
```

## Result

**Resolved**

The IIS website was successfully integrated with the internal DNS infrastructure.

---

# 4. DNS and Active Directory Service Discovery

Active Directory automatically creates DNS records required for domain services.

These records allow clients to locate services such as:

* Domain Controllers.
* Kerberos.
* LDAP.
* Global Catalog services.

The DNS zone was checked to verify the presence of Active Directory service records.

### Validation

```powershell
nslookup -type=SRV _ldap._tcp.dc._msdcs.manotech.local
```

## Result

**Passed**

Active Directory service discovery through DNS was operational.

---

# 5. DNS Configuration Validation

The Domain Controller DNS configuration was reviewed to verify the main enterprise DNS settings.

### Expected Configuration

| Setting             | Value                       |
| ------------------- | --------------------------- |
| DNS Server          | `192.168.1.2`               |
| Forward Lookup Zone | `manotech.local`            |
| Zone Type           | Active Directory Integrated |
| Dynamic Updates     | Secure Only                 |
| Domain              | `manotech.local`            |

## Validation

The following tests were performed:

```powershell
nslookup manotech.local
```

```powershell
nslookup SRV01-DC
```

```powershell
nslookup www.manotech.local
```

## Result

**Passed**

---

# 6. Troubleshooting Methodology

The DNS troubleshooting process followed a layered approach:

```text
Check Network Connectivity
          ↓
Check DNS Server Reachability
          ↓
Test Direct DNS Query
          ↓
Check Client DNS Configuration
          ↓
Check DNS Records
          ↓
Check Name Resolution Configuration
          ↓
Re-test Hostname Resolution
```

This approach helps determine whether the problem is caused by:

* Network connectivity.
* DNS server availability.
* Incorrect DNS configuration.
* Missing DNS records.
* Client resolver configuration.

---

# 7. Useful DNS Commands

## Windows

Check network configuration:

```powershell
ipconfig /all
```

Clear DNS cache:

```powershell
ipconfig /flushdns
```

Test DNS resolution:

```powershell
nslookup manotech.local
```

Test a specific DNS server:

```powershell
nslookup manotech.local 192.168.1.2
```

Test Active Directory SRV records:

```powershell
nslookup -type=SRV _ldap._tcp.dc._msdcs.manotech.local
```

---

## Linux

Test DNS resolution:

```bash
nslookup manotech.local
```

Test using the Domain Controller directly:

```bash
nslookup manotech.local 192.168.1.2
```

Test hostname resolution:

```bash
ping manotech.local
```

Review resolver configuration:

```bash
cat /etc/nsswitch.conf
```

---

# 8. Lessons Learned

The DNS troubleshooting process demonstrated several important concepts:

* Active Directory depends heavily on DNS.
* Domain clients should use the internal AD-integrated DNS server.
* Successful communication with a DNS server does not always mean normal hostname resolution is working.
* Linux name resolution can depend on `/etc/nsswitch.conf` in addition to DNS server configuration.
* DNS problems should be isolated before troubleshooting higher-level services such as Active Directory or IIS.
* DNS records should be validated using tools such as `nslookup`.

---

## Resolution Summary

| Issue                            | Status   |
| -------------------------------- | -------- |
| Windows Client DNS Configuration | Resolved |
| Active Directory DNS Resolution  | Resolved |
| IIS DNS Record                   | Resolved |
| AD SRV Record Resolution         | Passed   |
| Linux Hostname Resolution Issue  | Resolved |
| DNS Infrastructure Validation    | Passed   |

---

## Revision History

| Version | Date       | Author        | Description                               |
| ------- | ---------- | ------------- | ----------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial DNS troubleshooting documentation |
