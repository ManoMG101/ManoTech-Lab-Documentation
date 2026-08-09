# Active Directory Certificate Services (AD CS)

## Overview

Active Directory Certificate Services (AD CS) is Microsoft's Public Key Infrastructure (PKI) solution for issuing and managing digital certificates within an enterprise environment.

In the ManoTech Enterprise Windows Server Lab, AD CS is deployed as an Enterprise Root Certification Authority (CA) on the Domain Controller.

The internal PKI provides certificate services for domain users, computers, and internal infrastructure services.

---

## Server Information

| Property         | Value                                 |
| ---------------- | ------------------------------------- |
| Hostname         | DC01                                  |
| IP Address       | `192.168.1.2`                         |
| Operating System | Windows Server 2019                   |
| Role             | Active Directory Certificate Services |
| CA Type          | Enterprise Root CA                    |
| CA Name          | `ManoTech-ROOT-CA`                    |
| Domain           | `manotech.local`                      |

---

## Objectives

AD CS was implemented to:

* Deploy an internal enterprise PKI.
* Issue digital certificates to domain users and computers.
* Provide centralized certificate management.
* Support certificate-based security.
* Provide certificates for internal services.
* Integrate certificate enrollment with Active Directory.
* Demonstrate enterprise certificate lifecycle management.

---

## AD CS Architecture

The current PKI architecture uses an Enterprise Root CA integrated directly with the Active Directory domain.

```text id="z6q1rp"
manotech.local
│
└── DC01
    │
    └── ManoTech-ROOT-CA
        │
        ├── User Certificates
        ├── Computer Certificates
        └── Web Server Certificates
```

The CA is integrated with Active Directory, allowing domain-based certificate enrollment and centralized certificate management.

---

## Certificate Authority Configuration

The Enterprise Root CA was configured with the following parameters:

| Setting                | Value                        |
| ---------------------- | ---------------------------- |
| CA Type                | Enterprise Root CA           |
| CA Name                | `ManoTech-ROOT-CA`           |
| Cryptographic Provider | RSA                          |
| Key Length             | 2048 bits                    |
| Database               | Default CA Database Location |
| Integration            | Active Directory             |

The CA certificate and private key are managed by the Certification Authority service on DC01.

---

## Certificate Templates

Certificate templates define how certificates are issued and what they can be used for.

The lab uses certificate templates for different certificate purposes, including:

* User certificates
* Computer certificates
* Web server certificates

Templates control:

* Certificate purpose.
* Key usage.
* Enrollment permissions.
* Certificate validity.
* Subject/SAN configuration where applicable.

---

## Web Server Certificate

AD CS was also integrated with the IIS Web Server.

A dedicated web server certificate template was created for the internal IIS environment.

The certificate is intended for the internal website hosted on:

```text id="v4c2n7"
WEB01
192.168.1.5
```

The internal website uses:

```text id="k9m3sd"
www.manotech.local
```

The certificate allows the IIS server to provide HTTPS using a certificate issued by the internal ManoTech CA.

---

## Certificate Enrollment

Certificate enrollment was tested within the domain environment.

The general enrollment workflow is:

```text id="q7w2kx"
Domain User / Computer
        │
        ▼
Certificate Template
        │
        ▼
ManoTech-ROOT-CA
        │
        ▼
Certificate Request
        │
        ▼
Issued Certificate
        │
        ▼
User / Computer Certificate Store
```

The CA verifies the enrollment request against the permissions and requirements defined by the certificate template.

---

## Auto-Enrollment

Certificate auto-enrollment can be configured through Group Policy to automatically issue certificates to eligible domain users and computers.

The relevant Group Policy setting is located under:

```text id="h3f8pa"
Computer Configuration
    ↓
Policies
    ↓
Windows Settings
    ↓
Security Settings
    ↓
Public Key Policies
    ↓
Certificate Services Client - Auto-Enrollment
```

Auto-enrollment allows certificate management to be performed centrally without requiring users to manually request certificates.

The final auto-enrollment scope will follow the finalized Active Directory OU and GPO structure.

---

## Validation

The AD CS deployment was validated through the following tests:

* Enterprise Root CA installed successfully.
* `ManoTech-ROOT-CA` available in the Certification Authority console.
* Certificate templates available.
* Certificate requests processed.
* Certificates successfully issued.
* Issued certificates visible in certificate stores.
* Domain integration verified.
* Web server certificate workflow tested.

Certificate management can be verified using:

```text id="q4j9xn"
certsrv.msc
```

User and computer certificates can be inspected using:

```text id="p6m2vb"
certmgr.msc
```

---

## Troubleshooting

### Issue 1 — Certificate Request Failed

#### Error

A certificate request was rejected because required user information was unavailable.

#### Cause

The selected certificate template required an **Email** attribute, but the corresponding Active Directory user account did not contain the required email information.

#### Resolution

The Active Directory user attributes and certificate template requirements were reviewed.

The missing user information was corrected before submitting the certificate request again.

This resolved the certificate enrollment failure.

---

### Issue 2 — Certificate Not Automatically Issued

#### Possible Causes

* Auto-enrollment policy not applied.
* Incorrect Group Policy configuration.
* User or computer does not have enrollment permissions.
* Certificate template not published correctly.
* GPO scope does not include the target user or computer.

#### Resolution

The following were verified:

* Certificate template configuration.
* Enrollment permissions.
* Group Policy configuration.
* GPO scope.
* Domain membership.

The following command can be used to force Group Policy processing:

```powershell
gpupdate /force
```

---

### Issue 3 — IIS Certificate Warning

#### Possible Causes

* Client does not trust the internal CA.
* Certificate hostname does not match the website hostname.
* Certificate has expired.
* Incorrect certificate installed on WEB01.
* HTTPS binding is configured incorrectly.

#### Resolution

The following certificate properties should be verified:

* Certificate validity.
* Certificate chain.
* Subject/SAN.
* Trusted Root CA.
* IIS HTTPS binding.
* Website hostname.

The certificate chain should terminate at:

```text id="a8s1jd"
ManoTech-ROOT-CA
```

---

## Management Tools

The following Windows tools were used to manage and validate the PKI environment:

| Tool                                 | Purpose                            |
| ------------------------------------ | ---------------------------------- |
| `certsrv.msc`                        | Certification Authority Management |
| Certificate Templates Console        | Certificate Template Management    |
| `certmgr.msc`                        | User Certificate Management        |
| Group Policy Management              | Auto-Enrollment Configuration      |
| Active Directory Users and Computers | User Attribute Management          |

---

## Security Considerations

The internal CA is a critical security component and should be protected appropriately.

Recommended security practices include:

* Protect the CA private key.
* Restrict administrative access to the CA.
* Limit certificate enrollment permissions.
* Use dedicated certificate templates.
* Monitor issued certificates.
* Remove unnecessary enrollment permissions.
* Renew certificates before expiration.
* Maintain proper certificate lifecycle management.

For a production environment, a multi-tier PKI architecture with an offline Root CA and subordinate issuing CA would generally provide stronger separation and protection.

The current lab intentionally uses a single Enterprise Root CA for simplicity and learning purposes.

---

## Integration with Enterprise Services

The ManoTech PKI integrates with other infrastructure services:

```text id="u2e7mc"
                 DC01
                  │
        ┌─────────┴─────────┐
        │                   │
        ▼                   ▼
 Active Directory       ManoTech-ROOT-CA
        │                   │
        │            ┌──────┴──────┐
        │            │             │
        ▼            ▼             ▼
   Domain Users   WEB01        Computers
                     │
                     ▼
                    IIS
                     │
                     ▼
            www.manotech.local
```

This demonstrates how Active Directory Certificate Services can provide a centralized certificate infrastructure for enterprise systems.

---

## Notes

AD CS provides the foundation for the ManoTech enterprise PKI.

The current implementation uses an Enterprise Root CA hosted on DC01 and integrated with Active Directory.

The PKI is used to demonstrate certificate enrollment, certificate management, and internal HTTPS certificate deployment.

The final certificate auto-enrollment configuration will be aligned with the finalized Active Directory OU and Group Policy structure.

---

## Revision History

| Version | Date       | Author        | Description                                                                                                     |
| ------- | ---------- | ------------- | --------------------------------------------------------------------------------------------------------------- |
| 1.0     | 2026-08-04 | Mohamed Osama | Initial AD CS documentation                                                                                     |
| 1.1     | 2026-08-09 | Mohamed Osama | Updated CA architecture, server naming, certificate templates, IIS integration, validation, and troubleshooting |
