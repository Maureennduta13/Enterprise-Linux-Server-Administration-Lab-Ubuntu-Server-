# Module 1: Base Setup & Initial Hardening

## Objective
Get a fresh Ubuntu Server VM installed, updated, and configured with the
baseline settings every server needs before anything else is layered on top.

## Prerequisites
- VirtualBox installed on host (Windows 11)
- Ubuntu Server 26.04 LTS ISO downloaded
- VM created (4GB RAM, 2 CPUs, 25GB disk)

## Steps

### 1. Install VirtualBox and download Ubuntu Server 26.04 LTS
Installed VirtualBox for Windows hosts, then downloaded the current LTS release
directly from ubuntu.com/download/server (made sure to pick the release
explicitly labeled LTS, not an interim release like 25.04 or 25.10).

### 2. Create the VM
- Name: `ubuntu-server`
- 4096 MB RAM, 2 CPUs, 25GB dynamically allocated disk
- Used VirtualBox's unattended install feature to set hostname/username/password
  up front
- Checked "Install OpenSSH server" during setup

### 3. Configure two network adapters
- **Adapter 1**: NAT — gives the VM internet access for updates/packages
- **Adapter 2**: Host-only (`vboxnet0` / VirtualBox Host-Only Ethernet Adapter)
  — a private network that exists only between Windows and the VM, used for
  a reliable SSH connection

### 4. Update the system
```bash
sudo apt update && sudo apt upgrade -y
```

### 5. Set a static IP on the host-only adapter
Edited `/etc/netplan/00-installer-config.yaml`:
```yaml
network:
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      dhcp4: false
      addresses: [192.168.56.10/24]
  version: 2
```
```bash
sudo netplan apply
```

### 6. Confirm SSH access from Windows
```bash
sudo systemctl status ssh
```
From Windows Terminal:
```
ssh maureenserver@192.168.56.10
```

## Verification
```bash
ip a
```
Confirmed `enp0s8` shows the static `192.168.56.10/24` address, and a
successful SSH login from Windows Terminal lands at a normal shell prompt.

## Issues & Troubleshooting

**1. VirtualBox couldn't create a host-only network (E_FAIL / "Could not find
Host Interface Networking driver")**
- Symptom: Tools → Network → Host-only Networks → Create failed immediately
  with a driver error.
- Root cause: the VirtualBox Host-Only Ethernet Adapter existed in Windows
  Device Manager but was **disabled**.
- Fix: Device Manager → Network adapters → right-click "VirtualBox Host-Only
  Ethernet Adapter" → Enable device. Retried Create in VirtualBox and it worked.
- Takeaway: a repair-install of VirtualBox alone did not fix this — the actual
  problem was on the Windows device driver side, not the VirtualBox
  installation itself.

**2. Downloaded the wrong Ubuntu release the first time**
- Grabbed Ubuntu 25.04 (an interim, non-LTS release) by mistake before
  realizing an LTS release was the right choice for a lab meant to reflect
  real enterprise practice. Deleted the VM and ISO, re-downloaded the correct
  LTS release (26.04), and rebuilt the VM from scratch.
- Takeaway: always double check for "LTS" next to the version number before
  downloading, not just the first big "Download" button on the page.

**3. Forgot the username/password set during unattended install**
- Since it was a fresh VM with no real data, recovered access via GRUB
  recovery mode instead of reinstalling:
  1. Reboot → catch the GRUB menu (hold Shift if it's skipped)
  2. Advanced options for Ubuntu → select the `(recovery mode)` kernel entry
  3. From the recovery menu, choose "root – Drop to root shell prompt"
  4. `mount -o remount,rw /` (filesystem starts read-only in recovery mode)
  5. `cat /etc/passwd | grep 1000` to find the first user account (UID 1000)
  6. `passwd <username>` to set a new password, then `reboot`
- Takeaway: this is a realistic, common admin task — good to have practiced
  it directly rather than just reading about it. Wrote the new credentials
  down immediately after resetting them.

**4. Duplicate `enp0s8:` key in netplan config broke the static IP**
- While hand-editing the netplan file in `nano` to add a static IP for the
  host-only adapter, accidentally created **two separate `enp0s8:` blocks**
  instead of adding the `addresses:` line inside the existing one. YAML
  doesn't allow duplicate keys, so the static IP never actually applied — the
  interface kept getting a DHCP-assigned address (`192.168.56.101`) instead.
- Fix: rewrote the file cleanly using `sudo tee ... << 'EOF'` to avoid further
  `nano` cursor-navigation mistakes, keeping `enp0s3` on DHCP and setting
  `enp0s8` to `dhcp4: false` with a single `addresses:` entry.
- Interim workaround: SSH'd in using the DHCP-assigned address (`.101`)
  instead of waiting on the static IP fix, so the rest of the lab wasn't
  blocked.
- Takeaway: for anything beyond a one-line edit, writing the whole file with
  `tee`/heredoc is more reliable than navigating `nano` by hand — fewer
  chances for indentation or duplicate-key mistakes.

**5. Static IP finally applied — but with a leftover DHCP lease**
- After the `tee`/heredoc fix and a later reboot, `ip a` showed `enp0s8` with
  **two** addresses: the intended static `192.168.56.10/24` *and* a lingering
  DHCP lease (`192.168.56.101/24 ... secondary dynamic`).
- Since the static one is present and marked primary, `ssh
  maureenserver@192.168.56.10` is now the correct, reliable address going
  forward instead of the old DHCP-assigned `.101`.
- Takeaway: this is just Ubuntu keeping the old lease around after the
  interface got a new configuration; it isn't a sign of a broken config, and
  clears after another clean reboot.

## Key Takeaways
Getting from a blank VM to a working SSH session touched real sysadmin
fundamentals: VirtualBox networking (NAT vs host-only), Linux account
recovery, and YAML/netplan syntax pitfalls. None of these went smoothly on
the first try — which is normal, and exactly the kind of friction a real
server admin runs into. Documenting the actual failures (not just the happy
path) is what makes this section useful to future-me and to anyone
reviewing the repo.
