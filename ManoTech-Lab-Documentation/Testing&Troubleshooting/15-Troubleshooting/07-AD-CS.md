# AD CS Troubleshooting

## Overview

This document records the certificate-related troubleshooting scenarios encountered during the deployment and configuration of Active Directory Certificate Services (AD CS) in the ManoTech Enterprise Windows Server Lab.

The main troubleshooting focus was certificate request failures, certificate template requirements, and enrollment configuration.

---

# 1. Certificate Request Failed

## Problem

A certificate request submitted to the Enterprise Certificate Authority failed and appeared under **Failed Requests** in the Certification Authority console.

## Symptoms

The certificate was not issued successfully.

The request appeared in the CA console with an error indicating that the required email information was unavailable.

## Error

```text
The EMail name is unavailable
```

## Root Cause

The certificate template being used required the user's email address to be available in Active Directory.

The affected user account did not contain the required email attribute.

The certificate template therefore could not populate the required certificate subject information.

## Investigation

The following components were reviewed:

* Active Directory user attributes.
* Certificate template configuration.
* Certificate enrollment permissions.
* Certification Authority failed requests.

The failed request was inspected through:

```text
Certification Authority
→ ManoTech-ROOT-CA
→ Failed Requests
```

## Resolution

The required user information was reviewed and corrected in Active Directory.

The certificate request was then submitted again using the configured certificate template.

## Result

**Resolved**

The certificate was successfully issued by:

```text
ManoTech-ROOT-CA
```

---

# 2. Certificate Template Configuration

## Problem

Certificate enrollment depends on the configuration of the certificate template.

An incorrectly configured template can prevent users or computers from successfully requesting certificates.

## Investigation

The certificate template configuration was reviewed to verify:

* Template purpose.
* Enrollment permissions.
* Subject information requirements.
* Key usage.
* Validity settings.

The Certificate Templates console was used to inspect the configuration.

## Resolution

The template requirements were reviewed and aligned with the available Active Directory user information.

## Result

**Passed**

Certificate requests could be processed successfully.

---

# 3. Certificate Enrollment Permissions

## Problem

Certificate enrollment can fail if the requesting user or computer does not have the required permissions on the certificate template.

## Investigation

The security permissions of the certificate template were reviewed.

The following were verified:

* Enrollment permissions.
* Applicable users or security groups.
* Certificate template availability on the Enterprise CA.

## Resolution

The appropriate enrollment permissions were confirmed.

## Result

**Passed**

Authorized domain users were able to request certificates.

---

# 4. Auto-Enrollment Troubleshooting

## Problem

A certificate was not automatically available after configuring certificate auto-enrollment.

## Possible Causes

* Auto-enrollment Group Policy not applied.
* Incorrect certificate template permissions.
* User or computer outside the applicable policy scope.
* Group Policy processing delay.

## Troubleshooting

The client was forced to refresh Group Policy:

```powershell
gpupdate /force
```

The applied policies were then checked using:

```powershell
gpresult /r
```

Certificate stores were also checked to determine whether the certificate had been successfully enrolled.

## Result

Auto-enrollment behavior was validated after correcting the applicable configuration.

---

# 5. Certificate Authority Validation

The Enterprise CA was checked to verify that it was operating correctly.

### CA

```text
ManoTech-ROOT-CA
```

### Validation Areas

* CA service availability.
* Certificate templates.
* Issued certificates.
* Failed requests.
* Certificate enrollment.

The Certification Authority console was used to review certificate request status.

## Result

**Passed**

The Enterprise Root CA was operational.

---

# 6. Certificate Store Validation

After successful enrollment, the issued certificate was verified in the appropriate certificate store.

For user certificates:

```text
certmgr.msc
```

For computer certificates:

```text
certlm.msc
```

The certificate was checked for:

* Issuer.
* Subject.
* Validity period.
* Certificate purpose.
* Trust chain.

## Result

**Passed**

The issued certificate was available in the certificate store.

---

# 7. Troubleshooting Methodology

The AD CS troubleshooting process followed this sequence:

```text
Certificate Request
        ↓
Check CA Status
        ↓
Check Failed Requests
        ↓
Identify Error
        ↓
Check Certificate Template
        ↓
Check AD User / Computer Attributes
        ↓
Check Enrollment Permissions
        ↓
Correct Configuration
        ↓
Request Certificate Again
        ↓
Verify Certificate Store
```

This approach helps identify whether the problem originates from the CA, certificate template, Active Directory attributes, or enrollment permissions.

---

# 8. Useful Tools and Commands

### Certification Authority

```text
certsrv.msc
```

### User Certificate Store

```text
certmgr.msc
```

### Computer Certificate Store

```text
certlm.msc
```

### Group Policy Update

```powershell
gpupdate /force
```

### Applied Group Policies

```powershell
gpresult /r
```

---

# 9. Lessons Learned

The AD CS troubleshooting process demonstrated several important PKI concepts:

* Certificate templates can impose requirements on Active Directory attributes.
* Failed certificate requests should be inspected through the CA console.
* Active Directory user information can directly affect certificate enrollment.
* Enrollment permissions must be configured correctly.
* Group Policy is important for certificate auto-enrollment.
* Successful certificate issuance should always be verified in the certificate store.
* Troubleshooting should start with the exact CA error rather than changing multiple settings at once.

---

## Resolution Summary

| Issue                           | Status    |
| ------------------------------- | --------- |
| Certificate Request Failure     | Resolved  |
| Missing Email Attribute         | Resolved  |
| Certificate Template Validation | Passed    |
| Enrollment Permissions          | Passed    |
| Auto-Enrollment Configuration   | Validated |
| CA Operation                    | Passed    |
| Certificate Store Validation    | Passed    |

---

## Revision History

| Version | Date       | Author        | Description                                 |
| ------- | ---------- | ------------- | ------------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial AD CS troubleshooting documentation |
