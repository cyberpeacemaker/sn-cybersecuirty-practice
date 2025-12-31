### 1. The "First Launch" Rule Creation

When you install a new app (like a game, Zoom, or Spotify) and open it for the first time, you often see a Windows prompt asking: *"Allow this app to communicate on these networks?"*

* If you click **Allow**, Windows instantly creates a new "Inbound Rule" for that specific program's `.exe` file.
* If you have **Administrator** rights, the app's installer may even add these rules silently during installation without ever showing you a prompt.

### 2. Windows Updates & System Features

Microsoft regularly adds or updates firewall rules during Windows Updates.

* **App Updates:** When an app like **Microsoft Edge** or **Teams** updates, its "fingerprint" (hash) changes. Windows will often add a new rule to match the new version so the app doesn't stop working.
* **New Features:** If Windows adds a feature like "Recall" or "Copilot," it will automatically create the necessary firewall rules to let those features talk to Microsoft's servers.
* **mDNS/Discovery:** Windows often re-enables rules for things like "mDNS" (Multicast DNS) or "Network Discovery" to ensure you can still find your printers and other PCs on your home network.

### 3. Service Host (svchost.exe) Logic

Many system rules are tied to the **NetworkService** or **LocalSystem** accounts rather than a specific file. Windows manages these rules dynamically based on which "Services" are currently running. If you start using a feature like "Remote Desktop," Windows will "poke a hole" in the firewall specifically for that service.



### Can you stop Windows from doing this?

Technically yes, but it is a "losing battle."

* **The "Block All" Setting:** You can check the box in Firewall Settings that says *"Block all incoming connections, including those in the list of allowed apps."* This makes your PC a fortress, but it will break almost all local sharing (printers, casting, etc.).
* **Registry/Group Policy:** In professional versions of Windows (Pro/Enterprise), you can disable "Local Policy Merge." This tells Windows to **ignore** rules created by apps and only follow rules set by you (the Admin).

| Action | Result |
| --- | --- |
| **New App Install** | Often adds a rule for that app's executable. |
| **Windows Update** | May refresh or "reset" core system rules. |
| **Network Change** | Switching from "Public" to "Private" activates a different set of rules. |




---

### How to see what changed (and who did it)

If you want to play detective and see when a rule was modified or added, you can check the **Event Viewer**:

1. Press `Win + R`, type `eventvwr.msc`, and hit **Enter**.
2. Navigate to: **Applications and Services Logs** > **Microsoft** > **Windows** > **Windows Defender Firewall with Advanced Security** > **Firewall**.
3. Look for **Event ID 2004** (Rule Added) or **Event ID 2005** (Rule Modified).
* The "General" tab for these events will show you exactly which program or service requested the change.