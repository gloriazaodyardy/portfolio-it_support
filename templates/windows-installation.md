---
title: WINDOWS INSTALLATION BASELINE
type: installation-template
deployment_type: Bare metal / VM / Upgrade
status: Draft
author: Gloriaza Odyardy
created: 2026-10-25
updated: 2026-10-25
version: "1.0"
document_id: "{{DOC_ID}}"
file_name: "{{FILE_NAME}}"
platform: Windows / {{WINDOWS_EDITION}} / {{WINDOWS_VERSION}} / {{ARCH}}
vm_name: "{{VM_NAME}}"
hostname: "{{DEPLOYMENT-ROLE-OS-NUMBER}}"
timezone: "{{TIMEZONE}}"
primusr/stdusr: "{{PRIMARY_USER}} / {{STANDARD_USER}}"
encryption: "{{ENCRYPTION}}"
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

> Obsidian/Dataview: keep frontmatter keys and replace `{{...}}` values. Rendered values use inline queries below:

```
`=(space)this.title` 
```
## 1. Purpose
Standardize a clean Windows installation.
## 2. Scope
- **In scope**: media, installation, OOBE, baseline config, verification
- **Out of scope**: MDM/domain design, mass deployment, LOB app config
## 3. Prerequisites
- [ ] ISO verified / bootable media ready
- [ ] Valid license or entitlement
- [ ] User data backed up
- [ ] Target disk identified
- [ ] Admin credentials prepared
## 4. Steps
1. Prepare bootable media and verify hash.
2. Boot installer and create approved storage layout.
3. Install Windows, then complete OOBE.
4. Rename host to `` `= this.hostname` ``; create standard and admin accounts.
5. Activate, update, reboot, install drivers.
6. Enable security baseline: firewall, AV, UAC, BitLocker if required, RDP policy.
7. Configure network, NTP `` `= this.ntp_server` ``, proxy `` `= this.proxy` ``, and baseline applications.
## 5. Verification
| Area       | Expected                                     |
| ---------- | -------------------------------------------- |
| Boot       | Normal boot                                  |
| Version    | `winver` matches target                      |
| Activation | Activated                                    |
| Drivers    | No unknown devices                           |
| Security   | Firewall/AV active; BitLocker matches policy |
| Network    | Have correct IP, DNS, gateway, NTP           |
| Updates    | No critical updates pending                  |
| Accounts   | Have standard & admin login                  |

## 6. Rollback
1. Use WinRE for boot issues.
2. Reinstall from media if baseline fails.
3. Restore BitLocker key and data from verified backup.
4. Revert/upgrade only if documented and approved.
## 7. Evidence and notes
- Store redacted screenshots/logs in `Installation/windows/assets/`.
- Record issues, decisions, and exceptions here.
## 8. Change log

| Version | Date             | Change                   | By              |
| ------- | ---------------- | ------------------------ | --------------- |
| 1.0     | `= this.created` | Initial windows template | `= this.author` |
