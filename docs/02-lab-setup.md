# Lab Setup

## Overview
This lab simulates an IT technician workflow using a virtual machine as
the target "business device". All installation, configuration, security,
and troubleshooting work is performed inside the VM, with the host machine
acting as the technician's workstation.

## Host machine (technician)
- OS: Windows 11
- CPU: AMD Ryzen 7 5700G
- RAM: 16 GB
- Virtualization: Enabled in BIOS

## Hypervisor
- VMware Workstation Pro 25H2
- TPM 2.0 support enabled
- Secure Boot support enabled
- VM encryption enabled (required for TPM)

## Guest machine (simulated user device)

| Setting | Value |
|---|---|
| Name | ITSUP-W11-01 |
| Guest OS | Windows 11 x64 |
| Processors | 1 CPU, 2 cores |
| Memory | 4 GB (4096 MB) |
| Disk | 80 GB, single file NVMe |
| Firmware | UEFI |
| Secure Boot | Enabled |
| TPM | 2.0 (VMware, version 2.102.0.1) |
| Network | NAT |
| Encryption | Files needed for TPM encrypted |

## Software installed on guest
- Windows 11 Enterprise Evaluation (Version 24H2, Build 26100.1742)
- VMware Tools (for enhanced VM features: dynamic resolution, shared clipboard)

## Why these choices
- **VMware Workstation Pro** supports native TPM 2.0 emulation, which is
  required for Windows 11 installation and BitLocker later in the project.
- **Enterprise Evaluation edition** provides 90 days of free access to
  all business features including BitLocker, Group Policy, and AppLocker.
- **NAT networking** allows internet access without exposing the VM
  directly to the host network — safer for testing.
- **TPM-backed encryption** mirrors how modern business laptops are secured
  out of the box.

## Evidence
See `/screenshots/` folder:
- `01-vm-hardware-settings.png` — VMware configuration summary
- `05-vmware-tools-install.png` — VMware Tools installation
- `08-vmware-settings-tpm-present.png` — TPM listed in VM hardware
