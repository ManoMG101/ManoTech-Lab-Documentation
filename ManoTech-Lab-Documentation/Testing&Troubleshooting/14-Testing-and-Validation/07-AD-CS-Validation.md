# AD CS Validation

## Overview

This document describes the validation tests performed to verify the Active Directory Certificate Services (AD CS) infrastructure within the ManoTech Enterprise Windows Server Lab.

The validation confirms that the Enterprise Root CA is operational, certificate templates are available, certificate enrollment is functioning, and issued certificates can be accessed by domain users and computers.

---

## Certificate Authority Information

| Property         | Value                                 |
| ---------------- | ------------------------------------- |
| Hostname         | `SRV01-DC`                            |
| IP Address       | `192.168.1.2`                         |
| Operating System | Windows Server 2019                   |
| Role             | Active Directory Certificate Services |
| CA Type          | Enterprise Root CA                    |
| CA Name          | `ManoTech-ROOT-CA`                    |
| Domain           | `manotech.local`                      |

---

# 1. Certificate Authority Service Validation

The Certificate Services service was checked to verify that the CA is operational.

### Test

```powershell id="d4y6jv"
Get-Service CertSvc
```

### Expected Result

```text
Status: Running
```

### Result

**Passed**

---

# 2. Certification Authority Availability

The Certification Authority console was opened to verify the CA status.

### Validation

Verified:

* CA is online.
* CA name is `ManoTech-ROOT-CA`.
* Certificate requests can be processed.
* Issued certificates are visible.

### Result

**Passed**

---

# 3. Certificate Templates Validation

Available certificate templates were checked through the Certificate Templates console.

### Templates Tested

* User Certificate
* Computer Certificate

### Expected Result

Required templates should be available for enrollment.

### Result

**Passed**

---

# 4. Certificate Enrollment Test

Certificate enrollment was tested using a domain user.

### Validation Process

1. Log in using a domain account.
2. Open the certificate management interface.
3. Request a certificate using an available template.
4. Submit the certificate request.
5. Verify that the certificate is issued.

### Expected Result

The certificate request should be successfully processed by:

```text
ManoTech-ROOT-CA
```

### Result

**Passed**

---

# 5. Issued Certificate Validation

After successful enrollment, the issued certificate was inspected.

### Validation

The following information was verified:

* Certificate subject.
* Issuing CA.
* Validity period.
* Enhanced Key Usage.
* Certificate status.

### Expected Result

The certificate should be valid and trusted by the domain environment.

### Result

**Passed**

---

# 6. Certificate Store Validation

The issued certificate was verified inside the user's certificate store.

### Tool

```text
certmgr.msc
```

### Location

```text
Personal
└── Certificates
```

### Expected Result

The issued certificate should appear in the user's Personal certificate store.

### Result

**Passed**

---

# 7. CA Trust Validation

The Enterprise Root CA trust relationship was verified.

### Validation

The client was checked to confirm that the CA certificate is trusted by the domain environment.

### Expected Result

Certificates issued by:

```text
ManoTech-ROOT-CA
```

should be recognized as trusted certificates.

### Result

**Passed**

---

# 8. Auto-Enrollment Validation

Certificate auto-enrollment configuration was tested through Group Policy.

### Configuration Path

```text
Computer Configuration
→ Policies
→ Windows Settings
→ Security Settings
→ Public Key Policies
→ Certificate Services Client - Auto-Enrollment
```

### Test

```powershell id="1x4q1y"
gpupdate /force
```

### Expected Result

The applicable domain users or computers should automatically enroll for configured certificates.

### Result

**Passed**

---

# 9. Certificate Request Troubleshooting Validation

A certificate enrollment failure was previously encountered during the implementation.

### Problem

A certificate request was rejected because the certificate template required an Email attribute that was not available for the user account.

### Error

```text
The EMail name is unavailable
```

### Resolution

The required user information and certificate template configuration were reviewed and corrected.

The certificate was then requested again successfully.

### Result

**Passed**

This confirms that certificate enrollment and template requirements were successfully validated.

---

# 10. AD CS Integration Validation

The complete certificate enrollment path was validated:

```text
Domain User / Computer
        │
        ↓
Certificate Template
        │
        ↓
Active Directory
        │
        ↓
ManoTech-ROOT-CA
        │
        ↓
Issued Certificate
        │
        ↓
Certificate Store
```

### Result

**Passed**

---

# 11. AD CS Validation Summary

| Test                   | Expected Result     | Status |
| ---------------------- | ------------------- | ------ |
| Certificate Services   | Running             | Passed |
| Enterprise Root CA     | Online              | Passed |
| CA Name                | `ManoTech-ROOT-CA`  | Passed |
| Certificate Templates  | Available           | Passed |
| Certificate Enrollment | Successful          | Passed |
| Issued Certificate     | Valid               | Passed |
| Certificate Store      | Certificate visible | Passed |
| CA Trust               | Trusted             | Passed |
| Auto-Enrollment        | Functional          | Passed |
| AD CS Integration      | Functional          | Passed |

---

# 12. Validation Result

The Active Directory Certificate Services infrastructure was successfully validated.

The `ManoTech-ROOT-CA` Enterprise Root CA is operational and integrated with the `manotech.local` Active Directory environment.

Certificate templates are available, certificate enrollment was successfully tested, and issued certificates were verified in the appropriate certificate stores.

The troubleshooting and correction of the Email attribute requirement also confirmed that certificate template requirements and enrollment permissions are functioning as expected.

---

## Tools Used

* Certification Authority Console (`certsrv.msc`)
* Certificate Templates Console
* Certificate Manager (`certmgr.msc`)
* Group Policy Management
* PowerShell
* Active Directory Users and Computers

---

## Revision History

| Version | Date       | Author        | Description                            |
| ------- | ---------- | ------------- | -------------------------------------- |
| 1.0     | 2026-08-09 | Mohamed Osama | Initial AD CS validation documentation |
