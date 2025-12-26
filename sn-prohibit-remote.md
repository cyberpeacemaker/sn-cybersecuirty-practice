Yes, Windows 11 does have a way to control remote login access, but it's a little different from Linux. In Windows, the equivalent of SSH is **Remote Desktop** (RDP), but there are also other remote login methods you might want to control, like PowerShell remoting or third-party tools like SSH (if you've installed an OpenSSH server).

Here’s how you can **disable remote login** (RDP or other remote access) on Windows 11:

### 1. **Disable Remote Desktop (RDP)**

If you're worried about **Remote Desktop Protocol (RDP)** allowing remote access to your machine, you can disable it.

#### Steps to Disable RDP (Remote Desktop):

1. Press `Windows + R` to open the **Run** dialog.
2. Type `sysdm.cpl` and press **Enter** to open **System Properties**.
3. Go to the **Remote** tab.
4. Under **Remote Desktop**, select **Don't allow remote connections to this computer**.
5. Click **Apply** and **OK**.

Alternatively, you can also disable RDP through **Settings**:

1. Open **Settings** (press `Windows + I`).
2. Go to **System** > **Remote Desktop**.
3. Turn off the toggle for **Remote Desktop**.

### 2. **Disable SSH Server (if installed)**

If you're using **SSH** (for remote access via tools like OpenSSH), you can disable the SSH server.

#### Steps to Disable SSH Server:

1. Open **Settings** (`Windows + I`).
2. Go to **Apps** > **Optional Features**.
3. Scroll down and click **Add a feature**.
4. Search for **OpenSSH Server** (if it's installed) and click **Uninstall**.

Alternatively, you can stop the SSH service using the **Services app**:

1. Press `Windows + R` and type `services.msc`.
2. In the **Services** window, look for **OpenSSH SSH Server**.
3. Right-click on it and select **Stop** to temporarily disable it or set the **Startup type** to **Disabled** to prevent it from starting automatically.

### 3. **Disable Remote PowerShell Access**

Windows 11 allows remote management via **PowerShell remoting**, which you can also disable.

#### Steps to Disable PowerShell Remoting:

1. Open **PowerShell** as Administrator.
2. Run the following command to disable remoting:

This command disables PowerShell remoting across the network.
   ```powershell
   Disable-PSRemoting -Force
   ```
Get-Service WinRM
winrm enumerate winrm/config/listener
   winrm delete winrm/config/listener?Address=*+Transport=HTTP
   winrm delete winrm/config/listener?Address=*+Transport=HTTPS

4️⃣ Restore LocalAccountTokenFilterPolicy (lock admin remote access)
```powershell
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System `
/v LocalAccountTokenFilterPolicy /t REG_DWORD /d 0 /f
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System `
/v LocalAccountTokenFilterPolicy
REG_DWORD    0x0
```

Listining port
tasklist /svc /fi "PID eq 4"


# Advanced sharing setting
off

### 4. **Disable Network Level Authentication (NLA)**

If you're looking to prevent **Remote Desktop** connections that require **Network Level Authentication (NLA)**, you can adjust the settings:

1. Open **Local Group Policy Editor** (`gpedit.msc`).
2. Navigate to **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Remote Desktop Services** > **Remote Desktop Session Host** > **Security**.
3. Look for the setting **Require user authentication for remote connections by using Network Level Authentication**.
4. Set it to **Disabled**.

### 5. **Firewall Rules to Block Remote Access**

You can also block RDP and other remote access methods using **Windows Firewall**.

#### Steps to Block RDP with Firewall:

1. Press `Windows + R`, type `wf.msc`, and press **Enter** to open **Windows Firewall with Advanced Security**.
2. In the left panel, click **Inbound Rules**.
3. Find and disable rules related to **Remote Desktop (RDP)**. There should be rules like **Remote Desktop - User Mode (TCP-In)**.
4. Right-click the rule and select **Disable Rule**.

---

### Recap:

* **Remote Desktop**: Disable through System Properties or Settings.
* **SSH (if installed)**: Uninstall via Optional Features or disable via Services.
* **PowerShell Remoting**: Use the `Disable-PSRemoting -Force` command.
* **Firewall**: Block inbound RDP connections via Windows Firewall.

Let me know if you need more info on any of these!
