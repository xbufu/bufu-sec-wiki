# Domains

## Get Current Domain

```powershell
# AD Module
Get-ADDomain

# PowerView
Get-NetDomain
```

## Get Object of Another Domain

```powershell
# AD Module
Get-ADDomain -Identity lab.local

# PowerView
Get-NetDomain -Domain lab.local
```

## Get Domain SID for Current Domain

```powershell
# AD Module
(Get-ADDomain).DomainSID

# PowerView
Get-DomainSID
```

## Get Domain Policy for Current Domain

```powershell
# PowerView
Get-DomainPolicy
(Get-DomainPolicy)."System Access"
```

## Get Password Policy for Another Domain

```powershell
# PowerView
(Get-DomainPolicy -Domain lab.local)."System Access"
```

## Get Kerberos Policy for e.g. Mimikatz Golden Tickets

```powershell
# PowerView
(Get-DomainPolicy -Domain lab.local)."Kerberos Policy"
```

## Get Domain Controllers for Current Domain

```powershell
# AD Module
Get-ADDomainController

# PowerView
Get-NetDomainController
```

## Get Domain Controllers for Another Domain

```powershell
# AD Module
Get-ADDomainController -DomainName lab.local -Discover

# PowerView
Get-NetDomainController -Domain lab.local
```

## Enumerate All Gobal Catalogs in the Forest

```powershell
# PowerView
Get-ForestGlobalCatalog
```

## Turn a List of Computer Short Names to FQDNs, Using a Global Catalog

```powershell
# PowerView
gc computers.txt | % {Get-DomainComputer -SearchBase "GC://GLOBAL.CATALOG" -LDAP "(name=$_)" -Properties dnshostname}
```

## Enumerate the Current Domain Controller Policy

```powershell
# PowerView
$DCPolicy = Get-DomainPolicy -Policy DC
$DCPolicy.PrivilegeRights # user privilege rights on the dc...
```

## Enumerate the Current Domain Policy

```powershell
# PowerView
$DomainPolicy = Get-DomainPolicy -Domain bufu-sec.local
$DomainPolicy.KerberosPolicy # useful for golden tickets ;)
$DomainPolicy.SystemAccess # password age/etc.
```
