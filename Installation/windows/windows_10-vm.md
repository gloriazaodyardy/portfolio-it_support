---
title: WINDOWS INSTALLATION via VIRTUALBOX
type: windows-installation
deployment_type: VM
status: Draft
author: Gloriaza Odyardy
created: 2026-10-25
updated: 2026-10-29
version: "1.2"
document_id: INST-VM-WIN10-001
file_name: Win10_22H2_English_x64v1
platform: Windows / Windows 10 (multi-edition ISO) / 10 22H2 / amd64
vm_name: windows_10
ram_mb: 2048
vcpu: 2
disk_gb: 20
username: name.dept
timezone: UTC (+07:00) Bangkok, Hanoi, Jakarta
encryption: none for VM lab
ntp_server: "{{NTP_SERVER}}"
proxy: "{{PROXY}}"
tags:
  - it-support
  - installation
  - windows
  - baseline
  - redacted
  - obsidian
  - dataview
confidentiality: Redact passwords, tokens, public IPs, and serial numbers before sharing
---
# `= this.title` — `= this.platform`

> Obsidian/Dataview: keep frontmatter keys and replace `{{...}}` values as needed.

## 1. Purpose
Standardize a clean Windows 10 Pro VM installation on VirtualBox for a normal local end user, without connecting to Windows Server, domain, or Entra ID.
## 2. Scope
- **In scope**: VM provisioning, media verification, local installation, OOBE, baseline config, verification
- **Out of scope**: domain/Entra join, Windows Server connectivity, MDM, mass deployment, enterprise-level of security, line-of-business app config
## 3. Prerequisites
### Host and hypervisor
- [ ] VirtualBox 7.2 installed and updated. Install [here](https://www.virtualbox.org/wiki/Downloads)
- [ ] Host has enough free storage for VDI, ISO, and snapshots
- [ ] Host virtualization support enabled (VT-x / AMD-V)
### Installation media
- [ ] ISO present and checksum verified: `= this.file_name`. Install [here](https://www.microsoft.com/en-us/software-download/windows10iso)
- [ ] ISO source is Microsoft or an approved mirror
- [ ] ISO is not stored inside the VM folder
### Credentials and access
- [ ] VM name: `= this.vm_name`
- [ ] Username: `= this.username` 
- [ ] Password manager entry created for VM and Windows accounts. Contact author to get the password.
- [ ] Encryption: `= this.encryption`
## 4. Steps
1. Create VM `= this.vm_name` in VirtualBox:
![[win10-general_info.png]]
   - Type: Microsoft Windows
   - Version: Windows 10 (64-bit)
   - Attach the verified ISO to the optical drive
   - Un-tick "Proceed with Unattended installation" to config manually
2. Allocate resources without exceeding the host budget:
![[win10-memory.png]]
![[win10-vhard_disk.png]]
   - Memory: `= this.ram_mb` MB as a minimum for Windows 10 lab
   - vCPU: `= this.vcpu`, minimum
   - Disk: VDI, dynamically allocated, `= this.disk_gb` GB minimum
   - Click **Finish** to create the VM.
3. Click Start to run the VM and complete Windows installation.
4. Setup the language, time and currency format, and also keyboard setup (can change this later)
5. Input if you have product key, otherwise click "I don't have a product key" to continue the installation without product key.
6. Select Windows 10 Pro so the VM can use RDP, Local Group Policy, and BitLocker management.
7. Tick the licence terms box to accept in and then click Next.
8. Select the empty drive, click New, then click Apply so Windows creates the system reserved partition automatically.
![[win10-drive.png]]

9. Wait until the Windows installation and restart process is complete.
10. OOBE:
	- Select region
	- Select keyboard, setup second keyboard or click Skip
	- Select Click Join domain to create normal local end user using template `name.dept`
	- Set a strong password and up to 3 security questions
	- Click Accept for Microsoft Edge as default browser (optional)
	- Privacy settings: No for Diagnostic data, Advertising ID, and Tailored experience
	- Keep custom experience for business only if required
	- Skip Cortana setup (optional)
11. Wait until setup completes, then configure only minimal browser and security baseline settings so the end user can focus on work
12. Verify regional format, date, and time
13. Activate Windows if a license is available, run updates, reboot until clean, then install drivers and Guest Additions.
14. Uninstall unnecessary default apps and disable activity of Cortana and Xbox in Settings > Gaming | Settings > Apps
15. Install work softwares via browser
	- **AnyDesk** version: 9.8.3
	- **7-Zip** version: 26.05
16. Enable the security baseline: firewall, Defender/AV, User Account Control/UAC (Search: UAC > push max the notification > Ok)
17. Verify network connectivity and time synchronization. If the physical host already has internet, Windows usually obtains IP, DNS, and time automatically through NAT/Bridged and the default time service; configure custom NTP (`= this.ntp_server`) or proxy (`= this.proxy`) only if policy or the target environment requires it.
18. Create a clean baseline snapshot:
    - Name: `win10-base-install`
## 5. Verification
| Area       | Expected                                          |
| ---------- | ------------------------------------------------- |
| Hypervisor | VirtualBox version recorded                       |
| Boot       | Normal boot to desktop                            |
| Version    | `winver` matches target                           |
| Activation | Activated or documented lab state                 |
| Drivers    | No unknown devices                                |
| Security   | Firewall/AV, UAC active; BitLocker matches policy |
| Network    | Network is connected                              |
| Updates    | No critical updates pending                       |
| Accounts   | Standard account login successful                 |
| Snapshot   | Baseline snapshot documented                      |

## 6. Rollback
1. Restore the `windows10-base-install` snapshot if post-install config fails.
2. Use WinRE for boot issues.
3. Recreate and reinstall from verified ISO if the VM is unrecoverable.
4. Record time, cause, and recovery steps in the change log.
## 7. Evidence and notes
- Store redacted screenshots/logs in `Installation/windows/assets/`.
- Suggested evidence: VM settings, port forwarding, installer, Guest Additions, activation, updates, disk, snapshot
- Record exceptions, temporary accounts, and known issues here.
## 8. Change log

| Version | Date             | Change                                  | By               |
| ------- | ---------------- | --------------------------------------- | ---------------- |
| 1.0     | `= this.created` | Initial Windows 10 VM installation file | `= this.author`  |
| 1.2     | 29, October 2026 | Install Windows 10 & Update file        | Gloriaza Odyardy |
