# Files and Shares

## Find Shares on Hosts in Current Domain

```powershell
Invoke-ShareFinder -Verbose
```

## Find Shares from other Domain

```powershell
Invoke-ShareFinder -Domain lab.local
```

## Exclude Default Shares

```powershell
Invoke-ShareFinder -ExcludeStandard
```

## Show only Shares the Current User Has Access to

```powershell
Invoke-ShareFinder -CheckShareAccess
```

## Find Sensitive Files on Computers

```powershell
Invoke-FileFinder -Verbose
```

## Get All Fileservers

```powershell
Get-NetFileServer
```

## Use Alternate Credentials when Searching for Files

```powershell
# Find-InterestingDomainShareFile == old Invoke-FileFinder
$Password = "PASSWORD" | ConvertTo-SecureString -AsPlainText -Force
$Credential = New-Object System.Management.Automation.PSCredential("DOMAIN\user",$Password)
Find-InterestingDomainShareFile -Domain Domain -Credential $Credential
```
