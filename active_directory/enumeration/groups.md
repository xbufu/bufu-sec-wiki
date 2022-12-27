# Domain Groups

## Get All Groups in Current Domain

```powershell
# AD Module
Get-ADGroup -Filter * | Select Name
Get-ADGroup -Filter * -Properties *

# PowerView
Get-NetGroup
Get-NetGroup -FullData
```

## Get Information about Groups in other Domain

```powershell
Get-NetGroup -Domain lab.local
```

## Get All Groups Containing the Word "admin" in Group Name

```powershell
# AD Module
Get-ADGroup -Filter 'Name -Like "*admin*"' | Select Name

# PowerView
Get-NetGroup "*admin*"
```

## Get Information about Specific Group

```powershell
Get-NetGroup -FullData "Domain Admins"
```

## Get All Members of Domain Admins Group

```powershell
# AD Module
Get-ADGroupMember -Identity "Domain Admins" -Recursive

# PowerView
Get-NetGroupMember -GroupName "Domain Admins" -Recurse
```

## Get List of Enterprise Admins, only Available from Forest Root

```powershell
Get-NetGroupMember -GroupName "Enterprise Admins" -Domain lab.local
```

## Get Group Membership for a User

```powershell
# AD Module
Get-ADPrincipalGroupMembership -Identity student1

# PowerView
Get-NetGroup -UserName "student1"
```

## List All Local Groups on a Machine (needs Administrator Privileges on Non-dc Machines)

```powershell
Get-NetLocalGroup -ComputerName dc.lab.local -ListGroups
```

## Get Members of All Local Groups on a Machine (needs Administrator Privileges on Non-dc Machines)

```powershell
Get-NetLocalGroup -ComputerName dc.lab.local -Recurse
```

## Find Linked DA Accounts Using Name Correlation

```powershell
Get-DomainGroupMember 'Domain Admins' | %{Get-DomainUser $_.membername -LDAPFilter '(displayname=*)'} | %{$a=$_.displayname.split(' ')[0..1] -join ' '; Get-DomainUser -LDAPFilter "(displayname=*$a*)" -Properties displayname,samaccountname}
```

## Find Any Machine Accounts in Privileged Groups

```powershell
Get-DomainGroup -AdminCount | Get-DomainGroupMember -Recurse | ?{$_.MemberName -like '*$'}
```

## Enumerate All Groups that Don't Have a Global Scope, Returning just Group Names

```powershell
Get-DomainGroup -GroupScope NotGlobal -Properties name
```
