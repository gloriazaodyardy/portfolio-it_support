---
title: Windows Installation Baseline
type: installation-template
template: windows
status: Draft
author: "{{AUTHOR}}"
created: "{{DATE_CREATED}}"
updated: "{{DATE_REVIEWED}}"
version: "1.0"
document_id: "{{DOC_ID}}"
platform: Windows / {{WINDOWS_EDITION}} / {{WINDOWS_VERSION}} / {{ARCH}}
confidentiality: Redact passwords, tokens, public IPs, and serial numbers before sharing
---
# Windows Installation Baseline — {{WINDOWS_EDITION}} {{WINDOWS_VERSION}}

> Obsidian/Dataview compatible: keep frontmatter keys and replace `{{...}}` values as needed.

## 1. Purpose
Standardize a clean, secure, and verifiable Windows installation.

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
4. Rename host; create standard and admin accounts.
5. Activate, update, reboot, install drivers.
6. Enable security baseline: firewall, AV, UAC, BitLocker if required, RDP policy.
7. Configure network, NTP, proxy, and baseline applications.

## 5. Verification
| Area       | Expected                                     |
| ---------- | -------------------------------------------- |
| Boot       | Normal boot                                  |
| Version    | `winver` matches target                      |
| Activation | Activated                                    |
| Drivers    | No unknown devices                           |
| Security   | Firewall/AV active; BitLocker matches policy |
| Network    | Have IP, DNS, gateway, NTP                   |
| Updates    | No critical updates pending                  |
| Accounts   | Have Standard & admin login                  |
## 6. Rollback
1. Use WinRE for boot issues.
2. Reinstall from media if baseline fails.
3. Restore BitLocker key and data from verified backup.
4. Revert/upgrade only if documented and approved.

## 7. Evidence and notes
- Store redacted screenshots/logs in `Installation/windows/assets/`.
- Record issues, decisions, and exceptions here.

## 8. Change log

| Version | Date | Change | By |
|---|---|---|---|
| 1.0 | {{DATE_CREATED}} | Initial template | {{AUTHOR}} |
