Phase 8: PowerShell Automation
Overview
This phase demonstrates automated IT support tasks using PowerShell to reduce manual effort in deployment workflows. Three core scripts are implemented to automate Windows 11 readiness checks, bulk user creation, and application deployment.

Objectives
✅ Automate Windows 11 compatibility auditing

✅ Enable mass user creation from CSV

✅ Deploy applications using winget package manager

✅ Create repeatable, consistent IT workflows

Script 1: Windows 11 Readiness Check
Purpose
Automated hardware/OS readiness check with pass/fail verdict logic based on Microsoft's minimum Windows 11 requirements.

Script Code
powershell
# script1-readiness-check.ps1
# Purpose: Automated Windows 11 readiness check with pass/fail verdict

$readiness = @{
    OS_Version      = (Get-CimInstance Win32_OperatingSystem).Caption
    RAM_GB          = [math]::Round((Get-CimInstance Win32_ComputerSystem).TotalPhysicalMemory / 1GB, 2)
    Disk_FreeGB     = [math]::Round((Get-PSDrive C).Free / 1GB, 2)
    TPM_Present     = (Get-Tpm).TpmPresent
    SecureBoot      = Confirm-SecureBootUEFI
}

$readiness.Verdict = if ($readiness.RAM_GB -ge 4 -and $readiness.Disk_FreeGB -ge 20 -and $readiness.TPM_Present -and $readiness.SecureBoot) {
    "READY for Windows 11"
} else {
    "NOT READY - check requirements"
}

$readiness | Format-Table -AutoSize
$readiness | ConvertTo-Json | Out-File "C:\readiness-report.json"
Action
Ran automated readiness check script on ITSUP-W11-01 to verify Windows 11 compatibility, including pass/fail verdict logic based on Microsoft's minimum requirements.

Result
Requirement	Status	Value
TPM Present	✅ Pass	True
Secure Boot	✅ Pass	Enabled
RAM	✅ Pass	4.00 GB
Free Disk Space	✅ Pass	30.56 GB
Verdict: ✅ "READY for Windows 11"

Output saved to C:\readiness-report.json

Why It Matters
Manually checking each requirement one by one during pre-deployment audits wastes time and is prone to errors. This script gives an instant, repeatable audit result for any workstation.

Evidence
43-readiness-check.png


Script 2: Bulk User Setup
Purpose
Automate creation of multiple local user accounts from a CSV file with automatic group membership assignment.

Script Code
powershell
# script2-user-setup.ps1
# Purpose: Bulk create users from CSV with group membership

Import-Csv "C:\newusers.csv" | ForEach-Object {
    New-LocalUser -Name $_.Username -FullName $_.FullName -Password (ConvertTo-SecureString $_.Password -AsPlainText -Force) -PasswordNeverExpires:$false
    Add-LocalGroupMember -Group $_.Group -Member $_.Username
    Write-Output "$($_.Username) created and added to $($_.Group)"
}
CSV Input Format (newusers.csv)
csv
Username,FullName,Password,Group
jmakinen,Juhani Mäkinen,P@ssw0rd123,Users
alaine,Alaine Smith,SecurePass456,Users
Action
Executed bulk user creation script on ITSUP-W11-01, importing CSV data to create local user accounts with automatic group membership assignment.

Result
User	Full Name	Group	Status
jmakinen	Juhani Mäkinen	Users	✅ Created
alaine	Alaine Smith	Users	✅ Created
Verification:

✅ Both users Enabled

✅ Both added to Users group

✅ Home directories confirmed in C:\Users

✅ Password policy applied (must change on next login)

Why It Matters
Manual user creation takes 10-15 minutes per account and risks data entry errors. This script reduces onboarding time to under 1 minute for any number of users, ensuring consistency and auditability.

Evidence
44-Bulk-User-setup.png
Script 3: Application Deployment
Purpose
Automate software installation using winget (Windows Package Manager) for consistent, silent deployments.

Script Code
powershell
# script3-app-deployment.ps1
# Purpose: Automated application installation via winget

$apps = @(
    "7zip.7zip",
    "Mozilla.Firefox",
    "Notepad++.Notepad++"
)

foreach ($app in $apps) {
    Write-Output "Installing $app..."
    winget install $app --silent --accept-package-agreements
}

Write-Output "`nVerifying installations..."
winget list | Select-String -Pattern "7zip|Firefox|Notepad\+\+"
Action
Executed automated application deployment script on ITSUP-W11-01 using winget package manager to install 3 applications silently.

Result
Application	Winget ID	Version	Status
7-Zip	7zip.7zip	26.02	✅ Upgraded
Firefox	Mozilla.Firefox	152.0.5	✅ Installed
Notepad++	Notepad++.Notepad++	Existing	✅ Already present
Verification:

powershell
winget list | Select-String -Pattern "7zip|Firefox|Notepad\+\+"
Output confirmed all 3 applications are installed.

Why It Matters
Manual software installation takes 30-45 minutes per machine and is prone to errors. This script uses winget to automate deployment in under 5 minutes, ensuring:

✅ Consistency across all workstations

✅ Silent installation (no user interaction needed)

✅ Automatic upgrades for already installed apps

✅ Eliminates human error

Evidence
45-App-deployment.png
46-App-deployment.png
