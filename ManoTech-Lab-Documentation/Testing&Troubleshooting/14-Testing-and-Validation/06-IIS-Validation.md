# IIS Validation

## Overview

This document describes the validation tests performed to verify the IIS Web Server and the internal ManoTech website.

The validation confirms that IIS is running correctly, the website is accessible through the internal network, and DNS correctly resolves the website hostname to the IIS server.

---

## IIS Server Information

| Property         | Value               |
| ---------------- | ------------------- |
| Hostname         | `SRV04-WEBSERVER`   |
| IP Address       | `192.168.1.5`       |
| Operating System | Windows Server 2019 |
| Service          | IIS Web Server      |
| Domain           | `manotech.local`    |

---

## Website Information

| Property      | Value                       |
| ------------- | --------------------------- |
| Website Name  | `ManoTech Internal Website` |
| Physical Path | `C:\inetpub\wwwroot`        |
| Protocol      | HTTP                        |
| Port          | `80`                        |
| Hostname      | `www.manotech.local`        |

---

# 1. IIS Service Validation

The IIS services were checked to verify that the web server is operational.

### Test

```powershell id="w3r1h6"
Get-Service W3SVC
```

### Expected Result

The World Wide Web Publishing Service should be running.

```text
Status: Running
```

### Result

**Passed**

---

# 2. IIS Website Status

The IIS Manager was checked to verify the status of the internal website.

### Expected Result

The website should be in the:

```text
Started
```

state.

### Result

**Passed**

---

# 3. Local Website Access

The website was tested directly from the IIS server.

### Test

```powershell id="u3m1b8"
curl http://localhost
```

### Expected Result

The IIS server should return an HTTP response from the configured website.

### Result

**Passed**

---

# 4. Website Access Using Server IP

The website was accessed using the IIS server's IP address.

### Test

```text
http://192.168.1.5
```

### Expected Result

The internal website should load successfully.

### Result

**Passed**

---

# 5. DNS Resolution

The website hostname was tested to verify DNS integration.

### Hostname

```text
www.manotech.local
```

### Test

```powershell id="x8h6yk"
nslookup www.manotech.local
```

### Expected Result

The hostname should resolve to:

```text
192.168.1.5
```

### Result

**Passed**

---

# 6. Website Access Using DNS Name

The website was accessed using its configured DNS hostname.

### Test

```text
http://www.manotech.local
```

### Expected Result

The website should load successfully using the DNS hostname.

### Result

**Passed**

---

# 7. HTTP Port Validation

The HTTP service was tested to verify that port `80` is accessible.

### Test

```powershell id="v2wz0u"
Test-NetConnection 192.168.1.5 -Port 80
```

### Expected Result

```text
TcpTestSucceeded : True
```

### Result

**Passed**

---

# 8. Domain Client Access

The Windows 11 domain client was used to access the internal website.

### Test

```text
http://www.manotech.local
```

### Expected Result

The website should be accessible from the domain client.

### Result

**Passed**

---

# 9. DNS and IIS Integration

The complete request path was validated:

```text
Windows 11 Client
        │
        ↓
DNS Query
        │
        ↓
www.manotech.local
        │
        ↓
192.168.1.5
        │
        ↓
SRV04-WEBSERVER
        │
        ↓
IIS
        │
        ↓
ManoTech Internal Website
```

### Result

**Passed**

The client successfully resolved the website hostname and connected to the IIS server.

---

# 10. IIS Validation Summary

| Test                 | Expected Result           | Status |
| -------------------- | ------------------------- | ------ |
| IIS Service          | Running                   | Passed |
| Website Status       | Started                   | Passed |
| Local Website Access | Successful                | Passed |
| IP-Based Access      | Successful                | Passed |
| DNS Resolution       | Resolves to `192.168.1.5` | Passed |
| DNS-Based Access     | Successful                | Passed |
| HTTP Port 80         | Accessible                | Passed |
| Domain Client Access | Successful                | Passed |
| DNS/IIS Integration  | Functional                | Passed |

---

# 11. Validation Result

The IIS Web Server was successfully validated.

The ManoTech Internal Website is hosted on `SRV04-WEBSERVER` and is accessible from domain clients through both the server IP address and the DNS hostname:

```text
http://www.manotech.local
```

The validation confirms successful integration between:

* IIS
* DNS
* Active Directory
* Windows 11 Domain Client

---

## Tools Used

* IIS Manager
* PowerShell
* `curl`
* `nslookup`
* `Test-NetConnection`
* Web Browser
* DNS Manager

---

## Revision History

| Version | Date       | Author        | Description                          |
| ------- | ---------- | ------------- | ------------------------------------ |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial IIS validation documentation |
