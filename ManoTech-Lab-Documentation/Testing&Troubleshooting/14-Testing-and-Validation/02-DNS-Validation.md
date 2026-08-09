# DNS Validation

## Overview

This document describes the validation tests performed to verify the functionality of the DNS infrastructure within the ManoTech Enterprise Windows Server Lab.

DNS is a critical dependency for Active Directory and internal services. The tests verify hostname resolution, FQDN resolution, DNS records, and client DNS configuration.

---

## DNS Server

| Property   | Value                       |
| ---------- | --------------------------- |
| Hostname   | `SRV01-DC`                  |
| IP Address | `192.168.1.2`               |
| DNS Zone   | `manotech.local`            |
| Zone Type  | Active Directory Integrated |

---

# 1. DNS Server Connectivity

The Windows 11 client was tested for connectivity with the Domain Controller hosting DNS.

### Test

```powershell
ping 192.168.1.2
```

### Expected Result

The Domain Controller should respond successfully.

### Result

**Passed**

---

# 2. DNS Server Configuration

The client DNS configuration was verified using:

```powershell
ipconfig /all
```

### Expected Configuration

```text
DNS Server: 192.168.1.2
DNS Domain: manotech.local
```

### Result

**Passed**

The domain client was configured to use the internal Domain Controller as its DNS server.

---

# 3. Forward Lookup Testing

The `manotech.local` forward lookup zone was tested to verify hostname resolution.

### Test

```powershell
nslookup manotech.local
```

### Expected Result

The domain name should resolve using the internal DNS server.

### Result

**Passed**

---

# 4. Hostname Resolution

Infrastructure hostnames were tested using DNS.

### Tests

```powershell
nslookup SRV01-DC
nslookup SRV02-DHCP
nslookup SRV03-FILESRV
nslookup SRV04-WEBSERVER
nslookup SRV05-RRAS
```

### Expected Result

Each hostname should resolve to its corresponding IP address.

### Expected Results

| Hostname        |   Expected IP |
| --------------- | ------------: |
| SRV01-DC        | `192.168.1.2` |
| SRV02-DHCP      | `192.168.1.3` |
| SRV03-FILESRV   | `192.168.1.4` |
| SRV04-WEBSERVER | `192.168.1.5` |
| SRV05-RRAS      | `192.168.1.1` |

### Result

**Passed**

---

# 5. FQDN Resolution

Fully Qualified Domain Names were tested to verify domain-based name resolution.

### Test

```powershell
nslookup SRV01-DC.manotech.local
```

Additional server FQDNs were tested where applicable.

### Expected Result

The FQDN should resolve to the correct internal IP address.

### Result

**Passed**

---

# 6. Internal Website DNS Resolution

The DNS record used by the internal IIS website was tested.

### DNS Record

```text
www.manotech.local
```

### Test

```powershell
nslookup www.manotech.local
```

### Expected Result

The hostname should resolve to:

```text
192.168.1.5
```

### Result

**Passed**

The internal website DNS record successfully resolved to the IIS server.

---

# 7. Reverse Lookup Testing

The reverse lookup zone was checked to verify reverse DNS functionality.

### Test

```powershell
nslookup 192.168.1.2
```

Additional infrastructure IP addresses can be tested in the same way.

### Expected Result

The IP address should resolve to the corresponding hostname when a PTR record is available.

### Result

**Passed**

---

# 8. Active Directory DNS Records

Active Directory automatically creates and maintains important DNS records used by domain services.

The DNS zone was checked for Active Directory-related records, including:

* Host (A) records.
* SRV records.
* Domain Controller service records.

### Validation

The presence of the required Active Directory DNS records was verified through DNS Manager and `nslookup`.

### Result

**Passed**

---

# 9. DNS and Active Directory Integration

DNS functionality was validated as part of the Active Directory environment.

The following operations depend on correct DNS configuration:

* Domain discovery.
* Domain join.
* Domain authentication.
* Group Policy processing.
* Internal service discovery.

The Windows 11 client successfully communicated with the Domain Controller and remained joined to:

```text
manotech.local
```

### Result

**Passed**

---

# 10. DNS Validation Summary

| Test                     | Expected Result                       | Status |
| ------------------------ | ------------------------------------- | ------ |
| DNS Server Connectivity  | DNS server reachable                  | Passed |
| Client DNS Configuration | `192.168.1.2` configured              | Passed |
| Domain Resolution        | `manotech.local` resolves             | Passed |
| Hostname Resolution      | Infrastructure names resolve          | Passed |
| FQDN Resolution          | FQDNs resolve correctly               | Passed |
| IIS DNS Record           | `www.manotech.local` resolves         | Passed |
| Reverse Lookup           | PTR resolution works where configured | Passed |
| AD DNS Records           | Required records available            | Passed |
| AD/DNS Integration       | Domain services operate correctly     | Passed |

---

# 11. Validation Result

The DNS infrastructure was successfully validated.

The Domain Controller provides internal DNS services for the `manotech.local` domain, and domain clients use `192.168.1.2` as their DNS server.

Hostname and FQDN resolution were successfully tested for the main infrastructure services, including the internal IIS website.

The successful DNS configuration also supports Active Directory domain operations and internal service discovery.

---

## Tools Used

* DNS Manager
* `ipconfig`
* `ping`
* `nslookup`
* Active Directory Users and Computers
* Windows Server 2019

---

## Revision History

| Version | Date       | Author        | Description                          |
| ------- | ---------- | ------------- | ------------------------------------ |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial DNS validation documentation |
