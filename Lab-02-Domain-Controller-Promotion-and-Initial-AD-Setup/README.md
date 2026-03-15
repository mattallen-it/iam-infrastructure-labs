# Lab-02 - Domain Controller Promotion and Initial Active Directory Setup

## Objective
Install Windows Server 2022 in Hyper-V, configure the server baseline, and promote the system to a Domain Controller hosting the initial Active Directory environment.

## Scope

### Included
- Windows Server 2022 installation in Hyper-V
- Initial server login and baseline configuration
- Hostname change to DC01
- Static IPv4 configuration
- Hyper-V checkpoint creation
- Active Directory Domain Services installation
- Domain Controller promotion
- DNS validation
- Logon server verification

### Not Included
- Organizational Unit design
- User and group provisioning
- Client domain join
- Group Policy creation

## Environment
- Host OS: Windows 11 Pro
- Hypervisor: Hyper-V
- VM Name: DC01
- Guest OS: Windows Server 2022 Standard Evaluation (Desktop Experience)
- VM Generation: Generation 2
- Memory: 4 GB RAM
- Virtual Disk: 80 GB VHDX
- Network: Hyper-V internal switch (`LAB_INTERNAL_NETWORK`)
- Static IP: `192.168.10.10/24`
- Domain Name: `lab.local`

## Architecture

```text
Hyper-V Host
└─ DC01
   ├─ Windows Server 2022
   ├─ Active Directory Domain Services
   └─ DNS

### 1. Launched the Windows Server installer
The virtual machine booted successfully from the Windows Server 2022 ISO, confirming that the Hyper-V boot order and installation media were configured correctly.

![Windows Server Setup Launch](images/lab-02-01-windows-server-setup-launch.png)

### 2. Selected the Windows Server 2022 installation edition
I reviewed the available server installation options before proceeding with the Desktop Experience edition.

![Server 2022 Edition Selection](images/lab-02-02-server-2022-edition-selection.png)

### 3. Selected Windows Server 2022 Standard Evaluation (Desktop Experience)
I selected the Desktop Experience edition instead of Server Core so I could use graphical administration tools during the early IAM lab stages.

![Desktop Experience Selected](images/lab-02-03-desktop-experience-selected.png)

### 4. Selected the virtual disk installation target
I installed the operating system to the 80 GB virtual disk attached to the DC01 virtual machine.

![Install Target Disk Selected](images/lab-02-04-install-target-disk-selected.png)

### 5. Logged into Windows Server and verified Server Manager
After installation completed, I logged into the server and confirmed that Server Manager loaded successfully.

![Server Manager First Login](images/lab-02-05-server-manager-first-login.png)

### 6. Renamed the server to DC01
I renamed the host from its default generated name to DC01 to align with standard infrastructure naming conventions.

![Server Renamed DC01](images/lab-02-06-server-renamed-dc01.png)

### 7. Configured a static IPv4 address
I assigned DC01 a static IP address so that DNS and Active Directory services would have a stable network identity.

![Static IP Configuration](images/lab-02-07-static-ip-configuration.png)

### 8. Verified DC01 baseline configuration in Server Manager
I confirmed the server name and IPv4 address were correctly applied after the baseline configuration changes.

![Server Manager DC01 Configured](images/lab-02-08-server-manager-dc01-configured.png)

### 9. Created a Hyper-V checkpoint before installing Active Directory
I created a pre-AD checkpoint so I could safely roll back if the domain controller promotion failed or required rework.

![Hyper-V Pre-AD Checkpoint](images/lab-02-09-hyperv-pre-ad-checkpoint.png)

### 10. Selected role-based installation
I used the Add Roles and Features Wizard and chose the role-based installation method for DC01.

![Role Based Installation Selected](images/lab-02-10-role-based-installation-selected.png)

### 11. Confirmed DC01 as the destination server
I verified that the AD DS role would be installed on the correct server.

![Destination Server DC01](images/lab-02-11-destination-server-dc01.png)

### 12. Selected Active Directory Domain Services
I selected the AD DS role, which provides the directory service and authentication foundation for enterprise Windows environments.

![AD DS Role Selected](images/lab-02-12-active-directory-domain-services-role.png)

### 13. Installed the AD DS role
The AD DS installation completed successfully and prepared the server for domain controller promotion.

![AD DS Installation Progress](images/lab-02-13-ad-ds-installation-progress.png)

### 14. Started domain controller promotion
After role installation, I launched the post-deployment configuration workflow to promote DC01 to a domain controller.

![Promote Server to Domain Controller](images/lab-02-14-promote-server-to-domain-controller.png)

### 15. Chose to add a new forest
I selected the option to create a new Active Directory forest for the lab environment.

![AD DS New Forest Selection](images/lab-02-15-ad-ds-new-forest-selection.png)

### 16. Configured the lab.local domain
I entered `lab.local` as the root domain name for the initial Active Directory forest.

![Domain Name lab.local](images/lab-02-16-domain-name-lab-local.png)

### 17. Passed the AD DS prerequisites check
I validated the promotion configuration and confirmed that all prerequisite checks passed successfully.

![AD DS Prerequisites Check Passed](images/lab-02-17-ad-ds-prerequisites-check-passed.png)

### 18. Verified DNS forward lookup zones
After promotion, I confirmed that Active Directory-integrated DNS zones were created successfully for the new domain.

![DNS Forward Lookup Zone Created](images/lab-02-18-dns-forward-lookup-zone-created.png)

### 19. Verified the DC01 host record in DNS
I confirmed that the domain controller registered its host record in DNS as `dc01.lab.local`.

![DNS Host Record DC01](images/lab-02-19-dns-host-record-dc01.png)

### 20. Verified the logon server
I ran `echo %logonserver%` to confirm the system was authenticating against `\\DC01`.

![Logon Server Verification](images/lab-02-20-logon-server-verification.png)

### 21. Created a post-promotion checkpoint
After successful domain controller promotion, I created a new baseline checkpoint for future IAM lab work.

![Post-AD Domain Controller Checkpoint](images/lab-02-21-post-ad-domain-controller-checkpoint.png)

## Outcome
At the end of this lab, DC01 was successfully installed as a Windows Server 2022 virtual machine, renamed, assigned a static IP address, and promoted to a Domain Controller for the `lab.local` Active Directory domain. DNS was installed alongside AD DS, and the new domain was validated through DNS records and logon server verification.
