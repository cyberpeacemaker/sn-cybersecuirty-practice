# 0.
- router (support wifi7, WPA3)

- WHOIS

# 1.PC

## Windows Secuirty
- Virus > Virus & threat protecion setting > Controlled folder access
- Account
- Firewall
    - Profile Incoming
    - Advanced (rule)
- App
- Device > Core isolation
- Setting > Notification

## Network & Internet
- https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/configure
- Random MAC
- NIC [uncheck adapter items , WINS > uncheck LMHOSTS and NetBIOS]
- DNS, [Dynmaic Keyword, DoH, **zero trust DNS**]
- advanced sharing setting
- always on VPN, secure network

## Remote
- `Disable-PSRemoting -Force`
- view advanced system setting

## Setting & Policy
- backup and sync disable
- rename and set pwd for built-in administrator
```shell
# This renames the account from 'Administrator' to something random like 'Ghost_Admin'
wmic useraccount where name='Administrator' rename 'Ghost_Admin'
net user Administrator *
```

- [setting, control panel, task manager]
- [system Configuration, view advanced system, system information]
- Network and Sharing Center
- Turn off Media Streaming

```
compmgmt.msc
msinfo32.exe
```

---

# 2. Browsing

## Habit
Lock (Windoes + L)
Logout

## Browser

## Email