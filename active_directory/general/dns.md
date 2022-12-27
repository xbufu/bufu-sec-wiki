# Dns

## General

- Active Directory relies on DNS
- Locate machines and resources on the same domain

## Setup DNS

```powershell
# Install normal DNS server
Install-WindowsFeature DNS

# Register DNS records
cmd /c ipconfig -registerdns
```
