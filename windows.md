- backup and sync disable
- rename and set pwd for built-in administrator
```shell
# This renames the account from 'Administrator' to something random like 'Ghost_Admin'
wmic useraccount where name='Administrator' rename 'Ghost_Admin'
net user Administrator *
```

# Setting
- [setting, control panel, task manager]
- [system Configuration, view advanced system, system information]
- Network and Sharing Center
- Turn off Media Streaming

```
compmgmt.msc
msinfo32.exe
```

# Concept
- permission 
- Volume Shadow copy Service (VSS)
- [file system NTFS] 
- share
- In Windows, any share name ending **with a $ (dollar sign) is a hidden share**

---

### 1. The "Dual-Token" Reality

Even though you are an administrator, Windows does not give you "full power" all the time. When you log in, Windows gives you two "tokens": a **Standard User** token and an **Administrator** token.

* By default, you use the **Standard User** token.
* You only switch to the **Administrator** token when you see a UAC (User Account Control) prompt.
* **Policy** is what defines how that process works. For example, a policy can determine if you are asked for a password, just a "Yes" click, or if UAC is disabled entirely (which is very dangerous).

### 2. Guardrails Against Malware

If you are the only user and an administrator, any malware that manages to run under your account *also* gets administrative potential. Local Security Policies act as "internal walls" to limit this:

* **Software Restriction Policies:** Can prevent unauthorized scripts or unknown `.exe` files from running in your `Downloads` folder, even if you accidentally double-click them.
* **User Rights Assignment:** Policies define who can perform "System-level" actions (like changing the system time or backing up files). By default, these are restricted so that even a compromised admin account can't easily destroy the OS kernel.

### 3. Hardening the "Built-in Administrator"

There is a "ghost" account on your PC called the **Built-in Administrator** (it has no password and is disabled by default).

* **Policy** is the only way to ensure this account remains disabled or renamed, preventing hackers from using it as a "backdoor" if they get local access to your machine.

---

### Comparison: Why Policy Still Matters

| Scenario | Without Policy Control | With Policy Control |
| --- | --- | --- |
| **Malware Attack** | Malware uses your admin rights to disable your antivirus and encrypt your drive. | Policy prevents any application from running unless it's in `C:\Program Files`. |
| **Network Security** | Your PC responds to any incoming request, potentially exposing your files. | Policy forces "Inbound Traffic" to be blocked unless specifically whitelisted. |
| **Remote Access** | Anyone who guesses your password can Remote Desktop into your machine. | Policy restricts "Log on through Remote Desktop" to only specific hardware IDs. |