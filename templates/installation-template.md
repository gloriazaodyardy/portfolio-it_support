---
title: "{{DOCUMENT_TITLE}}"
type: installation-template
status: Draft
author: "{{AUTHOR}}"
owner: "{{OWNER}}"
created: "{{DATE_CREATED}}"
updated: "{{DATE_REVIEWED}}"
version: "1.0"
document_id: "{{DOC_ID}}"
platform: "{{OS_FAMILY}} / {{OS_NAME}} / {{OS_VERSION}}"
architecture: "{{ARCH}}"
tags:
  - it-support
  - installation
  - template
  - operating-system
  - redacted
confidentiality: "Redact passwords, tokens, public IPs, and serial numbers before sharing"
related:
  - "../SOPs/"
  - "../Troubleshootings/"
  - "../scripts/"
---

# {{DOCUMENT_TITLE}}

> **Template:** OS/distribution installation (generic)  
> Copy this file, fill in the variables below, and remove irrelevant sections.  
> Do not commit passwords, tokens, serial numbers, public IPs, or personal user data.

## Metadata

| Item | Value |
|---|---|
| Document ID | `{{DOC_ID}}` |
| Status | Draft / Review / Approved / Archived |
| Owner | `{{OWNER}}` |
| Author | `{{AUTHOR}}` |
| Created | `{{DATE_CREATED}}` |
| Last reviewed | `{{DATE_REVIEWED}}` |
| Template version | `1.0` |
| Target platform | `{{OS_FAMILY}}` / `{{OS_NAME}}` / `{{OS_VERSION}}` / `{{ARCH}}` |
| Deployment type | Bare metal / VM / Cloud / Container |
| Change type | New install / Reinstall / Upgrade / Migration |

## Variables to fill

| Variable             | Example                            | Notes                                  |
| -------------------- | ---------------------------------- | -------------------------------------- |
| `{{OS_FAMILY}}`      | Linux / Windows / BSD / Network OS | Operating system family                |
| `{{OS_NAME}}`        | Debian / Windows / Ubuntu          | OS name                                |
| `{{OS_VERSION}}`     | 13 (Trixie) / 23H2                 | Version or channel                     |
| `{{DISTRO_VARIANT}}` | Server / Desktop / Minimal         | Image variant                          |
| `{{ARCH}}`           | amd64 / arm64 / x86_64             | Architecture                           |
| `{{HOSTNAME}}`       | support-pc-01                      | Redact if sensitive; use a placeholder |
| `{{INSTALL_MEDIA}}`  | USB 8 GB, ISO `debian-13.x.iso`    | Installation media                     |
| `{{INSTALL_METHOD}}` | Native / Dual boot / VM / PXE      | Deployment method                      |
| `{{PRIMARY_USER}}`   | `localadmin`                       | Initial administrative account         |
| `{{NETWORK_MODE}}`   | DHCP / Static IP                   | Network mode                           |
| `{{STORAGE_LAYOUT}}` | Single disk, LVM, GPT, BitLocker   | Storage layout                         |

---
## 1. Purpose

This document describes installing **{{OS_NAME}} {{OS_VERSION}}** from the initial state until the system is ready to use and verified.

Operational goals:
- Create a repeatable installation.
- Standardize the initial security and configuration baseline.
- Provide verification evidence and rollback steps.

## 2. Scope

### 2.1 In scope
- Installation media preparation.
- Firmware/boot configuration.
- Core OS installation.
- Initial configuration, updates, drivers/firmware, security, accounts, and networking.
- Post-installation verification.

### 2.2 Out of scope
- Advanced business or user-specific application configuration.
- Domain/MDM integrations requiring production credentials.
- In-depth troubleshooting for specific hardware (see troubleshooting documentation).

### 2.3 Platform matrix

| Platform | Supported | Notes |
|---|---|---|
| Bare metal | Yes / No | |
| Virtual machine | Yes / No | |
| Dual boot | Yes / No | |
| In-place upgrade | Yes / No | |

## 3. Prerequisites

### 3.1 Hardware / firmware
- [ ] CPU supports `{{ARCH}}`.
- [ ] Minimum RAM: `{{RAM_MIN}}`
- [ ] Minimum storage: `{{STORAGE_MIN}}`
- [ ] Firmware/BIOS is updated or version recorded: `{{FIRMWARE_VERSION}}`
- [ ] Secure Boot / TPM / virtualization settings verified where relevant.
- [ ] Basic input, display, and networking are available.
### 3.2 Software / media
- [ ] ISO/installer checksum verified.
- [ ] USB/cloud boot media is ready.
- [ ] Critical drivers are available offline if required.
- [ ] License/activation key is available and valid.
### 3.3 Data and access
- [ ] Existing user or old-system data is backed up and restore-tested.
- [ ] Local/administrator account credentials are prepared and stored in a password manager.
- [ ] Network/internet access is available if online updates are required.
- [ ] Approval or change request is available if required.
### 3.4 References
- Vendor official documentation: `{{VENDOR_DOC_URL}}`
- Related internal SOP: `{{RELATED_SOP}}`
- Related script/automation: `{{RELATED_SCRIPT}}`

## 4. Risk & Impact

| Risk | Impact | Mitigation |
|---|---|---|
| Old data overwritten | High | Backup + restore verification |
| Wrong partition selected | High | Confirm target disk before committing changes |
| Network driver failure | Medium | Prepare offline drivers / USB tethering |
| Update failure | Medium | Snapshot or restore point before updates |
| Weak credentials | High | Use password manager and MFA where available |

## 5. Pre-Installation Checklist

- [ ] Machine hostname/label is defined.
- [ ] Target disk is confirmed (capacity, model; redact serial).
- [ ] Timezone is planned: `{{TIMEZONE}}`.
- [ ] Language/locale is planned: `{{LOCALE}}`.
- [ ] Storage policy (encryption, partitions) is approved.
- [ ] Rollback plan is prepared.

## 6. Installation Steps

> Use sequential numbering. Save important screenshots/logs in `./assets/`.

### 6.1 Prepare installation media
1. Download the official ISO/installer.
2. Verify the checksum.
3. Write it to media (balenaEtcher, Rufus, `dd`, etc.).
4. Label the media with version and date.

### 6.2 Firmware / boot configuration
1. Enter BIOS/UEFI.
2. Set boot order: removable media first.
3. Configure Secure Boot as required: `{{SECURE_BOOT}}`
4. Configure virtualization: `{{VIRTUALIZATION}}`
5. Save and reboot.

### 6.3 Storage layout
1. Select the correct target disk.
2. Create the layout according to policy:

   | Partition | FS | Size | Purpose |
   |---|---|---|---|
   | `{{PART_BOOT}}` | | | Boot/EFI |
   | `{{PART_ROOT}}` | | | System |
   | `{{PART_DATA}}` | | | Data/user |
   | `{{PART_SWAP}}` | | | Swap/pagefile |

3. Confirm before writing changes.

### 6.4 Base OS installation
1. Boot from installation media.
2. Select language, locale, keyboard, and timezone.
3. Select disk/layout.
4. Create the initial account/administrator.
5. Complete the base installation until the first reboot.

### 6.5 First boot / OOBE
1. Log in using the account created.
2. Configure or skip optional features according to the baseline.
3. Create an initial recovery point or snapshot where supported.

### 6.6 Initial OS-specific configuration
> Adjust this section per OS/distribution. Examples: package manager, service manager, firewall, hostname.

```text
{{OS_SPECIFIC_STEPS}}
```

### 6.7 Updates & patches
1. Connect to a trusted network.
2. Run system updates.
3. Install the latest security patches.
4. Reboot if required.

### 6.8 Drivers / firmware
1. Verify critical drivers: GPU, NIC, storage controller, audio, printer.
2. Install drivers from official sources.
3. Reboot and test devices.

### 6.9 Security baseline
- [ ] System updates enabled.
- [ ] Firewall enabled with default deny inbound.
- [ ] Administrative account separated from daily-use account where policy supports it.
- [ ] Password and MFA policy applied.
- [ ] Disk encryption applied where required: `{{ENCRYPTION}}`
- [ ] Remote access (SSH/RDP) restricted or disabled until hardening is complete.
- [ ] Basic logging/audit enabled where policy supports it.

### 6.10 User & access
1. Create a standard end-user account.
2. Store administrator credentials securely.
3. Test sudo/UAC/elevation.
4. Remove temporary or demo accounts if present.

### 6.11 Network configuration
- Mode: `{{NETWORK_MODE}}`
- IP/DNS/gateway: `{{IP_CONFIG}}`
- NTP: `{{NTP_SERVER}}`
- Proxy: `{{PROXY}}`

Verification:
- [ ] IP configured correctly.
- [ ] DNS resolves.
- [ ] Gateway reachable.
- [ ] Time synchronized successfully.

### 6.12 Application baseline (minimum)
- [ ] Browser / terminal / editor / monitoring tools.
- [ ] Antivirus/EDR where policy requires it.
- [ ] Approved remote support tool.
- [ ] Backup agent where required.

## 7. Verification

Run all checks below and save evidence.

| Area | Command / Step | Expected | Result |
|---|---|---|---|
| Boot | Restart system | Normal boot | |
| OS version | `{{VERSION_COMMAND}}` | `{{OS_VERSION}}` | |
| Network | `ping` / browser | Internet / LAN OK | |
| Time | NTP sync | Synchronized | |
| Disk | `{{DISK_COMMAND}}` | Layout matches plan | |
| Firewall | Firewall status | Active | |
| Accounts | Standard and admin login | Successful | |
| Update | Latest patch check | No critical updates pending | |

### Evidence
Save screenshots/logs in:
- `./assets/` or `Installation/<os>/assets/`
- Redact sensitive data before commit.

## 8. Post-Installation Baseline Checklist

- [ ] Baseline documented and approved.
- [ ] Change/ticket recorded.
- [ ] Asset label/hostname recorded (redact if public).
- [ ] Initial configuration backup created.
- [ ] Maintenance and update plan defined.

## 9. Troubleshooting Quick Reference

| Symptom | Likely cause | First steps | Related document |
|---|---|---|---|
| Fails to boot from USB | Boot order / Secure Boot | Check BIOS, recreate media | `../Troubleshootings/` |
| No network | Driver / cable / DHCP | Check NIC and IP configuration | `../Troubleshootings/` |
| Update fails | Repository / key / proxy | Check package manager logs | `../Troubleshootings/` |
| Login fails | Keyboard layout / password | Recover account safely | `../Troubleshootings/` |

## 10. Rollback / Recovery

1. If installation fails before final commit: boot installation media again and repeat.
2. If the system boots but configuration is wrong: use restore point or snapshot.
3. If storage is corrupted or mispartitioned: restore from backup and reinstall.
4. If an upgrade or migration fails: use vendor recovery media and restore backup.
5. Record time, cause, and recovery steps in the Change Log section.

## 11. Assets & Evidence

| File | Description | Location |
|---|---|---|
| `install-media-checksum.txt` | Installer hash | `./assets/` |
| `boot-uefi.png` | Boot settings | `./assets/` |
| `partition-layout.png` | Disk layout | `./assets/` |
| `network-verification.txt` | IP/DNS/ping | `./assets/` |
| `os-version.txt` | OS version | `./assets/` |

## 12. Related Documents

- Installation index: `../Installation/README.md` (if available)
- Related SOP: `../SOPs/`
- Related troubleshooting: `../Troubleshootings/`
- Related scripts: `../scripts/`
- Ticket/issue: `{{TICKET_REF}}`

## 13. Notes, Caveats, Known Issues

- `{{KNOWN_ISSUES}}`

## 14. Change Log

| Version | Date       | Change           | By             |
| ------- | ---------- | ---------------- | -------------- |
| 1.0     | 21/10/2026 | Initial template | Gloriaza Y.L.O |

---

## Recommended use of this template

Do not use one installation document for all operating systems or distributions. Use this file as the **shared skeleton**, then create per-OS/per-distribution derivatives for truly different steps (package manager, partitioning, drivers, updates, encryption). This keeps the standard sections (Purpose, Scope, Verification, Rollback) consistent while allowing accurate details.
