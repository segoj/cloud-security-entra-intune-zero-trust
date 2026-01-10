# Microsoft Entra ID & Intune Zero Trust Labs (Day 1–Day 5)

This document captures a hands-on Zero Trust implementation using Microsoft Entra ID, Conditional Access, Multi-Factor Authentication (MFA), and Microsoft Intune. The focus is on identity-driven access control and device trust for modern cloud-first environments.

---

## Day 1 – Identity & Tenant Baseline

### Objective
Establish a secure Microsoft Entra ID tenant baseline with administrative resilience and audit visibility.

### Configuration
- Created a Microsoft Entra ID tenant for lab use
- Verified default identity configuration and directory health
- Created a dedicated break-glass Global Administrator account
- Confirmed availability of Audit logs and Sign-in logs in Entra ID

### Security Rationale
- A break-glass account ensures tenant recovery in case of Conditional Access misconfiguration
- Audit and sign-in logs provide immutable identity activity tracking
- Establishing a clean identity baseline is required before Zero Trust enforcement

### Validation
- Global Administrator role successfully assigned
- Audit logs accessible under Entra ID → Users → Audit logs
- Tenant functional without dependency on Microsoft Security Defaults

---

## Day 2 – Privileged Access Protection (Admin MFA)

### Objective
Protect privileged roles by enforcing Multi-Factor Authentication using Conditional Access.

### Configuration
- Disabled Microsoft Security Defaults to allow granular policy design
- Created Conditional Access policy requiring MFA for all administrator roles
- Scoped policy to all cloud resources
- Explicitly excluded break-glass administrator account

### Security Rationale
- Security Defaults limit enterprise-grade Conditional Access customization
- Admin MFA is the most critical identity security control
- Break-glass exclusion prevents tenant lockout during misconfiguration

### Validation
- Administrative sign-in flows triggered MFA
- Admin access without MFA was not permitted
- Break-glass account remained unaffected and usable

---

## Day 3 – Standard User MFA Enforcement

### Objective
Extend MFA enforcement to standard users using group-based Conditional Access.

### Configuration
- Created a security group for standard users
- Created Conditional Access policy enforcing MFA
- Scoped policy to all cloud resources
- Excluded break-glass administrator account

### Security Rationale
- Group-based targeting enables scalable policy management
- MFA significantly reduces credential-based attacks
- Aligns with Microsoft Zero Trust identity pillar

### Validation
- Standard user sign-ins required MFA
- Authentication succeeded only after MFA completion
- No Conditional Access block errors observed

---

## Day 4 – Device Trust Foundation (Entra ID + Intune)

### Objective
Establish device trust by joining a Windows device to Entra ID and enrolling it into Intune.

### Configuration
- Built a Windows 11 virtual machine using Hyper-V
- Joined the device to Microsoft Entra ID
- Enrolled the device into Microsoft Intune
- Verified device presence in Entra ID and Intune portals

### Security Rationale
- Device trust is a core Zero Trust signal
- Entra ID join establishes cloud-native device identity
- Intune enrollment enables posture and compliance evaluation

### Validation
- `dsregcmd /status` confirmed AzureAdJoined = YES
- Device visible in Entra ID → Devices
- Device visible in Intune → Devices → All devices

---

## Day 5 – Device Compliance & Zero Trust Enforcement

### Objective
Enforce Zero Trust access by combining identity, MFA, and device compliance.

### Configuration
- Created Windows 10/11 compliance policy in Microsoft Intune
- Required:
  - Firewall enabled
  - Antivirus present
  - Antispyware present
- Excluded:
  - TPM
  - Secure Boot
  - BitLocker (due to VM limitations)
- Assigned compliance policy to devices
- Created Conditional Access policy requiring:
  - MFA
  - Compliant device
- Scoped policy to standard users and excluded break-glass admin

### Security Rationale
- Device posture must be evaluated before access is granted
- VM-aware compliance avoids false noncompliance
- Conditional Access enforces real Zero Trust decisions at sign-in

### Validation
- Device marked **Compliant** in Intune
- Authentication flows progressed without Conditional Access block messages
- MFA and device compliance checks triggered as expected
- Microsoft 365 portal UI instability identified as a known VM limitation

---

## Overall Zero Trust Outcome

- Identity protected using MFA
- Privileged access hardened
- Device trust established and evaluated
- Access granted only after identity and device signals were verified
- Break-glass recovery maintained throughout

This lab demonstrates a practical Zero Trust implementation aligned with real-world enterprise security practices.
