---
title: "Linux Distribution Installation - {{DISTRO_NAME}} {{DISTRO_VERSION}}"
type: installation-template
template: "linux-distribution"
status: Draft
author: "{{AUTHOR}}"
owner: "{{OWNER}}"
created: "{{DATE_CREATED}}"
updated: "{{DATE_REVIEWED}}"
version: "1.0"
document_id: "{{DOC_ID}}"
platform: "{{DISTRO_NAME}} / {{DISTRO_VERSION}} / {{CODENAME}}"
architecture: "{{ARCH}}"
tags:
  - it-support
  - installation
  - linux
  - distribution
  - baseline
  - redacted
confidentiality: "Redact passwords, tokens, public IPs, and serial numbers before sharing"
related:
  - "../templates/installation-template.md"
  - "../SOPs/"
  - "../Troubleshootings/"
  - "../scripts/"
---

# Linux Distribution Installation — {{DISTRO_NAME}} {{DISTRO_VERSION}}

> **Derived template** from `installation-template.md` for Linux distribution installations.  
> Adjust for package manager, init system, and disk layout.

## Metadata

| Item | Value |
|---|---|
| Document ID | `{{DOC_ID}}` |
| Status | Draft / Review / Approved / Archived |
| Owner | `{{OWNER}}` |
| Target OS | `{{DISTRO_NAME}}` `{{DISTRO_VERSION}}` (`{{CODENAME}}`) |
| Architecture | `{{ARCH}}` |
| Deployment | Bare metal / VM / Dual boot / Cloud |
| Image | Netinst / Live / Server / Minimal / Cloud |
| Init system | `{{INIT_SYSTEM}}` (systemd / OpenRC / runit) |
| Package manager | `{{PKG_MANAGER}}` (apt / dnf / pacman / zypper) |
| Desktop | `{{DESKTOP}}` / Server (headless) |

## Linux Variables

| Variable | Example |
|---|---|
| `{{DISTRO_NAME}}` | Debian / Ubuntu / Fedora / Arch |
| `{{DISTRO_VERSION}}` | 13 / 24.04 / 40 |
| `{{CODENAME}}` | Trixie / Noble / Rawhide |
| `{{INSTALL_MEDIA}}` | USB, netinst ISO |
| `{{PART_BOOT}}` | `/boot/efi` |
| `{{PART_ROOT}}` | `/` |
| `{{PART_DATA}}` | `/home` or `/data` |
| `{{PART_SWAP}}` | swap / zram |
| `{{FILESYSTEM}}` | ext4 / btrfs / xfs |
| `{{ENCRYPTION}}` | LUKS on root / none |
| `{{PKG_MANAGER}}` | apt |
| `{{PRIMARY_USER}}` | `sysadmin` |
| `{{SUDO_GROUP}}` | sudo / wheel |
| `{{SSH_PORT}}` | 22 or custom |
| `{{FIREWALL}}` | ufw / firewalld / nftables |
| `{{TIMEZONE}}` | Asia/Jakarta |
| `{{LOCALE}}` | en_US.UTF-8 / id_ID.UTF-8 |
| `{{NTP_SERVICE}}` | systemd-timesyncd / chrony |

---

## 1. Purpose

Standardize installation of a Linux distribution from official media through a secure and verified administration baseline.

## 2. Scope

### In scope
- Bootable media creation.
- Partitioning, filesystem, and encryption.
- Base system and bootloader installation.
- User/sudo, network, repository, and update configuration.
- Drivers/firmware, basic services, firewall, and SSH hardening.
- Post-installation verification.

### Out of scope
- Advanced desktop customization.
- Full container or Kubernetes orchestration.
- Application-specific service configuration (database, web server) without a separate document.

## 3. Prerequisites

- [ ] Official distribution ISO.
- [ ] Bootable USB or PXE server.
- [ ] Checksum/signature verified.
- [ ] Data backed up.
- [ ] Internet connection (netinst) or local mirror.
- [ ] Target disk and firmware understood.
- [ ] Root/administrator credentials prepared.
- [ ] Repository mirror and proxy policy known for offline or restricted networks.

## 4. Pre-Installation Checklist

- [ ] Target platform: bare metal / VM / dual boot.
- [ ] Boot mode: UEFI/BIOS `{{BOOT_MODE}}`.
- [ ] Secure Boot status: `{{SECURE_BOOT}}`.
- [ ] Storage layout approved.
- [ ] Existing data layout understood; backup verified.
- [ ] Hostname: `{{HOSTNAME}}`.
- [ ] Locale/timezone: `{{LOCALE}}` / `{{TIMEZONE}}`.
- [ ] Mirror/repositories: `{{REPO_MIRROR}}`.
- [ ] Encryption plan: `{{ENCRYPTION}}`.

## 5. Installation Steps

### 5.1 Prepare installation media
1. Download the ISO.
2. Verify the checksum:
   ```bash
   sha256sum {{ISO_FILE}}
   ```
3. Write it to USB:
   ```bash
   sudo dd if={{ISO_FILE}} of=/dev/sdX bs=4M status=progress conv=fsync
   ```
   Replace `/dev/sdX` with the correct device; verify with `lsblk`.
4. Or use an approved GUI tool.

### 5.2 Firmware / boot configuration
1. Enter UEFI/BIOS.
2. Set boot order to USB.
3. Configure Secure Boot:
   - Debian/Ubuntu commonly support Secure Boot.
   - Other distributions may require disabling it.
4. Enable virtualization if VMs will run.
5. Save and reboot.

### 5.3 Storage layout
1. Select the target disk carefully.
2. Define the layout:

   | Mount | FS | Suggested size | Notes |
   |---|---|---:|---|
   | `/boot/efi` | FAT32/vfat | 512 MB | UEFI |
   | `/boot` | ext4 | 1 GB | If separate |
   | `/` | `{{FILESYSTEM}}` | 30-60 GB | Root |
   | `/home` or `/data` | `{{FILESYSTEM}}` | Remainder | Data |
   | swap | swap / zram | RAM or 0 | According to policy |

3. Enable LUKS if `{{ENCRYPTION}}`.
4. For dual boot, do not format partitions belonging to the other OS.

### 5.4 Base installation
1. Boot the installer.
2. Select language, keyboard, and location.
3. Configure initial networking.
4. Set hostname and domain (if applicable).
5. Create root/admin and standard user accounts.
6. Select software baseline: minimal / server / desktop tasks.
7. Install the bootloader to the correct disk.
8. Reboot and eject media.

### 5.5 First boot
1. Log in as `{{PRIMARY_USER}}`.
2. Verify boot services.
3. Check the initial journal:
   ```bash
   journalctl -xb
   ```
4. Create an initial snapshot or backup if tools are available.

### 5.6 Repository & time
1. Set timezone:
   ```bash
   sudo timedatectl set-timezone {{TIMEZONE}}
   ```
2. Set locale:
   ```bash
   sudo localectl set-locale LANG={{LOCALE}}
   ```
3. Configure mirrors/repositories:
   - Debian/Ubuntu: `/etc/apt/sources.list` or `.sources`
   - Fedora: `/etc/yum.repos.d/`
   - Arch: `/etc/pacman.d/mirrorlist`
4. Update package metadata.

### 5.7 Updates & package baseline
Use commands matching `{{PKG_MANAGER}}`:

```bash
# Debian/Ubuntu
sudo apt update && sudo apt full-upgrade -y

# Fedora
sudo dnf upgrade --refresh -y

# Arch
sudo pacman -Syu --noconfirm

# openSUSE
sudo zypper refresh && sudo zypper update -y
```

- Install baseline packages (curl, wget, git, vim/nano, htop, tmux, sudo, rsync, ca-certificates).
- Reboot if the kernel changes.

### 5.8 User & privilege
1. Confirm `{{PRIMARY_USER}}` exists.
2. Add the user to the sudo/wheel group:
   ```bash
   sudo usermod -aG {{SUDO_GROUP}} {{PRIMARY_USER}}
   ```
3. Test sudo:
   ```bash
   sudo whoami
   ```
4. Lock or disable direct root access if policy requires:
   - `passwd -l root` or disable root SSH login.
5. Use SSH keys for administration; minimize password login.

### 5.9 Network configuration
- Mode: `{{NETWORK_MODE}}`
- IP/DNS/gateway: `{{IP_CONFIG}}`
- NTP: `{{NTP_SERVICE}}`

Configure according to distribution:
- NetworkManager: `nmcli`
- systemd-networkd: `/etc/systemd/network/`
- Legacy ifupdown: `/etc/network/interfaces`

Verify:
```bash
ip a
ip route
resolvectl status
timedatectl
```

### 5.10 Firewall
Choose `{{FIREWALL}}`:
```bash
# UFW example
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow {{SSH_PORT}}/tcp
sudo ufw enable
sudo ufw status verbose

# firewalld example
sudo firewall-cmd --set-default-zone=public
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
```
- Open additional ports only when required and documented.
- Do not expose database or web application ports publicly.

### 5.11 SSH hardening (if SSH is enabled)
1. Back up `/etc/ssh/sshd_config`.
2. Set:
   - `PermitRootLogin no`
   - `PasswordAuthentication no` (after deploying keys)
   - `PubkeyAuthentication yes`
   - Port: `{{SSH_PORT}}`
3. Validate:
   ```bash
   sudo sshd -t
   ```
4. Restart:
   ```bash
   sudo systemctl restart sshd
   ```
5. Test from a separate terminal before closing the current session.

### 5.12 Drivers / firmware
1. Check hardware:
   ```bash
   lspci -nnk
   lsusb
   ```
2. Install firmware:
   - Debian: `firmware-linux`, `firmware-iwlwifi`, etc.
   - Ubuntu: `linux-firmware`
   - Fedora: `linux-firmware`, RPM Fusion if required
3. Reboot and test NIC/Wi-Fi/GPU/audio.

### 5.13 Security baseline
- [ ] Automatic security updates configured (unattended-upgrades / dnf-automatic / pacman hooks) if adopted.
- [ ] Firewall active.
- [ ] SSH hardened.
- [ ] AppArmor/SELinux status matches policy.
- [ ] Disk encryption matches `{{ENCRYPTION}}`.
- [ ] Direct root access restricted.
- [ ] Logging/journal retention matches policy.

### 5.14 Application baseline (minimum)
- [ ] curl/wget/rsync/git/htop/tmux/nano
- [ ] Basic monitoring (htop, btop, atop).
- [ ] Approved remote support tool.
- [ ] Backup agent if adopted.
- [ ] Standard text editor / pager.

## 6. Verification

| Area | Command | Expected |
|---|---|---|
| Boot | `uptime`, reboot test | Normal boot |
| OS version | `hostnamectl` / `cat /etc/os-release` | `{{DISTRO_VERSION}}` |
| Network | `ping`, `curl` | OK |
| DNS | `resolvectl query` | OK |
| Time | `timedatectl` | Synchronized |
| Disk | `lsblk`, `findmnt` | Layout matches plan |
| Firewall | `ufw status` / `firewall-cmd --state` | Active |
| Accounts | `id {{PRIMARY_USER}}`, `sudo -l` | OK |
| Update | Package manager check | No critical updates pending |
| SSH | `sshd -t`, remote login | OK (if enabled) |

### Reference commands
```bash
cat /etc/os-release
hostnamectl
ip a
ip route
timedatectl
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
sudo ufw status verbose 2>/dev/null || true
systemctl --failed
journalctl -p 3 -xb
```

## 7. Troubleshooting Quick Reference

| Symptom | First steps |
|---|---|
| Fails to boot from USB | Check boot mode, recreate media, adjust Secure Boot |
| No Wi-Fi | Firmware, `rfkill`, driver |
| Update fails | Check mirror, GPG key, proxy, logs |
| SSH lockout | Use console; check port, firewall, key permissions |
| Desktop login loop | Check home permissions, disk space, journal |
| LUKS prompt fails | Keyboard layout, recovery key |

## 8. Rollback / Recovery

1. If installation is incomplete: boot installation media again.
2. If configuration is wrong: use snapshot or backup.
3. If bootloader is damaged: chroot from live ISO and reinstall grub/systemd-boot.
4. If LUKS is damaged: restore header backup (if available) or restore data from backup.
5. If upgrade fails: roll back with the package manager if supported; otherwise reinstall and restore.

## 9. Assets & Evidence

Store in `Installation/linux/assets/` and redact sensitive hostnames/IPs:
- `os-release.txt`
- `hostnamectl.txt`
- `partition-layout.png`
- `firewall-status.txt`
- `network-verification.txt`
- `journal-errors.txt`

## 10. Related Documents
- Generic template: `../templates/installation-template.md`
- Related SOP: `../SOPs/`
- Troubleshooting: `../Troubleshootings/`
- Scripts: `../scripts/`

## 11. Change Log
| Version | Date       | Change           | By             |
| ------- | ---------- | ---------------- | -------------- |
| 1.0     | 21/10/2026 | Initial template | Gloriaza Y.L.O |
