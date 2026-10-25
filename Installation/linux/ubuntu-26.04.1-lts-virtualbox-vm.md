---
title: Ubuntu Server 26.04.1 LTS on VirtualBox VM
type: installation-implementation
status: Draft
author: Gloriaza Y.L.O
owner: IT Support
created: 2026-10-21
updated: 2026-10-21
version: "1.0"
document_id: INST-VM-UBUNTU2604-2026-001
platform: Ubuntu Server / 26.04.1 LTS / VirtualBox VM
architecture: amd64
hypervisor: VirtualBox
vm_name: ubuntu-server-2604
guest_os: Ubuntu (64-bit)
ram_mb: 4096
vcpus: 0
disk_size_gb: 40
disk_type: VDI dynamically allocated
network_mode: NAT with port forwarding
host_ssh_port: 2222
guest_ssh_port: 22
iso_file: ubuntu-26.04.1-live-server-amd64.iso
primary_user: sysadmin
timezone: Asia/Jakarta
locale: en_US.UTF-8
tags:
  - it-support
  - installation
  - linux
  - ubuntu-server
  - virtualbox
  - vm
  - implementation
  - redacted
confidentiality: Redact passwords, tokens, public IPs, and serial numbers before sharing
related:
  - ../../templates/linux-distribution-installation-template.md
  - ../../templates/os-installation-template.md
  - ../../SOPs/
  - ../../Troubleshooting/
  - ../../scripts/
---
# `= this.title`
> Real implementation guide for installing **`= this.platform`** inside a VirtualBox VM.  
## 1. Purpose
This file guides real implementation in a VirtualBox VM step-by-step, including VM resources, virtual networking, and VM-specific verification steps.

## 2. Prerequisites
- [ ] VirtualBox installed on the host or download [here](https://www.virtualbox.org/wiki/Downloads).
- [ ] Ubuntu Server 26.04.1 LTS ISO available or download [here](https://ubuntu.com/download/server).
- [ ] Target VM folder has enough free host storage.
- [ ] Password manager prepared for VM and Ubuntu credentials.
- [ ] Host network available for packages and updates.

## 3. Create the VM in VirtualBox
1. Open VirtualBox and click **New**.
2. Set:
   - Name: `ubuntu-server-2604`
   - Type: Linux
   - Version: Ubuntu (64-bit)
3. Allocate memory: `4096` MB.
4. Create virtual hard disk:
   - Type: VDI
   - Storage: Dynamically allocated
   - Size: `40` GB
5. Attach the Ubuntu Server ISO to the VM optical drive.
6. Disable audio if not needed.
7. Configure display with default VMSVGA settings.

## 4. Configure network and serial/console access
Recommended for lab use:
- Adapter 1: **NAT**
- Port forwarding:
  - Host port: `2222`
  - Guest port: `22`
  - Protocol: TCP

Alternative for LAN testing:
- Adapter 1: **Bridged Adapter**
- Adapter 2: NAT for internet access

For server administration, use SSH:
```bash
ssh -p 2222 sysadmin@127.0.0.1
```

## 5. Install Ubuntu Server
1. Start the VM.
2. Boot from the ISO.
3. Select language and keyboard layout.
4. Configure network.
5. Set hostname: `ubuntu-server-2604`.
6. Create user `sysadmin`.
7. Use guided storage with LVM if desired.
8. Enable OpenSSH server.
9. Install and reboot.
10. Eject the ISO after installation.

## 6. Post-installation configuration
```bash
sudo apt update && sudo apt full-upgrade -y
sudo usermod -aG sudo sysadmin
sudo apt install -y curl wget git vim nano htop tmux ufw
```

Enable firewall:
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw status verbose
```

Harden SSH in `/etc/ssh/sshd_config`:
```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
Port 22
```

Validate and restart:
```bash
sudo sshd -t
sudo systemctl restart sshd
```

## 7. VM maintenance and snapshots
1. Create a clean baseline snapshot before major changes:
   - VirtualBox → Machine → Take Snapshot
2. Name snapshots clearly, for example:
   - `ubuntu-26.04.1-clean-install`
   - `ubuntu-26.04.1-hardened`
3. Test recovery by restoring a snapshot in a separate clone if possible.
4. Keep host backups of the VM folder for disaster recovery.

## 8. Verification

| Area | Step | Expected |
|---|---|---|
| VM boot | Start VM | Boots normally |
| SSH | `ssh -p 2222 sysadmin@127.0.0.1` | Login successful |
| OS version | `cat /etc/os-release` | Ubuntu 26.04.1 LTS |
| Network | `ip a`, `ping`, `curl` | OK |
| Firewall | `sudo ufw status verbose` | Active |
| Updates | `apt` check | No critical updates pending |
| Snapshot | Snapshot list | Baseline snapshot exists |

### Reference commands
```bash
hostnamectl
ip a
ip route
timedatectl
lsblk
systemctl --failed
journalctl -p 3 -xb
```

## 9. Assets & Evidence
Store in `Installation/linux/assets/` and redact sensitive data:
- `virtualbox-vm-settings.png`
- `port-forwarding-rules.png`
- `ubuntu-installer.png`
- `ssh-login.png`
- `os-release.txt`
- `network-verification.txt`
- `snapshot-list.png`

## 10. Related Documents
- Generic template: `../../templates/linux-distribution-installation-template.md`
- Generic installation template: `../../templates/os-installation-template.md`
- Related SOP: `../../SOPs/`
- Troubleshooting: `../../Troubleshooting/`
- Scripts: `../../scripts/`

## 11. Change Log
| Version          | Date     | Change                                    | By              |
| ---------------- | -------- | ----------------------------------------- | --------------- |
| `= this.version` | 10/21/26 | Created from VirtualBox VM implementation | `= this.author` |
