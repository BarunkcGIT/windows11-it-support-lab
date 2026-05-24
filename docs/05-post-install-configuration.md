# Phase 3 — Post-Install Configuration

## Goal
Configure the freshly installed Windows 11 machine for business use,
following standard IT deployment procedures used in Finnish workplaces.

---

### 3.1 Time Zone Verification

**Action:** Verified time zone using PowerShell `Get-TimeZone`.
**Result:** Already correctly set to FLE Standard Time (UTC+02:00 Helsinki). No change required.
**Why it matters:** Correct time zone ensures accurate event log timestamps and scheduled tasks.
**Evidence:** `15-get-timezone-verification.png`

---

### 3.2 Region and Language (fi-FI)

**Action:** Changed system locale and culture from en-US to Finnish (fi-FI).
**Commands:** `Set-WinSystemLocale fi-FI`, `Set-Culture fi-FI`, `Set-WinHomeLocation -GeoId 77`
**Result:** After restart, date format changed to Finnish: `lauantai 18. huhtikuuta 2026`. System locale and culture confirmed as fi-FI.
**Why it matters:** Finnish date formats, decimal separators, and week numbering differ from US standards. Correct locale prevents formatting errors in business apps.
**Evidence:** `16-region-language-before.png`, `17-region-change-commands.png`, `18-region-language-after.png`

---

### 3.3 Windows Update

**Action:** Installed pending updates using PSWindowsUpdate PowerShell module.
**Commands:** `Install-Module PSWindowsUpdate`, `Get-WindowsUpdate -Install -AcceptAll`
**Result:** Broadcom driver update (9.17.11.1, 28MB) downloaded and installed successfully.
**Why it matters:** Keeping systems updated is the most fundamental security practice in IT.
**Evidence:** `19-windows-update-scan.png`, `20-windows-update-history.png`

---

### 3.4 Standard User Account Creation

**Action:** Created local standard user `testuser` via `lusrmgr.msc`.
**Method:** GUI — Local Users and Groups management console
**Result:** testuser created as member of Users group only. No admin rights. Least privilege confirmed.
**Why it matters:** Standard user accounts limit the damage malware or human error can cause. Required by Finnish business security policies.
**Evidence:** `21-users-before.png`, `22-standard-user-creation.png`, `23-user-list-after-creation.png`

---

### 3.5 Remote Desktop

**Action:** Enabled Remote Desktop via Windows Settings (System → Remote Desktop → On).
**Verification:** Confirmed via three methods:
1. Windows Defender Firewall — Remote Desktop allowed on Private and Public networks
2. Services (services.msc) — Remote Desktop Services status: Running
3. Live RDP connection established from host PC to ITSUP-W11-01

**Why it matters:** Remote Desktop is the primary tool for IT helpdesk remote support in Finnish workplaces.
**Evidence:** `24-remote-desktop-enable.png`, `25-rdp-firewall-allowed.png`, `26-rdp-live-connection-verified.png`, `27-rdp-service-running.png`

---

## Summary

| Task | Status | Method |
|---|---|---|
| Time zone | ✅ Verified correct | PowerShell |
| Region/language | ✅ Changed to fi-FI | PowerShell + restart |
| Windows Update | ✅ 1 update installed | PSWindowsUpdate module |
| Standard user | ✅ testuser created | lusrmgr.msc |
| Remote Desktop | ✅ Enabled and verified | Settings + Services + live RDP |

## Computer name
ITSUP-W11-01 — set during Windows 11 installation. No rename required.
