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

