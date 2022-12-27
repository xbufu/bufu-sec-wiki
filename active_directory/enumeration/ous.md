# Domain OUs

## Get OUs in a Domain

```powershell
# AD Module
Get-ADOrganizationalUnit -Filter * -Properties *

# PowerView
Get-NetOU -FullData
```

## Get GPO Applied on an OU (Read GPOName from Gplink Attribute from Get-NetOU)

```powershell
# AD Module
Get-GPO -GUID "AB306569-220D-43FF-B03B-83E8F4EF8081"

# PowerView
Get-NetGPO -GPOname "{AB306569-220D-43FF-B03B-83E8F4EF8081}"
```

## Find All Computers in a given OU

```powershell
Get-DomainComputer -SearchBase "ldap://OU=..."
```

## Get the Logged on Users for All Machines in Any *server* OU in a Particular Domain

```powershell
Get-DomainOU -Identity *server* -Domain <domain> | %{Get-DomainComputer -SearchBase $_.distinguishedname -Properties dnshostname | %{Get-NetLoggedOn -ComputerName $_}}
```
