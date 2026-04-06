# Lab-08: Identity Monitoring and Auditing

![Platform](https://img.shields.io/badge/Platform-Windows%20Server%202022-blue)
![Directory](https://img.shields.io/badge/Directory-Active%20Directory-darkgreen)
![Focus](https://img.shields.io/badge/Focus-Identity%20Monitoring%20%26%20Auditing-purple)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

## Overview

In this lab, I implemented identity-focused auditing in the **Monroe Redstone Technology Group (MRTG)** Active Directory environment. The objective was to improve visibility into administrative identity activity by configuring domain controller audit policy, applying object-level auditing to the `_MRTG/Users` OU, and validating that delegated administrative actions generated the expected security events.

This lab builds directly on the delegated administration model established in **Lab 07**. Instead of relying on broad administrative access for user-management tasks, I used the delegated admin account **`john.smith.admin`** to perform controlled identity actions and then verified those actions through the domain controller's **Security** log.

---

## Objectives

- Create a dedicated auditing GPO for domain controllers
- Configure advanced audit policy for:
  - Logon events
  - User account management
  - Directory service changes
  - Security group management
- Verify policy application with `gpupdate` and `auditpol`
- Apply object-level auditing to the `_MRTG/Users` OU
- Generate auditable delegated identity actions
- Validate resulting events in **Event Viewer**

---

## Scope

### Included
- Domain controller advanced audit policy configuration
- Object-level auditing on the `_MRTG/Users` OU
- Delegated password reset activity
- Delegated user attribute modification
- Security log validation in Event Viewer

### Not Included
- SIEM integration
- Windows Event Forwarding
- Microsoft Defender for Identity
- Centralized alerting workflows
- Long-term retention architecture

This lab stays focused on **native Windows and Active Directory auditing**.

---

## Lab Environment

### Systems
- **Domain Controller:** `MRTG-DC01`
- **Domain:** `mrtg.local`

### Active Directory Structure
- **Parent OU:** `_MRTG`
- **User OU:** `_MRTG/Users`
- **Admin Accounts OU:** `_MRTG/Admin Accounts`

### Key Accounts and Groups
- **Delegated admin account:** `john.smith.admin`
- **Delegated admin group:** `GG_IT_HelpDesk_Admins`
- **Validation target user:** `kevin.carter`

### GPOs Used
- **Auditing GPO:** `MRTG-DC-Identity-Auditing`

### Validation Note
A temporary logon-validation configuration was used during lab preparation so the delegated admin account could sign in and perform the scoped identity actions required for testing. The core focus of this lab remained the auditing configuration and resulting event visibility.

### Assumptions
- The delegated administration model from **Lab 07** was already in place
- `john.smith.admin` had validated delegated access to manage users in `_MRTG/Users`
- The environment was running in Hyper-V with checkpoint support

---

## Architecture

This lab used a single-domain Active Directory environment with:

- audit policy configured at the **Domain Controllers** OU level
- object-level auditing applied to the `_MRTG/Users` OU
- delegated user-management activity performed by **`john.smith.admin`**
- event validation performed on **MRTG-DC01**

### High-Level Flow

1. Create and link a dedicated domain controller auditing GPO
2. Configure identity-related advanced audit policy settings
3. Apply object-level auditing to `_MRTG/Users`
4. Perform delegated identity actions
5. Validate resulting security events in Event Viewer

---

## Technologies Used

- Windows Server 2022
- Active Directory Users and Computers (ADUC)
- Group Policy Management Console (GPMC)
- Event Viewer
- `auditpol`
- Hyper-V

---

## Implementation Steps

### 1. Created a Pre-Lab Checkpoint

Before making changes, I created a Hyper-V checkpoint to preserve the clean post-Lab 07 environment.

![Lab-08-01 - Pre-Auditing Checkpoint](images/Lab-08-01-Pre-Auditing-Checkpoint.png)

---

### 2. Confirmed Delegated Administration Baseline

I verified that **`john.smith.admin`** remained a member of **`GG_IT_HelpDesk_Admins`** and confirmed the `_MRTG/Users` OU structure was present. This ensured the lab started from the intended delegated-access baseline rather than broad administrative access.

![Lab-08-02 - Delegated Admin Baseline](images/Lab-08-02-Delegated-Admin-Baseline.png)

---

### 3. Created and Linked the Auditing GPO

I created a dedicated GPO named **`MRTG-DC-Identity-Auditing`** and linked it to the **Domain Controllers** OU. I used a dedicated GPO rather than modifying the default policy so the change would remain easier to manage, explain, and validate.

![Lab-08-03 - Auditing GPO Created](images/Lab-08-03-Auditing-GPO-Created.png)

---

### 4. Configured Audit Logon

I enabled **Audit Logon** for both **Success** and **Failure** events. This provides visibility into successful and failed authentication activity on the domain controller.

![Lab-08-04 - Audit Logon Configured](images/Lab-08-04-Audit-Logon-Configured.png)

---

### 5. Configured Audit User Account Management

I enabled **Audit User Account Management** for both **Success** and **Failure** events. This supports monitoring of actions such as password resets and account changes.

![Lab-08-05 - Audit User Account Management Configured](images/Lab-08-05-Audit-User-Account-Management-Configured.png)

---

### 6. Configured Audit Directory Service Changes

I enabled **Audit Directory Service Changes** for **Success** events. This supports tracking of Active Directory object modifications when paired with the appropriate object-level auditing entry.

![Lab-08-06 - Audit Directory Service Changes Configured](images/Lab-08-06-Audit-Directory-Service-Changes-Configured.png)

---

### 7. Configured Audit Security Group Management

I enabled **Audit Security Group Management** for both **Success** and **Failure** events to improve visibility into changes affecting access-control groups.

![Lab-08-07 - Audit Security Group Management Configured](images/Lab-08-07-Audit-Security-Group-Management-Configured.png)

---

### 8. Applied and Verified Audit Policy

I applied the updated policy and verified the resulting configuration with:

```cmd
gpupdate /force
auditpol /get /category:*

---

The output confirmed that the required audit subcategories were enabled, including:

• Logon
• Security Group Management
• User Account Management
• Directory Service Changes

This validated that the domain controller was prepared to capture the identity-related events required for the lab.

---

10. Added an Auditing Entry for Descendant User Objects

I created a new auditing entry with the following settings:

• Principal: Everyone
• Type: Success
• Applies to: Descendant User objects

I scoped the entry toward change activity rather than passive read activity. This step was critical because enabling directory service change auditing alone is not enough. The relevant OU or object must also have an auditing entry configured to generate object-modification events.

---

11. Performed a Delegated Password Reset

Using the delegated admin account john.smith.admin, I reset the password for Kevin Carter and selected User must change password at next logon.

This validated that delegated administrative activity was being performed through the intended scoped identity rather than a broad domain-wide administrative account.

---

12. Performed a Delegated User Attribute Change

Still using john.smith.admin, I modified the Description field for Kevin Carter to:

Lab 08 audit validation change

This provided a second controlled identity action for audit validation and helped confirm that both account-management and object-modification events were being logged as expected.

---

Validation / Verification

After the delegated actions were completed, I reviewed the Security log in Event Viewer on MRTG-DC01 and confirmed the expected identity-related events.

• Event ID 4724 - Password reset attempt
Confirmed that a password reset operation occurred for the target account.
• Event ID 4738 - User account changed
Confirmed that the user object was modified.
• Event ID 5136 - Directory service object modified
Confirmed that the Active Directory object change was captured through directory service auditing.
• Event ID 4719 - System audit policy changed
Confirmed that audit policy configuration changes were also logged.

---

Security Relevance

This lab strengthened the environment by making identity activity visible, traceable, and reviewable.

Key security benefits included:

• improved auditability of delegated identity actions
• better visibility into password resets and user-object changes
• stronger change tracking for Active Directory objects
• improved administrative accountability
• reinforcement of least privilege by separating delegated user actions from broader audit-policy administration

This is directly relevant to IAM operations, directory administration, helpdesk delegation, and regulated IT environments where identity changes must be monitored and validated.

---

Key Skills Demonstrated
• Group Policy Management
• Advanced Audit Policy Configuration
• Active Directory Users and Computers
• Object-level auditing in Active Directory
• Delegated administration validation
• Password reset workflows
• User attribute administration
• Event Viewer log analysis
• Security event validation
• Identity monitoring fundamentals

---

Outcome

By the end of this lab, I had successfully:

• deployed a dedicated identity-auditing GPO to domain controllers
• verified advanced audit policy settings on the domain controller
• applied object-level auditing to the _MRTG/Users OU
• performed delegated identity actions using john.smith.admin
• confirmed those actions generated the expected security events in Event Viewer

This lab moved the MRTG environment beyond basic user administration and into auditable identity operations, which is a core control in enterprise IAM and security-focused IT environments.
