# Phase 6 — User and Access Management

## Goal
Demonstrate core user management tasks performed by IT helpdesk technicians,
following the principle of least privilege.

---

### 6.1 User Account Review

**Action:** Listed all local users and group memberships.

**Result:**
- Techadmin: Enabled, member of Administrators
- testuser: Enabled, member of Users only
- Administrator, Guest, DefaultAccount: all disabled

**Commands used:**
```powershell
Get-LocalUser | Format-Table Name, Enabled, LastLogon, Description
net localgroup Administrators
net localgroup Users
```

**Evidence:** `39-user-review.png`

---

### 6.2 Password Reset

**Action:** Simulated helpdesk password reset for testuser.

**Command used:**
```powershell
net user testuser NewP@ssword2026!
```

**Result:** Password reset successfully. Account verified as active,
group membership unchanged (Users only — no admin rights).

**Why it matters:** Password resets are one of the most common helpdesk
tickets in Finnish workplaces. IT tech must reset without granting
extra permissions.

**Evidence:** `40-password-reset.png`

---

### 6.3 Temporary Admin Elevation

**Action:** Temporarily elevated testuser to Administrators for an
approved task, then removed admin rights immediately after.

**Commands used:**
```powershell
net localgroup Administrators testuser /add
net localgroup Administrators
net localgroup Administrators testuser /delete
net localgroup Administrators
```

**Result:** testuser added → verified as admin → removed → verified
as standard user again. Full cycle documented.

**Why it matters:** In Finnish businesses, standard users sometimes
need temporary admin rights for approved tasks (e.g. installing
approved software). IT policy requires removing those rights
immediately after. This demonstrates proper access control workflow.

**Evidence:** `41-temp-admin-elevation.png`

---

## Summary

| Task | Action | Result |
|---|---|---|
| User review | Listed all users and groups | ✅ Verified |
| Password reset | Reset testuser password | ✅ Successful |
| Temp elevation | Add → verify → remove admin | ✅ Full cycle done |

## Key principle demonstrated
**Least privilege:** Users have only the permissions needed for their
daily work. Admin rights are granted only when necessary and removed
immediately after use.
