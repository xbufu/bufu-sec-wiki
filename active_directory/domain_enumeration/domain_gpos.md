# Domain GPOs

## General

- Security settings
- Registry-based policy settings
- GPP like start/shutdown/log-on/logff script settings
- Software installation
- Abused for privesc, backdoors, persistence

## Display RSoP Summary Data

```powershell
gpresult /R

# AD Module
Get-GPResultantSetOfPolicy -ReportType Html -Path C:\Users\Administrator\report.html
```

## Get List of GPOs in Current Domain

```powershell
# AD Module
Get-GPO -All

# PowerView
Get-NetGPO
Get-NetGPO | Select displayname
Get-NetGPO -ComputerName ws01.lab.local
Get-DomainGPO -ComputerIdentity windows1.testlab.local
```

## Get GPO(s) Which Use Restricted Groups or groups.xml for Interesting Users

```powershell
Get-NetGPOGroup
```

## Get Users Which Are in a Local Group of a Machine Using GPO

```powershell
Find-GPOComputerAdmin -ComputerName ws01.lab.local
```

## Get Machines where the given User is a Member of a Specific Group

```powershell
Find-GPOLocation -UserName user -Verbose
```

## Enumerate what Machines that a Particular User/Group Identity Has Local Admin Rights to

```powershell
# Get-DomainGPOUserLocalGroupMapping == old Find-GPOLocation
Get-DomainGPOUserLocalGroupMapping -Identity <User/Group>
```

## Enumerate what Machines that a given User in the Specified Domain Has RDP Access Rights to

```powershell
Get-DomainGPOUserLocalGroupMapping -Identity <USER> -Domain <DOMAIN> -LocalGroup RDP
```

## Export a CSV of All GPO Mappings

```powershell
Get-DomainGPOUserLocalGroupMapping | %{$_.computers = $_.computers -join ", "; $_} | Export-CSV -NoTypeInformation gpo_map.csv
```
