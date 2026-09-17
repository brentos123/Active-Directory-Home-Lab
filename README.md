Active Directory Home Lab
Overview

I built an isolated enterprise network in VMware Workstation Pro to get hands-on practice with the core skills of an IT helpdesk / junior sysadmin role: Active Directory, user and group management, Group Policy, DNS/DHCP, and basic troubleshooting.

The lab simulates a small company domain: one domain controller, one Windows 10 client, a handful of departments, and the policies you'd expect a real IT team to enforce. Below is a phase-by-phase writeup of how I built it, including the problems I hit and how I fixed them.

Contents
Network & Hypervisor Setup
Installing Windows Server
Post-Install Configuration
Static IP & Hostname
Installing Active Directory Domain Services
Promoting the Domain Controller
Organizational Unit Structure
Bulk User Provisioning with PowerShell
Group Policy Enforcement
Client Workstation Setup
Domain Join
Verifying the Setup
Skills Demonstrated
Lab Topology
blocked
VMnet1 - Host-only, isolated
Corp-DC01Windows Server 2016192.168.100.10AD DS / DNS / DHCP
Corp-Client01Windows 10 Pro192.168.100.20
Internet
Physical Host
Component	Role	IP
Corp-DC01	Domain Controller, DNS, DHCP	192.168.100.10
Corp-Client01	Domain-joined workstation	192.168.100.20 (static, see Phase 11)
Subnet	192.168.100.0/24	Isolated host-only network (VMnet1), no internet access
Phase 1: Network & Hypervisor Setup

I wanted a self-contained network that couldn't touch my real LAN or the internet, so I set it up as a host-only network in VMware.

Steps:

Opened the VMware Virtual Network Editor as Administrator.
Created a dedicated host-only network (VMnet1) with no bridge to the physical adapter, isolating lab traffic completely.
Disabled VMware's built-in DHCP service on VMnet1 — I wanted the domain controller to be the only DHCP server on the network, like it would be in a real company.

Domain controller VM specs:

OS: Windows Server 2016 Standard (Desktop Experience)
Hostname: Corp-DC01
2 vCPU / 4 GB vRAM / 60 GB disk
Phase 2: Installing Windows Server

Installed Windows Server 2016 Standard (Desktop Experience) onto the 60 GB virtual disk and let it run through setup and initial updates.

Phase 3: Post-Install Configuration

Once Windows was installed and VMware Tools was added (for proper display scaling and mouse integration), I set the local Administrator password and logged into Server Manager for the first time.

State at end of this phase:

OS: Windows Server 2016 Standard Evaluation (GUI)
Next steps: assign the static IP (192.168.100.10) and rename the host to Corp-DC01
Phase 4: Static IP & Hostname

Before promoting this box to a domain controller, it needed a fixed identity on the network.

Network config (Ethernet0):

IP: 192.168.100.10
Subnet mask: 255.255.255.0
Gateway: 192.168.100.1
Preferred DNS: 127.0.0.1 (pointing at itself, since it will host DNS once AD DS is installed)

Hostname: renamed from the default WIN-XXXXXX string to Corp-DC01, then rebooted to apply.

Verification
<img width="1919" height="1029" alt="image" src="https://github.com/user-attachments/assets/49d09961-0830-43ed-8507-a068b0026367" />
Phase 5: Installing Active Directory Domain Services

Used Server Manager's "Add Roles and Features" wizard to install AD DS, along with the RSAT management tools.

Verification
<img width="1919" height="1032" alt="image" src="https://github.com/user-attachments/assets/9c1db3f1-b2eb-4692-98fa-be047a556bd0" />

Next: promote this server to a domain controller and create a new forest.

Phase 6: Promoting the Domain Controller
Issue: password didn't meet complexity requirements

When I ran the promotion wizard, the prerequisite check failed on the local Administrator password — AD requires a mix of uppercase, lowercase, numbers, and special characters before it'll let you promote the account to a domain admin.

Fix: reset the local password via lusrmgr.msc to something that meets the complexity policy, then re-ran the check and it passed.

Forest setup
Root domain: corp.local
Installed DNS as part of the promotion, so the server handles name resolution for the domain going forward
Verification
<img width="1919" height="1030" alt="image" src="https://github.com/user-attachments/assets/9d4f84c9-f6e7-4a47-803a-83e58784607c" />

Next: build out the OU structure (Phase 7).

Phase 7: Organizational Unit Structure

Rather than dumping every account into the default Users container, I built a proper OU structure so I could target Group Policy and permissions by department — the way a real company would.

Corporate_HQ
├── Groups          (security groups)
├── IT_Department   (sysadmins, network, helpdesk)
├── HR_Department   (HR staff)
└── Accounting      (finance staff)
Verification
<img width="1919" height="1030" alt="image" src="https://github.com/user-attachments/assets/0ca1d9b7-c0fd-4a0e-81b3-f6125d118e96" />

Next: provision users into these OUs with PowerShell, and confirm login works via CORP\Administrator.

Phase 8: Bulk User Provisioning with PowerShell

Instead of clicking through "New User" four separate times, I wrote a short script to provision accounts into their correct OUs — closer to how this would actually be done at scale.

Each account gets a UPN, a temporary password, and is flagged to force a password change at next logon.

powershell
$Pass = ConvertTo-SecureString "<REDACTED>" -AsPlainText -Force

New-ADUser -Name "Alice.Support" -SamAccountName "Alice.Support" -UserPrincipalName "Alice.Support@corp.local" -Path "OU=IT_Department,OU=Corporate_HQ,DC=corp,DC=local" -AccountPassword $Pass -ChangePasswordAtLogon $true -Enabled $true

New-ADUser -Name "Bob.Admin" -SamAccountName "Bob.Admin" -UserPrincipalName "Bob.Admin@corp.local" -Path "OU=IT_Department,OU=Corporate_HQ,DC=corp,DC=local" -AccountPassword $Pass -ChangePasswordAtLogon $true -Enabled $true

New-ADUser -Name "Emma.HR" -SamAccountName "Emma.HR" -UserPrincipalName "Emma.HR@corp.local" -Path "OU=HR_Department,OU=Corporate_HQ,DC=corp,DC=local" -AccountPassword $Pass -ChangePasswordAtLogon $true -Enabled $true

New-ADUser -Name "Sarah.Finance" -SamAccountName "Sarah.Finance" -UserPrincipalName "Sarah.Finance@corp.local" -Path "OU=Accounting,OU=Corporate_HQ,DC=corp,DC=local" -AccountPassword $Pass -ChangePasswordAtLogon $true -Enabled $true

(Password redacted here — swap in your own before running this. In PowerShell, boolean parameters need the $ prefix, e.g. $true, not true.)

Verification
<img width="1919" height="1029" alt="image" src="https://github.com/user-attachments/assets/8be31348-a965-412e-9fd0-66fb6f2c07ad" />

Next: lock down workstations with a Group Policy baseline.

Phase 9: Group Policy Enforcement

Created a GPO to stop standard users from making system-level changes to their workstations.

GPO name: GPO_Restrict_Control_Panel
Linked at: the domain root (corp.local), so it applies to all authenticated users
Setting: Prohibit access to Control Panel and PC settings, blocking control.exe
Verification
<img width="1919" height="1029" alt="image" src="https://github.com/user-attachments/assets/8175a910-da95-4583-b769-28095a0477c7" />
Phase 10: Client Workstation Setup

Added a Windows 10 client to test domain join and GPO inheritance.

Specs:

OS: Windows 10 Pro
Hostname: Corp-Client01
2 vCPU / 2 GB vRAM
Network adapter set to VMnet1 (Host-only) — same isolated network as the domain controller
Verification
<img width="1919" height="1029" alt="image" src="https://github.com/user-attachments/assets/6afaedbb-aaba-4900-9cef-6960b0bb2093" />

Next: configure networking on the client and join it to the domain.

Phase 11: Domain Join
Issue: no network connectivity

ping to the DC failed with PING: transmit failed. General failure.

Root cause: I'd disabled VMware's DHCP service back in Phase 1 so the domain controller could handle DHCP itself — but the DC wasn't yet serving DHCP at this point, so the client had no IP address at all.

Fix: set a temporary static IP on the client (192.168.100.20, DNS pointed at 192.168.100.10) so it could reach the DC and join the domain.

Once connectivity was confirmed, I joined the client to corp.local using domain administrator credentials.

Verification
<img width="1919" height="1031" alt="image" src="https://github.com/user-attachments/assets/d0d03892-3b26-4677-bed1-943b7f770209" />

Next: log in as a regular domain user and confirm Group Policy is applying correctly.

Phase 12: Verifying the Setup
Logged in as CORP\Alice.Support on the client for the first time and confirmed the forced password change worked as expected.
Tested the GPO by trying to open control.exe as that user — it was blocked, confirming the policy applies correctly to standard domain users.
Verification
<img width="1919" height="1027" alt="image" src="https://github.com/user-attachments/assets/7406912a-e559-4779-8837-5687eed72107" />
Skills Demonstrated

This lab covers the core skill set for a Helpdesk Tier 1 / Junior Systems Administrator role:

Networking: isolated host-only network design, static IP configuration, basic DHCP/DNS troubleshooting
Identity & Access Management: AD DS deployment, OU design, bulk account provisioning with PowerShell
Systems Hardening: Group Policy Object creation and enforcement
Troubleshooting: diagnosing and resolving real issues that came up during the build (password policy failures, network connectivity loss)
