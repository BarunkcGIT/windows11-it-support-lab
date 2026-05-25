# Phase 4 — Security Baseline

## Goal
Configure core security settings to meet Windows 11 business security
standards, aligned with Finnish NIS2 compliance requirements.

---

### 4.1 Windows Defender
**Action:** Verified all Defender components via `Get-MpComputerStatus`.
**Result:** AntivirusEnabled, RealTimeProtectionEnabled, AMServiceEnabled,
AntispywareEnabled, IoavProtectionEnabled, NISEnabled — all True.
Signatures updated 24.5.2026. Reputation-based protection and Memory
integrity enabled. Windows Security dashboard shows all areas green.
**Evidence:** `29-defender-status.png`, `30-windows-security-dashboard.png`

---

### 4.2 Firewall Profiles
**Action:** Verified all three firewall profiles using `Get-NetFirewallProfile`.
**Result:** Domain, Private, and Public profiles all Enabled: True.
**Evidence:** `31-firewall-profiles.png`

---

### 4.3 UAC Verification
**Action:** Verified UAC registry values using `Get-ItemProperty`.
**Result:** EnableLUA=1 (UAC active), ConsentPromptBehaviorAdmin=5 (default prompt).
**Evidence:** `32-uac-status.png`

---

### 4.4 Password Policy
**Action:** Set minimum password length to 12 using `net accounts /minpwlen:12`.
**Result:** secpol.msc GUI was read-only (Enterprise policy restriction) but
command line succeeded. Complexity requirements already enabled by default.
**Evidence:** `33-password-policy-verified.png`

---

### 4.5 Account Lockout Policy
**Action:** Configured lockout using `net accounts` commands.
**Result:** Lockout threshold: 5 attempts, duration: 15 minutes,
observation window: 15 minutes.
**Evidence:** `34-account-lockout-policy.png`

---

### 4.6 BitLocker
**Action:** Enabled BitLocker on C: drive using TPM protector.
**Issue encountered:** Initial attempt failed — virtual CD/DVD drive was
attached. Disconnected the drive, retried successfully.
**Result:** VolumeStatus FullyEncrypted, ProtectionStatus On,
KeyProtectors: {Tpm, RecoveryPassword}.
**Evidence:** `35-bitlocker-enabled.png`

---

### 4.7 Final Verification
**Action:** Single PowerShell session verified all six security controls.
**Result:** All checks passed — Defender, Firewall (3 profiles), UAC,
password policy, lockout policy, BitLocker all confirmed active.
**Evidence:** `36-phase4-security-verification.png`

---

## Summary

| Control | Setting | Status |
|---|---|---|
| Windows Defender | All components active | ✅ |
| Firewall | All 3 profiles enabled | ✅ |
| UAC | Enabled, default prompt | ✅ |
| Password length | 12 characters minimum | ✅ |
| Account lockout | 5 attempts, 15 min | ✅ |
| BitLocker | On, TPM + RecoveryPassword | ✅ |
