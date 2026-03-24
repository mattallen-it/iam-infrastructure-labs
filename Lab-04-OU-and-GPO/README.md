# IAM Lab 04 – Organizational Units (OU) Design and Group Policy

![Type](https://img.shields.io/badge/type-lab-blue)
![Tech](https://img.shields.io/badge/technology-Active%20Directory-blue)
![Platform](https://img.shields.io/badge/platform-Windows%20Server%202022-blue)
![Focus](https://img.shields.io/badge/focus-IAM-green)
![Domain](https://img.shields.io/badge/domain-Identity%20Management-green)
![Level](https://img.shields.io/badge/level-Foundation-lightgrey)

---

## Overview

This lab builds on the identity foundation by introducing **organizational structure and policy enforcement** within the MRTG Active Directory environment.

An enterprise-style OU hierarchy was implemented to support:

- Role-based access control (RBAC)  
- Policy targeting  
- Administrative delegation  

A baseline Group Policy Object (GPO) was created and applied to domain-joined workstations.

---

## Objective

- Design scalable OU structure  
- Organize identity objects (users, devices, accounts)  
- Create and link a workstation GPO  
- Configure password and lockout policies  
- Validate GPO application  

---

## Scope

### Included
- Organizational Unit (OU) design and implementation
- User, computer, and account organization
- Workstation OU creation and device placement
- Group Policy Object (GPO) creation and linking
- Password policy configuration
- Account lockout policy configuration
- GPO validation using gpupdate and gpresult

### Not Included
- User lifecycle management (Lab 05)
- Group-based access assignment (Lab 05)
- Delegation of control (future lab)
- SIEM/logging integration (future lab)

---

## Environment

| Component | Value |
|----------|------|
| Domain | mrtg.local |
| Domain Controller | MRTG-DC01 |
| Client Machine | CLIENT01 |
| Virtualization | Hyper-V |
| Network | MRTG-Internal |

---

## Architecture

mrtg.local
└── _MRTG
├── Users
│ ├── IT
│ ├── Security
│ ├── HR
│ ├── Finance
│ ├── Operations
│ ├── Engineering
│ └── Executives
├── Computers
│ ├── Workstations
│ └── Servers
├── Groups
├── Admin Accounts
└── Service Accounts


---

## Implementation

### 1. OU Structure Creation

Established `_MRTG` as the root organizational container and built core identity structure.

![OU Structure](images/lab04_01_base_ou_structure.png)
![Core Layout](images/lab04_03_core_ou_layout.png)

---

### 2. Departmental User OUs

Organized users by business function to support RBAC and policy targeting.

![Departments](images/lab04_04_user_department_ous.png)

---

### 3. Computer OU Structure

Separated endpoints into Workstations and Servers for targeted policy enforcement.

![Computer OUs](images/lab04_05_computer_ous.png)

---

### 4. Identity Object Creation

Provisioned standard users, admin accounts, and service accounts.

![Users](images/lab04_06_user_creation.png)
![Admins](images/lab04_07_admin_accounts.png)
![Service Accounts](images/lab04_08_service_accounts.png)

---

### 5. Domain Join (CLIENT01)

Configured DNS and successfully joined CLIENT01 to mrtg.local.

![Domain Join](images/lab04_09_domain_join.png)
![DNS Validation](images/lab04_10_dns_validation.png)
![Client Joined](images/lab04_11_client_joined_domain.png)

---

### 6. Device Placement

Moved CLIENT01 from default container to Workstations OU.

![Before](images/lab04_12_client_default_container.png)
![After](images/lab04_13_client_workstations_ou.png)

---

### 7. GPO Creation and Linking

Created and linked:
MRTG-Workstation-Baseline


![GPO Linked](images/lab04_14_gpo_linked.png)

---

### 8. Password Policy Configuration

Configured baseline identity security controls:

- Minimum length: 8  
- Complexity: Enabled  
- Maximum age: 90 days  
- Password history: 5  

![Password Policy](images/lab04_15_password_policy.png)

---

### 9. Account Lockout Policy Configuration

Configured protection against brute-force authentication attempts:

- Threshold: 5 attempts  
- Duration: 15 minutes  
- Reset counter: 15 minutes  

![Lockout Policy](images/lab04_16_lockout_policy.png)

---

### 10. GPO Validation

Validated policy application on CLIENT01:

```powershell
gpupdate /force
gpresult /r /scope computer
