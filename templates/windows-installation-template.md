---
title: "Windows Installation Baseline - {{WINDOWS_EDITION}} {{WINDOWS_VERSION}}"
type: installation-template
template: "windows"
status: Draft
author: "{{AUTHOR}}"
owner: "{{OWNER}}"
created: "{{DATE_CREATED}}"
updated: "{{DATE_REVIEWED}}"
version: "1.0"
document_id: "{{DOC_ID}}"
platform: "Windows / {{WINDOWS_EDITION}} / {{WINDOWS_VERSION}}"
architecture: "{{ARCH}}"
tags:
  - it-support
  - installation
  - windows
  - baseline
  - endpoint
  - redacted
confidentiality: "Redact passwords, tokens, public IPs, and serial numbers before sharing"
related:
  - "../templates/installation-template.md"
  - "../SOPs/"
  - "../Troubleshootings/"
  - "../scripts/"
---

# Windows Installation Baseline — {{WINDOWS_EDITION}} {{WINDOWS_VERSION}}

> **Derived template** from `installation-template.md` for Windows clients.  
> Focus: clean installation, OOBE, drivers, Windows Update, accounts, BitLocker/Defender, and baseline verification.

## Metadata

| Item | Value |
|---|---|
| Document ID | `{{DOC_ID}}` |
| Status | Draft / Review / Approved / Archived |
| Owner | `{{OWNER}}` |
| Target OS | Windows `{{WINDOWS_EDITION}}` `{{WINDOWS_VERSION}}` (`{{BUILD}}`) |
| Architecture | `{{ARCH}}` (x64 / arm64) |
| Deployment | Bare metal / VM / In-place upgrade |
| License | OEM / Retail / Volume / Digital entitlement |
| Language | `{{LANGUAGE}}` |
| Region / Timezone | `{{REGION}}` / `{{TIMEZONE}}` |

## Windows Variables

| Variable | Example |
|---|---|
| `{{WINDOWS_EDITION}}` | Pro / Enterprise / Home |
| `{{WINDOWS_VERSION}}` | 11 23H2 / 10 22H2 |
| `{{BUILD}}` | 22631.xxxx |
| `{{SECURE_BOOT}}` | Enabled (recommended for Windows 11) |
| `{{TPM}}` | TPM 2.0 Enabled |
| `{{VIRTUALIZATION}}` | VT-x / AMD-V Enabled |
| `{{ENCRYPTION}}` | BitLocker on system drive / None |
| `{{PRIMARY_USER}}` | `localadmin` |
| `{{STANDARD_USER}}` | `{{END_USER}}` |
| `{{LOCAL_ACCOUNT_POLICY}}` | Local only / Microsoft account / Entra ID / Domain join |
| `{{DISK_LAYOUT}}` | GPT, EFI + MSR + Windows + Recovery |

---

## 1. Purpose

Standardize a clean, secure, and verifiable Windows installation for client endpoints.

## 2. Scope

### In scope
- Windows installation media creation.
- UEFI configuration (Secure Boot, TPM, virtualization).
- Clean installation and OOBE.
- Account configuration, hostname rename, Windows Update, drivers, Defender/antivirus.
- BitLocker, firewall, remote access, and application baseline.

### Out of scope
- Full enterprise domain join, Intune, or MDM.
- Mass deployment (Autopilot/MECM) unless referenced separately.
- Line-of-business application configuration.

## 3. Prerequisites

- [ ] Official Windows ISO from Microsoft.
- [ ] USB installer with at least 8 GB.
- [ ] Installer hash verified.
- [ ] Valid license or digital entitlement.
- [ ] User data backed up.
- [ ] Windows 11 requirements met: TPM 2.0, Secure Boot capable CPU, supported CPU.
- [ ] NIC/storage drivers available if installation is offline.
- [ ] Administrator account credentials prepared in a password manager.

## 4. Pre-Installation Checklist

- [ ] Hostname is defined: `{{HOSTNAME}}`
- [ ] Target disk is selected and backed up.
- [ ] Timezone: `{{TIMEZONE}}`.
- [ ] Encryption plan: `{{ENCRYPTION}}`.
- [ ] Initial account mode is selected: `{{LOCAL_ACCOUNT_POLICY}}`.
- [ ] Restore point/backup is verified.

## 5. Installation Steps

### 5.1 Prepare installation media
1. Download the Media Creation Tool or ISO.
2. Verify the hash/checksum.
3. Create a bootable USB:
   - GPT for UEFI.
   - FAT32/NTFS according to the tool.
4. Label the USB with version and date.

### 5.2 Firmware / UEFI settings
1. Reboot to BIOS/UEFI.
2. Set boot mode to **UEFI only** (disable Legacy CSM where possible).
3. Enable Secure Boot: `{{SECURE_BOOT}}`.
4. Enable TPM: `{{TPM}}`.
5. Enable virtualization: `{{VIRTUALIZATION}}`.
6. Set USB first in boot order.
7. Save and exit.

### 5.3 Storage and partitioning
1. Boot from USB.
2. Select **Custom: Install Windows only (advanced)** for a clean install.
3. Select the target disk.
4. Delete old partitions on the correct disk if reinstalling.
5. Allow Windows to create the default layout or create it manually:

   | Partition | Purpose |
   |---|---|
   | EFI System Partition | Boot |
   | MSR | Reserved |
   | Primary | Windows |
   | Recovery | WinRE |

6. Continue installation.

### 5.4 Base Windows installation
1. Select language, time format, and keyboard.
2. Click **Install now**.
3. Enter product key or select **I don't have a product key**.
4. Select edition: `{{WINDOWS_EDITION}}`.
5. Accept the license terms.
6. Wait for file copy, feature installation, and updates.
7. Reboot automatically.

### 5.5 OOBE / first boot
1. Select region and keyboard.
2. Connect to a network if required.
3. Select setup mode:
   - Personal or Organization according to policy.
   - Domain: manual later, or join during OOBE if planned.
4. Create local admin account `{{PRIMARY_USER}}` or use MSA/Entra.
5. Disable privacy/experience options according to company baseline.
6. Wait until the desktop is ready.

### 5.6 Rename host and accounts
1. Rename the computer to `{{HOSTNAME}}`.
2. Create standard account `{{STANDARD_USER}}`.
3. Place the admin in the Administrators group.
4. Test UAC elevation.
5. Remove demo or bloatware accounts if present.

### 5.7 Activation
1. Open **Settings > System > Activation**.
2. Confirm the status is Activated.
3. For volume/retail licenses, use `slmgr.vbs` only according to vendor licensing procedure.

### 5.8 Windows Update
1. Run Windows Update until no important updates are pending.
2. Install available driver updates.
3. Reboot until updates are complete.
4. Check again.

### 5.9 Drivers
1. Verify drivers for NIC, GPU, chipset, audio, touchpad, and printer.
2. Use Windows Update, OEM vendor tools, or official sources.
3. Avoid drivers from unknown sources.
4. Reboot and test hardware.

### 5.10 Disk encryption (BitLocker)
If policy requires it:
1. Confirm TPM is active.
2. Enable BitLocker on the system volume.
3. Store the recovery key in a safe location (AD/Entra/password manager).
4. Verify unlock/recovery procedures safely.
5. If not used, document the reason and compensating controls.

### 5.11 Security baseline
- [ ] Windows Firewall enabled on all profiles.
- [ ] Microsoft Defender / AV enabled and updated.
- [ ] SmartScreen enabled according to policy.
- [ ] UAC at standard/high level.
- [ ] Remote Desktop disabled unless required; if enabled, restrict users and require NLA.
- [ ] RDP port is not published directly.
- [ ] Office/macro hardening applied according to policy (if any).

### 5.12 Network configuration
- Mode: `{{NETWORK_MODE}}`
- IP/DNS/gateway: `{{IP_CONFIG}}`
- NTP: `time.windows.com` or `{{NTP_SERVER}}`
- Proxy: `{{PROXY}}`

Steps:
1. Configure IP/DNS as documented.
2. Enable automatic time sync.
3. Test DNS resolution and internet access.

### 5.13 Application baseline (minimum)
- [ ] Approved standard browser and extensions.
- [ ] Office/productivity if license is available.
- [ ] Approved remote support tool (AnyDesk/Quick Assist/TeamViewer).
- [ ] PDF reader, 7-Zip, VLC, etc.
- [ ] Backup agent where applicable.
- [ ] Chocolatey/winget packaging if standardization is used. Document the source.

### 5.14 Privacy & telemetry
- Configure privacy options according to organization policy.
- Disable ads/suggested apps where enterprise standardization requires it.
- Do not change security settings without documentation.

## 6. Verification

| Area | Step | Expected |
|---|---|---|
| Boot | Restart | Normal boot under 2 minutes |
| Activation | Settings > Activation | Activated |
| Windows version | `winver` | `{{WINDOWS_VERSION}}` build `{{BUILD}}` |
| Drivers | Device Manager | No unknown devices |
| Firewall | `Get-NetFirewallProfile` | Enabled on all profiles |
| Defender | Security Center | No action needed |
| Disk | BitLocker status | Matches `{{ENCRYPTION}}` |
| Network | `ipconfig`, ping, DNS | OK |
| Time | Time sync | Synchronized |
| Update | Windows Update | No critical updates pending |
| Accounts | Standard and admin login | Successful |
| Remote access | RDP/NLA status | Matches policy |

### Reference commands
```powershell
winver
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsBuildNumber
Get-NetFirewallProfile | Select Name, Enabled
Get-BitLockerVolume
Get-Service wuauserv, mpssvc
```

## 7. Troubleshooting Quick Reference

| Symptom | First steps |
|---|---|
| OOBE requires internet | Use official bypass or prepare offline NIC drivers |
| No network | Check NIC driver, `ipconfig`, disable/enable adapter |
| Activation fails | Check license, connectivity, date/time |
| BitLocker recovery | Use recovery key from storage location |
| Update loop | Run Windows Update troubleshooter, check CBS log |

## 8. Rollback / Recovery

1. Use **Windows Recovery Environment** (`WinRE`) for Startup Repair.
2. Reinstall from USB if the baseline fails.
3. Restore BitLocker recovery key if the system is locked.
4. Restore data from backup.
5. If an in-place upgrade fails, revert to the previous version within 10 days (if available) or perform a clean reinstall.

## 9. Assets & Evidence

Store in `Installation/windows/assets/` and redact sensitive data:
- `uefi-settings.png`
- `disk-partition.png`
- `activation.png`
- `windows-update.png`
- `bitlocker-status.txt`
- `network-verification.txt`

## 10. Related Documents
- Generic template: `../templates/installation-template.md`
- Related SOP: `../SOPs/`
- Troubleshooting: `../Troubleshootings/`
- Scripts: `../scripts/`

## 11. Change Log
| Version | Date       | Change           | By             |
| ------- | ---------- | ---------------- | -------------- |
| 1.0     | 21/10/2026 | Initial template | Gloriaza Y.L.O |
