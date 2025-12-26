netstat -abno
netstat -abno | findstr ESTABLISHED

# Services

### Normal & required (do NOT disable blindly)

| Port        | Meaning                                              |
| ----------- | ---------------------------------------------------- |
| 135         | RPC (Windows core service)                           |
| 445         | SMB (file sharing, Windows networking)               |
| 139         | NetBIOS (legacy networking)                          |
| 49664–49774 | Dynamic RPC ports (Windows uses these automatically) |
| 7680        | Windows Update / Delivery Optimization               |
| 5040        | Windows services (varies by version)                 |


## How to reduce unnecessary listening services (safe way)

### 1️⃣ Turn off Windows services you truly don’t use

Press **Win + R → `services.msc`**

Common candidates (ONLY if you don’t use them):

| Service               | Safe to disable if… |
| --------------------- | ------------------- |
| **Remote Registry**   | Almost always       |
| **Fax**               | Almost always       |
| **Bluetooth Support** | No Bluetooth        |
| **Print Spooler**     | No printer          |
| **Xbox services**     | Don’t use Xbox      |
| **Windows Search**    | Don’t want indexing |

⚠️ **Do NOT disable**:

* RPC services
* Windows Update
* DHCP Client
* Network Location Awareness

