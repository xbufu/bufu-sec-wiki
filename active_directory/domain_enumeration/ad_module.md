# AD Module

## Setup

### With Internet Access

```powershell
iex (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/samratashok/ADModule/master/Import-ActiveDirectory.ps1');Import-ActiveDirectory
```

### Without Internet Access

```powershell
# If computer has no internet access, download repository
Import-Module .\ADModule\Microsoft.ActiveDirectory.Management.dll -Verbose
Import-Module .\ADModule\ActiveDirectory\ActiveDirectory.psd1
```

### Check if Module Has Been Imported Correctly

```powershell
Get-Command -Module ActiveDirectory
```
