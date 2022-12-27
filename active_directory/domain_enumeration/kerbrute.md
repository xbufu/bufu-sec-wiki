# Kerbrute

## Overview

- Get precompiled binary for OS from https://github.com/ropnop/kerbrute/releases
- Does not trigger failed log on event
- Brute-force by sending only a single UDP frame to the KDC
- Enumerate users on the domain from a wordlist

## User Bruteforcing

```bash
# Username enumeration
kerbrute userenum --dc CONTROLLER.local -d CONTROLLER.local User.txt
```
