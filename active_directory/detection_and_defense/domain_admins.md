# Domain Admins

## General Mitigations

- Do not allow or limit login of DAs to any other machine other than the Domain Controllers
- If logins to some servers are necesarry, do not allow other administrators to login to that machine
- Never run a service with Domain Admin privileges as it makes many credential theft protections useless in case of a service account

## Temporary Group Membership

- Temporarily add user to a group
- Requires `Privileged Access Management` Feature to be enabled which can't be turned off later

```powershell
Add-ADGroupMember -Identity 'Domain Admins' -Members newDA -MemberTimeToLive (New-TimeSpan -Minutes 20)
```
