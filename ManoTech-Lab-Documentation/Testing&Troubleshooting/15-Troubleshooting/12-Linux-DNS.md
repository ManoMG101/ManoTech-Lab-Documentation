# Linux DNS Troubleshooting

## Overview

This document records the DNS resolution issue encountered while integrating the Linux environment with the ManoTech Active Directory infrastructure.

The issue occurred on the Ubuntu-based Monitor Server while attempting to resolve the internal Active Directory domain.

---

# 1. DNS Resolution Failure

## Problem

The Ubuntu server was able to communicate with the Active Directory DNS server directly, but normal hostname resolution for the internal domain was failing.

The affected domain was:

```text
manotech.local
```

The Active Directory DNS Server was:

```text
192.168.1.2
```

## Symptoms

Direct DNS queries to the Active Directory DNS server were successful, but normal hostname resolution from the Linux system failed.

For example, DNS queries directed explicitly to the Domain Controller worked correctly, while standard hostname resolution did not.

This indicated that the DNS server itself was reachable and responding, but the Linux name-resolution configuration was preventing the request from reaching the configured DNS server correctly.

---

# 2. Investigation

The Linux resolver configuration was reviewed.

The `/etc/nsswitch.conf` file contained:

```text
hosts: files mdns4_minimal [NOTFOUND=return] dns
```

The `mdns4_minimal` resolver configuration could terminate hostname resolution before the normal DNS resolver was consulted.

This caused problems when resolving the internal Active Directory domain.

---

# 3. Resolution

The `hosts` configuration in `/etc/nsswitch.conf` was changed from:

```text
hosts: files mdns4_minimal [NOTFOUND=return] dns
```

to:

```text
hosts: files dns
```

This configuration allows the system to use local hosts files first and then query the configured DNS server.

---

# 4. Validation

After modifying the resolver configuration, the internal Active Directory domain was tested again.

The following hostname became resolvable:

```text
manotech.local
```

The Linux server was then able to resolve the internal domain through the Active Directory DNS server.

## Result

**Resolved**

---

# 5. Root Cause

The Active Directory DNS server was not the root cause of the issue.

The problem was caused by the Linux Name Service Switch configuration in:

```text
/etc/nsswitch.conf
```

The resolver order prevented normal DNS resolution from reaching the configured DNS server as expected.

---

# 6. Troubleshooting Methodology

The troubleshooting process followed this sequence:

```text
Test Network Connectivity
        ↓
Test Direct DNS Query
        ↓
Compare Direct vs Normal Resolution
        ↓
Inspect /etc/nsswitch.conf
        ↓
Identify Resolver Order Issue
        ↓
Modify hosts Configuration
        ↓
Test Domain Resolution Again
```

This helped isolate the problem to the Linux resolver configuration rather than the Windows DNS infrastructure.

---

# 7. Useful Linux Commands

Check DNS configuration:

```bash
cat /etc/resolv.conf
```

Check Name Service Switch configuration:

```bash
cat /etc/nsswitch.conf
```

Test hostname resolution:

```bash
ping manotech.local
```

Test DNS directly against the Domain Controller:

```bash
nslookup manotech.local 192.168.1.2
```

Test the configured resolver:

```bash
getent hosts manotech.local
```

---

# 8. Lessons Learned

This issue demonstrated that DNS troubleshooting is not limited to checking the DNS server itself.

Important lessons included:

* Verify network connectivity before troubleshooting DNS.
* Test the DNS server directly to determine whether it is responding.
* Compare direct DNS queries with normal system hostname resolution.
* Linux uses `/etc/nsswitch.conf` to determine how hostname resolution is performed.
* Active Directory environments depend heavily on reliable DNS resolution.
* Cross-platform integration requires understanding how Windows and Linux handle name resolution differently.

---

## Resolution Summary

| Issue                       | Status     |
| --------------------------- | ---------- |
| Linux → AD DNS Connectivity | Passed     |
| Direct DNS Query            | Passed     |
| Normal Hostname Resolution  | Failed     |
| `/etc/nsswitch.conf` Issue  | Identified |
| Resolver Configuration      | Corrected  |
| `manotech.local` Resolution | Passed     |

---

## Revision History

| Version | Date       | Author        | Description                                     |
| ------- | ---------- | ------------- | ----------------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial Linux DNS troubleshooting documentation |
