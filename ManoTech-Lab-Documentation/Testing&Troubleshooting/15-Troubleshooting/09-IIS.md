# IIS Troubleshooting

## Overview

This document records the IIS-related troubleshooting scenarios encountered during the deployment and configuration of the ManoTech Enterprise Windows Server Lab.

The troubleshooting focused on website accessibility, DNS integration, HTTPS configuration, certificates, and IIS bindings.

---

# 1. HTTPS Certificate Warning

## Problem

When HTTPS was configured for the internal IIS website, the browser displayed a certificate warning.

## Symptoms

The website was reachable through HTTPS, but the browser did not consider the certificate fully trusted.

## Possible Causes

* Certificate not trusted by the client.
* Incorrect certificate binding.
* Certificate subject does not match the requested hostname.
* Internal CA certificate not trusted by the client.

## Investigation

The IIS website certificate and its binding were reviewed.

The Enterprise CA used in the lab was:

```text
ManoTech-ROOT-CA
```

The website hostname was:

```text
www.manotech.local
```

The certificate configuration and IIS HTTPS binding were reviewed to identify the trust issue.

## Resolution

The certificate configuration was reviewed and the internal CA trust relationship was considered when validating the HTTPS configuration.

The issue was documented as part of the lab's PKI and IIS integration testing.

## Result

**Investigated / Validated**

---

# 2. HTTPS Redirect Loop

## Problem

An HTTPS redirect configuration caused the internal website to enter a redirect loop.

## Symptoms

The browser repeatedly redirected the request instead of successfully loading the website.

## Investigation

The IIS website was configured with a redirect from HTTP to HTTPS.

The redirect configuration was reviewed after the website became inaccessible due to repeated redirects.

The hostname involved during testing was:

```text
intranet.manotech.local
```

## Root Cause

The redirect configuration was creating a loop in the website request flow.

## Resolution

The problematic redirect configuration was removed.

The website was then tested again to confirm that the redirect loop no longer occurred.

## Result

**Resolved**

---

# 3. IIS Binding Validation

## Problem

IIS website accessibility depends on the correct website binding.

Incorrect bindings can prevent IIS from responding to requests using the intended hostname or port.

## Configuration Reviewed

The website binding was checked for:

* Protocol.
* IP address.
* Port.
* Hostname.
* SSL certificate when HTTPS was enabled.

### HTTP Configuration

```text
Protocol: HTTP
Port: 80
Hostname: www.manotech.local
```

### HTTPS

When HTTPS was tested, the corresponding certificate binding was reviewed.

## Result

**Passed**

The IIS website responded correctly after the binding configuration was corrected.

---

# 4. DNS and IIS Integration

## Problem

The website needed to be accessible using its internal DNS name rather than its IP address.

## DNS Record

```text
www.manotech.local
```

was configured to point to:

```text
192.168.1.5
```

## Validation

DNS resolution was tested using:

```powershell
nslookup www.manotech.local
```

The website was then accessed through:

```text
http://www.manotech.local
```

## Result

**Passed**

The IIS website successfully resolved through the internal DNS infrastructure.

---

# 5. Website Accessibility Testing

The website was tested from the Windows 11 domain client.

### IP-Based Test

```text
http://192.168.1.5
```

### DNS-Based Test

```text
http://www.manotech.local
```

The tests were used to distinguish between:

* IIS availability problems.
* DNS resolution problems.
* Binding problems.

## Result

The website was accessible from the domain client.

---

# 6. Troubleshooting Methodology

The IIS troubleshooting process followed this sequence:

```text
Check Network Connectivity
        ↓
Check DNS Resolution
        ↓
Check IIS Service
        ↓
Check Website Status
        ↓
Check Bindings
        ↓
Check Certificates
        ↓
Check Redirect Configuration
        ↓
Test Website Again
```

This approach helps isolate whether the issue is related to networking, DNS, IIS, certificates, or website configuration.

---

# 7. Useful Tools and Commands

Test IIS server connectivity:

```powershell
ping 192.168.1.5
```

Test DNS resolution:

```powershell
nslookup www.manotech.local
```

Check IIS service:

```powershell
Get-Service W3SVC
```

Test HTTP connectivity:

```powershell
Invoke-WebRequest http://www.manotech.local
```

IIS management console:

```text
inetmgr
```

---

# 8. Lessons Learned

The IIS troubleshooting process demonstrated several important concepts:

* IIS bindings must match the requested hostname and protocol.
* DNS configuration is essential for hostname-based web access.
* HTTPS requires a correctly configured and trusted certificate.
* Redirect rules must be carefully configured to avoid redirect loops.
* Internal PKI and IIS can be integrated to provide trusted HTTPS services.
* Troubleshooting should isolate DNS, IIS, certificate, and redirect issues separately.

---

## Resolution Summary

| Issue                     | Status                   |
| ------------------------- | ------------------------ |
| HTTPS Certificate Warning | Investigated / Validated |
| HTTPS Redirect Loop       | Resolved                 |
| IIS Binding Configuration | Passed                   |
| DNS → IIS Integration     | Passed                   |
| Website Accessibility     | Passed                   |

---

## Revision History

| Version | Date       | Author        | Description                               |
| ------- | ---------- | ------------- | ----------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial IIS troubleshooting documentation |
