# IIS Web Server

## Overview

Internet Information Services (IIS) is Microsoft's web server platform used to host websites, web applications, and web services.

In the ManoTech Enterprise Windows Server Lab, IIS is deployed on WEB01 to host the internal company website and demonstrate integration between IIS, DNS, Active Directory, and the internal PKI.

---

## Server Information

| Property         | Value               |
| ---------------- | ------------------- |
| Hostname         | WEB01               |
| IP Address       | `192.168.1.5`       |
| Operating System | Windows Server 2019 |
| Service          | IIS Web Server      |
| Domain           | `manotech.local`    |

---

## Objectives

The IIS Web Server was deployed to:

* Host the internal company website.
* Provide centralized access to internal web resources.
* Practice IIS administration.
* Integrate web hosting with internal DNS.
* Demonstrate internal HTTPS certificate deployment.
* Simulate an enterprise internal web portal.

---

## Website Information

| Property      | Value                     |
| ------------- | ------------------------- |
| Website Name  | ManoTech Internal Website |
| Server        | WEB01                     |
| IP Address    | `192.168.1.5`             |
| Physical Path | `C:\inetpub\wwwroot`      |
| HTTP Port     | `80`                      |
| HTTPS Port    | `443` where configured    |
| Status        | Running                   |

---

## DNS Integration

The internal DNS server on DC01 provides name resolution for the IIS server.

The following DNS record is configured:

| Record Name          | Type     |    IP Address |
| -------------------- | -------- | ------------: |
| `www.manotech.local` | A Record | `192.168.1.5` |

The website can therefore be accessed using:

```text id="r89g52"
http://www.manotech.local
```

This removes the need for users to access the website directly using the server IP address.

---

## IIS Configuration

The ManoTech internal website was configured in IIS with the required website settings.

Key configuration areas include:

* Website name
* Physical path
* IP address
* HTTP binding
* HTTPS binding where applicable
* Port configuration
* Website content
* Default documents
* Static Content
* IIS Management Console

The website is hosted on WEB01 and is accessible to domain clients through the internal network.

---

## HTTP Configuration

The initial web service is available over HTTP:

```text id="l2n7kd"
http://www.manotech.local
```

The IIS site listens on port:

```text id="c4i8sy"
80
```

The HTTP configuration was used to validate basic IIS functionality and DNS integration.

---

## HTTPS and Internal PKI

The lab also demonstrates internal certificate usage through the ManoTech PKI.

The internal Certificate Authority is hosted on DC01:

```text id="z6r2pw"
ManoTech-ROOT-CA
```

A web server certificate was created for the IIS environment and can be used to provide HTTPS access to internal web services.

The intended HTTPS configuration is:

```text id="k8z0qh"
https://www.manotech.local
```

The certificate must contain the appropriate DNS name used by the website.

---

## Certificate Integration

The IIS server uses the internal PKI to demonstrate enterprise certificate management.

The certificate workflow is:

```text id="c3q6f1"
ManoTech-ROOT-CA
        │
        │ Certificate
        ▼
      WEB01
        │
        ▼
       IIS
        │
        ▼
https://www.manotech.local
```

This demonstrates integration between:

* Active Directory Certificate Services
* DNS
* IIS
* Internal domain clients

---

## Validation

The IIS deployment was validated through several tests.

### IIS Service

The IIS service was verified to be running on WEB01.

### IP Access

The website was tested using the server IP:

```text id="v1t7mx"
http://192.168.1.5
```

### DNS Access

The website was tested using its internal DNS name:

```text id="g4x6sa"
http://www.manotech.local
```

### DNS Resolution

DNS resolution was tested using:

```text id="k6m9t2"
nslookup www.manotech.local
```

The expected result is:

```text id="x3p1kw"
192.168.1.5
```

### HTTPS

Where HTTPS is configured, the website can be tested using:

```text id="e7m2sa"
https://www.manotech.local
```

The certificate should match the hostname used by the website and be trusted by clients that trust the internal ManoTech Certificate Authority.

---

## Common Troubleshooting

### Issue: Website Cannot Be Accessed

#### Possible Causes

* IIS service stopped.
* Incorrect website binding.
* Incorrect physical path.
* Windows Firewall blocking HTTP/HTTPS.
* Website stopped in IIS Manager.
* Network connectivity problem.

#### Resolution

The following were verified:

1. IIS service status.
2. Website status.
3. Website bindings.
4. Physical path.
5. Firewall configuration.
6. Network connectivity to WEB01.

---

### Issue: Website Cannot Be Accessed Using Hostname

#### Possible Causes

* Missing DNS A record.
* Incorrect DNS configuration on the client.
* DNS service unavailable.
* Incorrect hostname.

#### Resolution

The DNS record was verified:

```text id="sl6g0b"
www.manotech.local → 192.168.1.5
```

The client DNS configuration was also checked to ensure that DC01:

```text id="b1t7ko"
192.168.1.2
```

is being used as the DNS server.

---

### Issue: HTTPS Certificate Warning

#### Possible Causes

* Certificate issued by an untrusted CA.
* Certificate hostname does not match the requested hostname.
* Certificate has expired.
* Certificate is not correctly installed on WEB01.
* HTTPS binding is configured incorrectly.

#### Resolution

The certificate was checked for:

* Correct subject/SAN.
* Validity period.
* Correct certificate chain.
* Trust relationship with `ManoTech-ROOT-CA`.
* Correct IIS HTTPS binding.

---

### Issue: HTTPS Redirect or Binding Problems

During the IIS configuration process, HTTPS redirection and bindings were tested.

Incorrect redirect configuration can result in redirect loops or prevent the internal website from loading correctly.

The final IIS configuration should use only the required bindings and avoid unnecessary redirect rules.

---

## Security Considerations

For production environments, IIS should follow security best practices including:

* HTTPS instead of unencrypted HTTP.
* Trusted certificates.
* Least-privilege administrative access.
* Regular security updates.
* Firewall restrictions.
* Secure application configuration.
* Removal of unnecessary IIS features.
* Proper logging and monitoring.

The lab demonstrates these concepts through internal HTTPS certificate integration and controlled internal access.

---

## Notes

The IIS Web Server demonstrates the integration of multiple enterprise services:

```text id="e3t5z8"
DNS
 │
 ▼
www.manotech.local
 │
 ▼
WEB01
 │
 ▼
IIS
 │
 ▼
Internal Website
```

The internal PKI adds certificate-based HTTPS support:

```text id="p8c4x1"
DC01
└── ManoTech-ROOT-CA
          │
          ▼
        WEB01
          │
          ▼
         IIS
          │
          ▼
 HTTPS Internal Website
```

This provides a practical example of integrating IIS with Active Directory, DNS, and enterprise PKI.

---

## Revision History

| Version | Date       | Author        | Description                                                                                    |
| ------- | ---------- | ------------- | ---------------------------------------------------------------------------------------------- |
| 1.0     | 2026-07-26 | Mohamed Osama | Initial documentation                                                                          |
| 1.1     | 2026-08-04 | Mohamed Osama | Reviewed and updated documentation                                                             |
| 1.2     | 2026-08-09 | Mohamed Osama | Updated server naming, DNS integration, internal PKI, HTTPS configuration, and troubleshooting |
