# Phase 2 — Windows 11 Installation

## Goal
Perform a clean installation of Windows 11 Enterprise Evaluation on the VMware VM.

## Installation steps taken

1. Mounted Windows 11 ISO to the VM's virtual CD/DVD drive.
2. Powered on the VM and booted from the installer.
3. Selected "Install Windows 11" and accepted the data deletion warning.
4. Chose "I don't have a product key" (Evaluation mode).
5. Selected Windows 11 Enterprise Evaluation edition.
6. Accepted the license terms.
7. Chose "Custom: Install Windows only (advanced)".
8. Installed to Disk 0 (80 GB unallocated) — let Windows create partitions automatically.
9. Waited for installation to complete (multiple automatic restarts).
10. Created local admin account: **Techadmin**.
11. Reached the Windows 11 desktop.
12. Installed VMware Tools to enable enhanced VM features (dynamic resolution,
    shared clipboard, improved driver support).
13. Disconnected the installation ISO from the virtual CD/DVD drive to prevent
    accidental boot into the installer on subsequent restarts and to free up resources.

## Resulting disk layout

| Drive | File system | Size | Purpose |
|---|---|---|---|
| — | FAT32 | 96 MB | EFI System Partition (UEFI bootloader) |
| C: | NTFS | 79.26 GB | Main Windows drive |
| — | NTFS | 642 MB | Windows Recovery Partition |

This is a standard Windows 11 UEFI disk layout, created automatically by
Windows Setup when installing to unallocated space.

## Verification after install

Confirmed successful setup with:

```powershell
winver                                        # → Windows 11 Version 24H2, Build 26100.1742
hostname                                      # → ITSUP-W11-01
whoami                                        # → itsup-w11-01\techadmin
(Get-CimInstance Win32_OperatingSystem).Caption   # → Microsoft Windows 11 Enterprise Evaluation
```

## Post-install cleanup
Disconnected the Windows 11 installation ISO from the VM's virtual CD/DVD
drive. Leaving installation media mounted after deployment is considered
poor practice: it can cause the VM to attempt booting into the installer
on restart and wastes resources. This step is part of standard business
deployment checklists.

## Evidence
See `/screenshots/` folder:
- `02-windows-setup-option.png` — Install Windows 11 screen
- `03-user-account-creation.png` — Techadmin account creation
- `04-first-login-desktop.png` — Windows 11 desktop after first login
- `05-vmware-tools-install.png` — VMware Tools installation
- `12-disconnect-iso-vmware-menu.png` — ISO disconnected via VMware menu
