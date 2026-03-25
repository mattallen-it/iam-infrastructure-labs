# Lab-02 — Active Directory Domain Services (AD DS) Deployment

## Overview

This lab focuses on deploying Active Directory Domain Services (AD DS) within the MRTG environment by promoting the Windows Server virtual machine to a Domain Controller.

This establishes centralized identity management, enabling authentication, authorization, and directory-based access control across the environment.

---

## Why This Matters

Active Directory serves as the core identity provider in most enterprise and government environments.

Proper deployment of AD DS enables:

- Centralized authentication using Kerberos  
- Structured identity management through directory services  
- Secure access control across systems and resources  
- Foundation for Group Policy, RBAC, and auditing  

This lab establishes the identity authority that all future access control and policy enforcement will depend on.

---

## Environment

| Component           | Value                  |
|--------------------|-----------------------|
| Domain Name        | mrtg.local            |
| Domain Controller  | MRTG-DC01             |
| OS                 | Windows Server 2022   |
| Role               | Active Directory Domain Services (AD DS) |
| Virtualization     | Hyper-V               |

---

## Architecture

- **Domain Controller:** MRTG-DC01  
  - Active Directory Domain Services (AD DS)  
  - DNS Server  

- **Directory Structure:**  
  - New forest: `mrtg.local`  

- **Authentication Model:**  
  - Kerberos-based authentication  

This deployment establishes the centralized identity authority for the MRTG environment.

---

## Security Considerations

- Domain Controller deployed in isolated virtual network  
- AD DS installed using least privilege administrative access  
- DNS integrated with Active Directory for secure name resolution  
- Environment prepared for future policy enforcement and auditing  

---

## Lab Steps and Evidence

### 1. Installed Active Directory Domain Services Role

The AD DS server role was installed on MRTG-DC01 to enable domain services functionality.

![AD DS Installed](images/01_adds_role_installed.png)

---

### 2. Promoted Server to Domain Controller

The server was promoted to a Domain Controller and configured as a new forest.

- Root domain: `mrtg.local`

![Domain Promotion](images/02_domain_controller_promotion.png)

---

### 3. Configured Directory Services Restore Mode (DSRM)

A DSRM password was configured to support recovery operations.

![DSRM Config](images/03_dsrm_configuration.png)

---

### 4. Completed Domain Controller Deployment

The system was successfully promoted and restarted as a Domain Controller.

![DC Complete](images/04_dc_promotion_complete.png)

---

### 5. Verified Active Directory Domain

The `mrtg.local` domain was verified within Active Directory Users and Computers.

![AD Domain Verified](images/05_domain_verified.png)

---

### 6. Verified DNS Configuration

DNS functionality was validated to ensure proper name resolution within the domain.

![DNS Verified](images/06_dns_configuration.png)

---

## Outcome

A fully functional Active Directory Domain Services environment was successfully deployed.

- Domain `mrtg.local` created  
- MRTG-DC01 configured as Domain Controller  
- DNS integrated with Active Directory  
- Centralized identity management established  

This environment now supports user, group, and policy-based identity operations.

---

## Next Lab

[Lab-03 — Active Directory Domain Setup (Identity Authority Foundation)](../Lab-03-AD-Setup/README.md)

The next lab will cover:

- Organizational Unit (OU) structure design  
- User and group provisioning  
- Security group implementation (RBAC foundation)  
- Identity organization aligned to business roles  
