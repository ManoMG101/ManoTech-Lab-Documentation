# DNS Server

## Overview

The Domain Name System (DNS) is responsible for resolving hostnames to IP addresses within the ManoTech enterprise network.

DNS is a critical component of Active Directory and enables domain clients and servers to locate domain services and internal resources.

In this lab, DNS is integrated with Active Directory and hosted on the Domain Controller to provide internal name resolution and support Active Directory services.

---

## Server Information

| Property   | Value                       |
| ---------- | --------------------------- |
| Hostname   | DC01                        |
| IP Address | `192.168.1.2`               |
| Service    | DNS Server                  |
| Zone Type  | Active Directory Integrated |
| Domain     | `manotech.local`            |

---

## Objectives

The DNS infrastructure was implemented to:

* Support Active Directory services.
* Resolve internal hostnames.
* Allow clients to locate domain services.
* Provide reliable internal name resolution.
* Enable internal web services.
* Support domain authentication and Group Policy.
* Simplify enterprise network administration.

---

## DNS Configuration

| Setting             | Value            |
| ------------------- | ---------------- |
| DNS Server          | `192.168.1.2`    |
| Forward Lookup Zone | `manotech.local` |
| Reverse Lookup Zone | Configured       |
| Dynamic Updates     | Secure Only      |
| Zone Storage        | Active Directory |

The `manotech.local` zone is stored in Active Directory and uses secure dynamic updates.

---

## DNS Records

### Host (A) Records

The primary infrastructure hosts are registered in the internal DNS namespace.

| Hostname                |    IP Address |
| ----------------------- | ------------: |
| `dc01.manotech.local`   | `192.168.1.2` |
| `dhcp01.manotech.local` | `192.168.1.3` |
| `file01.manotech.local` | `192.168.1.4` |
| `web01.manotech.local`  | `192.168.1.5` |
| `rras01.manotech.local` | `192.168.1.1` |

---

## Internal Web Service

An internal DNS record is configured for the IIS web server:

```text
www.manotech.local
```

The record points to:

```text
192.168.1.5
```

This allows domain clients to access the internal ManoTech website using its hostname instead of its IP address.

---

## Active Directory DNS Records

Active Directory automatically creates and maintains required DNS service records during Domain Controller deployment.

These include SRV records used by domain clients to locate services such as:

* LDAP
* Kerberos
* Domain Controllers
* Global Catalog services

These records are essential for successful Active Directory authentication and domain operations.

---

## Client DNS Configuration

Domain clients use the Domain Controller as their primary DNS server:

```text
Preferred DNS:
192.168.1.2
```

The DNS configuration is provided to DHCP clients through the DHCP infrastructure.

Using the internal Active Directory DNS server ensures that clients can resolve:

* Domain names
* Domain Controllers
* Internal servers
* Internal applications
* Active Directory services

---

## Name Resolution Testing

DNS functionality was validated using several tests.

### Hostname Resolution

```text
ping dc01
ping file01
ping web01
```

### FQDN Resolution

```text
ping dc01.manotech.local
ping www.manotech.local
```

### NSLookup

```text
nslookup manotech.local
nslookup www.manotech.local
```

### DNS Configuration

```text
ipconfig /all
```

The tests verify that clients are using the correct DNS server and can successfully resolve internal resources.

---

## Common Troubleshooting

### Problem: Client Cannot Join the Domain

#### Possible Causes

* Incorrect DNS server configured on the client.
* Client is using an external DNS server.
* DNS service is unavailable.
* Network connectivity problems.
* Required Active Directory DNS records are missing.

#### Resolution

The client DNS configuration was verified to ensure that the Domain Controller at:

```text
192.168.1.2
```

is being used as the primary DNS server.

DNS resolution was then tested using `nslookup` and hostname/FQDN connectivity tests.

---

### Problem: Internal Hostname Cannot Be Resolved

#### Possible Causes

* Missing DNS record.
* Incorrect IP address.
* DNS service unavailable.
* Client DNS cache contains outdated information.

#### Resolution

The DNS record was verified in DNS Manager and name resolution was tested using:

```text
nslookup <hostname>
```

The client DNS cache can also be cleared when required:

```text
ipconfig /flushdns
```

---

### Problem: Internal Website Cannot Be Reached by Name

#### Possible Causes

* Missing `www` DNS record.
* Incorrect IP address.
* IIS service unavailable.
* Client DNS configuration is incorrect.

#### Resolution

The `www.manotech.local` DNS record was verified to point to:

```text
192.168.1.5
```

DNS resolution was then tested before troubleshooting the IIS service itself.

---

## DNS Validation Checklist

| Test                                 | Expected Result |
| ------------------------------------ | --------------- |
| Resolve `manotech.local`             | Successful      |
| Resolve `dc01.manotech.local`        | Successful      |
| Resolve `file01.manotech.local`      | Successful      |
| Resolve `web01.manotech.local`       | Successful      |
| Resolve `www.manotech.local`         | Successful      |
| Resolve Active Directory SRV records | Successful      |
| Client uses `192.168.1.2` as DNS     | Confirmed       |
| Domain name resolution               | Successful      |

---

## Notes

DNS is one of the most important services in an Active Directory environment.

Active Directory relies heavily on DNS for service discovery, authentication, domain operations, and Group Policy processing.

For this reason, the Domain Controller provides the primary internal DNS service for the `manotech.local` domain.

The DNS service is integrated with Active Directory and is not deployed as a separate virtual machine.

---

## Revision History

| Version | Date       | Author        | Description                                                                 |
| ------- | ---------- | ------------- | --------------------------------------------------------------------------- |
| 1.0     | 2026-07-26 | Mohamed Osama | Initial documentation                                                       |
| 1.1     | 2026-08-04 | Mohamed Osama | Reviewed and updated documentation                                          |
| 1.2     | 2026-08-09 | Mohamed Osama | Updated server naming, DNS architecture, records, and validation procedures |
