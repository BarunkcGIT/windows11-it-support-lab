# Phase 1 — Hardware Readiness Assessment

## Goal
Verify that the target virtual machine meets all Windows 11 hardware and
firmware requirements before deployment.

## Target machine
- Hostname: ITSUP-W11-01
- Hypervisor: VMware Workstation Pro 25H2
- Guest OS: Windows 11 Enterprise Evaluation (Version 24H2, Build 26100.1742)

## Verification results

| Requirement | Expected | Actual | Status |
|---|---|---|---|
| CPU cores | 2+ | 2 | PASS |
| RAM | 4 GB+ | 4 GB | PASS |
| Storage | 64 GB+ | 80 GB | PASS |
| Firmware | UEFI | UEFI | PASS |
| Secure Boot | Enabled | True | PASS |
| TPM | 2.0 | 2.0 (VMware, version 2.102.0.1) | PASS |
| Network | Reachable | Ping google.com 2ms | PASS |

## Commands used

```powershell
Get-ComputerInfo | Select-Object CsName, WindowsProductName, OsArchitecture, CsTotalPhysicalMemory, WindowsVersion
Get-Tpm
Confirm-SecureBootUEFI
Get-Volume
hostname
whoami
(Get-CimInstance Win32_OperatingSystem).Caption
winver
Test-NetConnection google.com -InformationLevel Detailed
```

## Finding noted
`Get-ComputerInfo` reports `WindowsProductName` as "Windows 10 Enterprise Evaluation"
despite a confirmed Windows 11 installation. This is expected behaviour — Microsoft
retains the Windows 10 product family identifier internally for backward compatibility.
Verified correct OS using `(Get-CimInstance Win32_OperatingSystem).Caption` and `winver`,
both of which correctly report Windows 11 Version 24H2, Build 26100.1742.

## Conclusion
Device passed all Windows 11 readiness checks and is approved for deployment.

## Evidence
See `/screenshots/` folder:
- `06-msinfo32-os-name.png`
- `07-msinfo32-system-details.png`
- `09-tpm-verification-get-tpm.png`
- `10-os-verification-multi-method.png`
- `11-phase1-final-verification.png`
