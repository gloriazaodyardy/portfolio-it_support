---
title: OPERATING SYSTEM INSTALLATION TEMPLATE
type: installation-template
deployment_type: Bare metal / VM / Upgrade
status: Draft
author: Gloriaza Odyardy
created: 2026-10-25
updated: 2026-10-25
version: "1.0"
document_id: "{{DOC_ID}}"
platform: "{{OS_FAMILY}} / {{OS_NAME}} / {{OS_EDITION}} / [[OS_VERSION}} / {{ARCH}}"
file_name: "{{FILE_NAME}}"
hostname: "{{DEPLOYMENT-ROLE-OS-NUMBER}}"
timezone: "{{TIMEZONE}}"
encryption: "{{ENCRYPTION}}"
tags:
  - it-support
  - installation
  - operating-system
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
Provide a standard, repeatable, and predictable Operating System (OS) installation baseline.
## 2. Scope
- **In scope**: media, OS install, OOBE, baseline configuration, verification
- **Out of scope**: MDM/domain design, mass deployment, application-specific configuration
## 3. Prerequisites
- [ ] Official installation media and verified checksum
- [ ] Valid license if required
- [ ] Existing data backed up and restore-tested
- [ ] Target platform/disk identified
- [ ] Required firmware/virtualization settings configured
- [ ] Administrator credentials stored securely
## 4. Steps
1. Prepare installation media.
2. Boot target system and enter firmware settings if applicable.
3. Select the correct disk and apply approved storage layout.
4. Install the operating system and complete OOBE/first boot.
5. Set hostname `` `= this.hostname` ``, timezone `` `= this.timezone` ``, and baseline network.
6. Apply updates, drivers, and security baseline.
7. Configure accounts, privileges, and required access methods.
8. Configure encryption according to `` `= this.encryption` ``.
9. Install approved minimum baseline tools.
## 5. Verification
| Area     | Expected                                        |
| -------- | ----------------------------------------------- |
| Boot     | Normal boot                                     |
| platform | `= this.platform` `= this.os_version` confirmed |
| Network  | Have correct IP, DNS, gateway                   |
| Time     | Synchronized                                    |
| Disk     | Matches planned layout                          |
| Security | Firewall/AV baseline applied                    |
| Updates  | No critical pending updates                     |
| Accounts | Have standard and admin login                   |

## 6. Rollback
1. Reboot installation media and repeat if install fails.
2. Restore snapshot or system backup for bad configuration.
3. Reinstall and restore data if storage/boot-loader is damaged.
4. Document time, cause, and recovery actions.
## 7. Evidence and notes
- Store redacted screenshots/logs in the OS-specific assets folder.
- Record exceptions, decisions, and known issues here.
## 8. Change log

| Version | Date             | Change                           | By              |
| ------- | ---------------- | -------------------------------- | --------------- |
| 1.0     | `= this.created` | Initial OS installation template | `= this.author` |
