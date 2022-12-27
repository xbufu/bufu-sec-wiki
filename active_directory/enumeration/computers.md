# Domain Computers

## Get List of Computers in Current Domain

```powershell
# AD Module
Get-ADComputer -Filter * -Properties *
Get-ADComputer -Filter * | Select Name

# PowerView
Get-NetComputer
Get-NetComputer -FullData
```

## Check for Live Hosts (depends on ICMP)

```powershell
# AD Module
Get-ADComputer -Filter * -Properties DNSHostName | %{Test-Connection -Count 1 -ComputerName $_.DNSHostName}

# PowerView
Get-NetComputer -Ping
```

## Information about Operating Systems

```powershell
# AD Module
Get-ADComputer -Filter 'OperatingSystem -Like "*Server 2016"' -Properties OperatingSystem | Select Name,OperatingSystem

# PowerView
Get-NetComputer -OperatingSystem "*Server 2016"
Get-NetComputer -FullData | select dnshostname,operatingsystem
```

## Get List of Sessions on Computer

```powershell
# PowerView
Get-NetSession -ComputerName "dc01.lab.local"
```

## Find Any Computers with Constrained Delegation Set

```powershell
# PowerView
Get-DomainComputer -TrustedToAuth
```

## Find All Servers that Allow Unconstrained Delegation

```powershell
# PowerView
Get-DomainComputer -Unconstrained
```

## Return the Local Groups of a Remote Server

```powershell
# PowerView
Get-NetLocalGroup SERVER.domain.local
```

## Return the Local Group Members of a Remote Server Using Win32 API Methods (faster but less info)

```powershell
# PowerView
Get-NetLocalGroupMember -Method API -ComputerName SERVER.domain.local
```

## Enumerates Computers in the Current Domain with 'outlier' Properties

```powershell
# PowerView
Get-DomainComputer -FindOne | Find-DomainObjectPropertyOutlier
```
