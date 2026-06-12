# Phase 5 — Business App Deployment

## Goal
Deploy standard business applications silently using winget (Windows
Package Manager) to simulate IT helpdesk software deployment workflow.

## winget version
v1.28.240 — verified with `winget --version`

## Applications deployed

| Application | winget ID | Version | Method |
|---|---|---|---|
| Google Chrome | Google.Chrome | 149.0.7827.54 | winget install |
| 7-Zip | 7zip.7zip | 26.01 | winget install |
| Adobe Acrobat Reader | Adobe.Acrobat.Reader.64-bit | 26.0.0.1 | winget install |
| Microsoft Teams | Microsoft.Teams | 26120.3106.4722.3411 | winget install |
| Visual Studio Code | Microsoft.VisualStudioCode | 1.123.0 | winget install |
| Notepad++ | Notepad++.Notepad++ | 8.9.6.4 | winget install |

## Commands used

```powershell
winget install Google.Chrome
winget install Adobe.Acrobat.Reader.64-bit
winget install Microsoft.VisualStudioCode
winget install 7zip.7zip
winget install Microsoft.Teams
winget install Notepad++.Notepad++
winget list
```

## Why winget matters
winget allows IT technicians to deploy software silently and repeatably
without clicking through GUI installers. This is how modern IT teams
deploy software at scale — one command per app, no user interaction
required, consistent results across all machines.

## Evidence
- `37-winget-version.png` — winget version confirmed
- `38-winget-list.png` — all apps verified installed
