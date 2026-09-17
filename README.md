# Enterprise Active Directory & Identity Management Home Lab

## Project Overview
The objective of this project is to architect, deploy, and configure a fully functional, isolated enterprise network environment using VMware Workstation Pro. This lab simulates a corporate domain infrastructure to gain hands-on experience with Identity and Access Management (IAM), User/Group Lifecycle Management, Group Policy Objects (GPOs), network services (DNS/DHCP), and helpdesk workflows.

---

## Phase 1: Virtual Network & Hypervisor Topology

To ensure a safe, self-contained testing environment that mirrors an isolated corporate segment, the hypervisor network topology was configured to block external traffic while allowing communication between internal nodes.

### 1. Network Topology Specifications
- **Hypervisor:** VMware Workstation Pro
- **Subnet Range:** 192.168.100.0/24 (Private Lab Subnet)
- **Domain Controller (Corp-DC01) Gateway IP:** 192.168.100.1
- **Domain Controller (Corp-DC01) Static IP:** 192.168.100.10
- **Workstation Client (Corp-Client01) Dynamic IP:** Assigned via DHCP

### 2. Hypervisor Configuration Steps
1. Launched the **VMware Virtual Network Editor** with Administrator privileges.
2. Created a dedicated host-only network designation (**VMnet1**) to completely isolate the lab traffic from the local host's physical network and internet interface.
3. Disabled the native VMware **DHCP service** on VMnet1. This design constraint ensures that the upcoming Windows Server 2016 instance will act as the sole authoritative DHCP server for the domain, mimicking production enterprise standards.

### 3. Compute Resource Allocation (Domain Controller)
The base virtual machine representing the enterprise core was provisioned with the following systems specifications:
- **Operating System:** Windows Server 2016 Standard (Desktop Experience)
- **Hostname:** Corp-DC01
- **vCPU Allocation:** 2 Cores
- **vRAM Allocation:** 4 GB (4096 MB)
- **Storage Target:** 60 GB Virtual Disk (Stored as a single file)

---

## Phase 2: Core Operating System Deployment

The baseline enterprise operating system framework has been successfully initialized and deployed to storage.

### 1. Installation Timeline & System Parameters
1. Provisioned **Windows Server 2016 Standard (Desktop Experience)** via virtual media mapping.
2. Configured the storage controller array using a single partition format on the **60.0 GB virtual disk asset**.
3. Initialized the localized Windows installation architecture, expanding systems files and applying baseline feature updates.

---

## Phase 3: Post-Installation & Hypervisor Guest Integration

The core operating system deployment has concluded, and guest-to-host integration drivers have been applied to optimize system performance.

### 1. Administrative Infrastructure Deployment
1. Initialized localized access security parameters via configuration of the root `Administrator` account identity credentials.
2. Initialized the system's primary user space interface, accessing the **Windows Server Manager** console platform environment.
3. Deployed **VMware Tools** software integration drivers into the server instance. This configuration step applies specialized video, storage, and I/O bus controller drivers to the virtual guest OS, enabling native display scaling, seamless pointer transitions, and optimized storage performance.

### 2. Next Milestones
- Assign a static IP framework matching architectural network specs (192.168.100.10).
- Alter the system identity from a generic host name string to `Corp-DC01`.

### 2. Current System State
- **OS Environment:** Windows Server 2016 Standard Evaluation (GUI Enabled)
- **Deployment Status:** In Progress / Finalizing automated reboots.
- **Next Scheduled Action:** Local administrative security provisioning and static IP schema routing.

---

## Phase 4: Static IP Provisioning & Host Identity Mapping

Before promoting the instance to an authoritative directory controller, network interfaces and host identities were systematically standardized.

### 1. Network Interface Configuration (TCP/IPv4)
The primary network adapter (`Ethernet0`) was bound to a static architectural schema to prevent IP address drifting and guarantee name resolution reliability:
- **Assigned IP Address:** `192.168.100.10`
- **Subnet Mask:** `255.255.255.0`
- **Default Gateway:** `192.168.100.1`
- **Preferred DNS Server:** `127.0.0.1` (Local loopback designation pointing to upcoming integrated DNS role)

### 2. System Identity Renaming
The localized netBIOS hostname was altered to reflect its structural role in the enterprise hierarchy:
- **Legacy Hostname:** Auto-Generated String (e.g., `WIN-XXXXXX`)
- **New Standard Hostname:** `Corp-DC01`
- **Deployment State:** Reboot initiated to finalize host kernel updates.
- ### 3. Visual Verification Verification
<img width="1919" height="1029" alt="image" src="https://github.com/user-attachments/assets/49d09961-0830-43ed-8507-a068b0026367" />

---

## Phase 5: Active Directory Domain Services (AD DS) Role Deployment

The core directory services framework has been committed and installed to the system storage array.

### 1. Role Provisioning Parameters
1. Utilized the Server Manager platform deployment engine to initialize **Active Directory Domain Services (AD DS)**.
2. Bound essential identity dependencies including remote server administration tools (RSAT) and directory management snap-ins to the kernel framework.

### 2. Visual Verification
<img width="1919" height="1032" alt="image" src="https://github.com/user-attachments/assets/9c1db3f1-b2eb-4692-98fa-be047a556bd0" />


### 3. Next Milestone
- Promote the standalone `Corp-DC01` server platform to a Root Domain Controller hosting a brand new forest infrastructure.

  ---

## Phase 6: Domain Controller Promotion & Forest Creation

The standalone server platform has been successfully promoted to the authoritative Root Domain Controller for the enterprise forest landscape.

### 1. Troubleshooting Case Study: Password Complexity Constraint
- **Issue Encountered:** The initial prerequisite check failed with a validation error regarding local Administrator account password requirements.
- **Root Cause Analysis:** Active Directory password policy parameters demand structural complexity (uppercase, lowercase, numerals, and special characters) before local credentials can be migrated to a global domain administrative context.
- **Remediation Action:** Launched the local security management console (`lusrmgr.msc`), bypassed the credential hurdle by forcing a reset to a hardened credential pattern (`EnterpriseSupport2026!`), and successfully passed the validation re-scan.

### 2. Forest Configuration Parameters
1. Established a clean directory forest layout under the functional root domain boundary: **`corp.local`**.
2. Automated the deployment and integration of the authoritative **Domain Name System (DNS)** server roles to control local zone lookups.

### 3. Visual Verification
<img width="1919" height="1030" alt="image" src="https://github.com/user-attachments/assets/9d4f84c9-f6e7-4a47-803a-83e58784607c" />


### 4. Next Milestone

---

## Phase 7: Organizational Unit (OU) Tree Architecture & Directory Structuring

To implement scalable Role-Based Access Control (RBAC) across the enterprise environment, a clean, structured Organizational Unit (OU) schema was built.

### 1. Structural Design Matrix
Moving away from default flat containers, the directory database tree was deliberately segmented to provide localized Group Policy targets and distinct user boundary definitions:
- **Root OU:** `Corporate_HQ` (Primary headquarters organizational boundary)
  - `Groups` (Container object dedicated to hosting universal and global security access groups)
  - `IT_Department` (Container object dedicated to systems administrators, networks teams, and helpdesk support identities)
  - `HR_Department` (Container object dedicated to human resources staff identities)
  - `Accounting` (Container object dedicated to financial and bookkeeping roles)

### 2. Visual Verification
<img width="1919" height="1030" alt="image" src="https://github.com/user-attachments/assets/0ca1d9b7-c0fd-4a0e-81b3-f6125d118e96" />


### 3. Next Milestone
- Deploy an automated PowerShell provisioning script to inject bulk user identities into their respective department OUs.

- Verify domain identity via the new secure network login prompt (`CORP\Administrator`).
- Implement the Organizational Unit (OU) department layout and begin bulk provisioning user profiles.


---

## Phase 8: Directory Provisioning & User Lifecycle Automation via PowerShell

To simulate large-scale enterprise administration and user lifecycle workflows, a targeted PowerShell script was deployed to inject baseline corporate identities into their respective department OUs.

### 1. Automation Execution Parameters
- **Scripting Environment:** Windows PowerShell Core (Executed as Domain Administrator)
- **Database Targets:** Distinct organizational units (`OU=IT_Department`, `OU=HR_Department`, `OU=Accounting`)
- **Security Profile Baseline:** Accounts were systematically generated with explicit User Principal Names (UPNs), initialized with complex seed credentials, and flagged with an administrative constraint forcing an immediate password change upon initial workstation authentication (`-ChangePasswordAtLogon $true`).

### 2. Verified Provisioning Script Code
```powershell
\$Pass = ConvertTo-SecureString "EnterpriseSupport2026!" -AsPlainText -Force

New-ADUser -Name "Alice.Support" -SamAccountName "Alice.Support" -UserPrincipalName "Alice.Support@corp.local" -Path "OU=IT_Department,OU=Corporate_HQ,DC=corp,DC=local" -AccountPassword \$Pass -ChangePasswordAtLogon true -Enabled true

New-ADUser -Name "Bob.Admin" -SamAccountName "Bob.Admin" -UserPrincipalName "Bob.Admin@corp.local" -Path "OU=IT_Department,OU=Corporate_HQ,DC=corp,DC=local" -AccountPassword \$Pass -ChangePasswordAtLogon true -Enabled true

New-ADUser -Name "Emma.HR" -SamAccountName "Emma.HR" -UserPrincipalName "Emma.HR@corp.local" -Path "OU=HR_Department,OU=Corporate_HQ,DC=corp,DC=local" -AccountPassword \$Pass -ChangePasswordAtLogon true -Enabled true

New-ADUser -Name "Sarah.Finance" -SamAccountName "Sarah.Finance" -UserPrincipalName "Sarah.Finance@corp.local" -Path "OU=Accounting,OU=Corporate_HQ,DC=corp,DC=local" -AccountPassword \$Pass -ChangePasswordAtLogon true -Enabled true
```

### 3. Visual Verification
<img width="1919" height="1029" alt="image" src="https://github.com/user-attachments/assets/8be31348-a965-412e-9fd0-66fb6f2c07ad" />


### 4. Next Milestone
- Engineer a Group Policy Object (GPO) baseline to deploy localized workstation restrictions across the domain framework.

---

## Phase 9: Group Policy Object (GPO) Baseline Enforcement

To secure domain assets and prevent unauthorized configuration changes by non-administrative users, an enterprise-wide system restriction policy was engineered.

### 1. Security Baseline Parameters
- **Policy Object Title:** `GPO_Restrict_Control_Panel`
- **Target Boundary:** Inherited at the root Domain layer (`corp.local`) affecting all standard authenticated user objects.
- **Enforced Constraint:** Enabled administrative template restriction rule `Prohibit access to Control Panel and PC settings`. This structural rule hardens workstations by blocking access to structural operating system modification applets (`control.exe`), mitigating the risk of unauthorized local system modifications.

### 2. Visual Verification
<img width="1919" height="1029" alt="image" src="https://github.com/user-attachments/assets/8175a910-da95-4583-b769-28095a0477c7" />


---

## Phase 10: Client Workstation Deployment & Hypervisor Network Alignment

To test group policy inheritance and directory authentication loops, a dedicated workstation asset was introduced into the virtualized environment.

### 1. Workstation Hardware Profile
- **Operating System:** Windows 10 Pro (Consumer Evaluation Media)
- **Hostname Target:** Corp-Client01
- **vRAM Allocation:** 2 GB / vCPU Allocation: 2 Cores
- **Hypervisor Network Interconnect:** Hard-bound to custom virtual switch layer **`VMnet1 (Host-only)`**. This synchronization locks the workstation into the exact same isolated broadcast domain as the `Corp-DC01` domain controller, setting up local data transit paths.

### 2. Visual Verification
<img width="1919" height="1029" alt="image" src="https://github.com/user-attachments/assets/6afaedbb-aaba-4900-9cef-6960b0bb2093" />


### 3. Next Milestone
- Configure network adapter settings on the workstation and execute a formal domain join sequence using automated employee accounts.



---

## Phase 11: Enterprise Domain Integration & Boundary Verification

The client workstation asset has been successfully bound to the secure network zone and integrated into the active directory structure.

### 1. Troubleshooting Case Study: Transmit Failed & Local Interface Disconnection
- **Issue Encountered:** Validation handshakes yielded an infrastructure failure state, and command-line tracing (`ping`) threw a severe network protocol error: `PING: transmit failed. General failure.`
- **Root Cause Analysis:** Because the native VMware hypervisor DHCP mechanism was systematically decommissioned in Phase 1 to allow the server to act as the authoritative address pool later, the Windows 10 operating system lacked an IP signature entirely, disabling its local network adapter.
- **Remediation Action:** Configured temporary explicit static routing fields directly on the workstation client node interface (`IP: 192.168.100.20`, `DNS: 192.168.100.10`), immediately establishing flat data transit paths across the `VMnet1` broadcast lane.

### 2. Verification Specifications
- **Interface Target:** Automated endpoint `Corp-Client01`
- **DNS Interconnect Routing:** Configured network adapter properties to route directory namespace lookup requests exclusively through target interface `192.168.100.10`.
- **Authentication Handshake:** Processed a secure structural handshake using global domain administrator credentials, verifying complete domain alignment across the isolated `corp.local` tree.

### 3. Visual Verification
<img width="1919" height="1031" alt="image" src="https://github.com/user-attachments/assets/d0d03892-3b26-4677-bed1-943b7f770209" />


### 4. Final Milestone
- Authenticate into the newly joined environment using an automated employee identity profile (`CORP\Alice.Support`) and verify security rule inheritance.

  ---

## Phase 12: End-User Authentication & GPO Enforcement Verification

The enterprise deployment cycle has concluded with formal system verification, confirming identity management validity and security policy compliance.

### 1. Verification Testing Procedures
1. **Domain User Authentication:** Initialized a new workstation user context utilizing the automated employee profile identity **`CORP\Alice.Support`**. Verified initial credential modification hooks on first network logon.
2. **Security Policy Audit:** Attempted execution of administrative operating system modification tools (`control.exe`). The kernel intercepted the transit command and dropped execution, successfully throwing a domain restrictions alert banner.

### 2. Visual Verification
<img width="1919" height="1027" alt="image" src="https://github.com/user-attachments/assets/7406912a-e559-4779-8837-5687eed72107" />


## Project Conclusion & Core Competencies Demonstrated
This home lab successfully replicates an isolated enterprise infrastructure environment. By engineering this framework from the ground up, the following Helpdesk Tier 1 / Junior Systems Administrator technical skill sets were validated:
- **Hypervisor Networking:** Isolated broadcast domain configuration, manual interface routing table alignment, and TCP/IPv4 diagnostic logging.
- **Identity & Access Management (IAM):** Core Active Directory Domain Services deployment, nested Organizational Unit provisioning, and bulk data automation scripting via Windows PowerShell.
- **Systems Hardening:** Centralized security auditing via Group Policy Object (GPO) administrative templates.

