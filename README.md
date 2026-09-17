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
