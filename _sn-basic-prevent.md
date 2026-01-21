- arp, dhcp
- uPnP
- mDNS, DNS-SD
- WD-Discovery

- WiFi-Direct
- dns hijacking, dns leak (Tranparent DNS Proxies)
- session hijacking

- telnet

Port,Status,Risk Level,Recommendation
5353,Open,Medium,"Disable mDNS or switch to ""Public"" network profile."
1900,Open,Low/Medium,Disable SSDPSRV service if not using media sharing.
3702,Closed,Safe,No action needed; service is currently inactive.

# mDNS
```powershell
Stop-Service -Name "Dnscache" -Force
# Note: This is aggressive as it stops the main DNS cache. 
# Better to disable via Group Policy:
# Administrative Templates > Network > DNS Client > Turn off LLMNR
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters" -Name "EnableMDNS" -Value 0 -PropertyType DWORD -Force

New-NetFirewallRule -DisplayName "Block mDNS UDP 5353" -Direction Inbound -Action Block -Protocol UDP -LocalPort 5353
New-NetFirewallRule -DisplayName "Block mDNS UDP 5353" -Direction Outbound -Action Block -Protocol UDP -LocalPort 5353
```

# SSDP (used for UPnP/Discovery)
```powershell
Stop-Service -Name "SSDPSRV"
Set-Service -Name "SSDPSRV" -StartupType Disabled

Stop-Service -Name "SSDPSRV" -Force
Set-Service -Name "SSDPSRV" -StartupType Disabled
# For WS-Discovery
Stop-Service -Name "FDResPub" -Force
Set-Service -Name "FDResPub" -StartupType Disabled
```
# LLMNR
```powershell
New-Item "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\DNSClient" -Force
New-ItemProperty "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\DNSClient" -Name "EnableMulticast" -Value 0 -PropertyType DWORD -Force
```
# Multicase
```powershell
# Block all outbound traffic to Multicast ranges
New-NetFirewallRule -DisplayName "Block Outbound Multicast" -Direction Outbound -Action Block -RemoteAddress "224.0.0.0/4"
# Block all inbound traffic from Multicast ranges
New-NetFirewallRule -DisplayName "Block Inbound Multicast" -Direction Inbound -Action Block -RemoteAddress "224.0.0.0/4"
```
2. Common "Shouting" Services in Windows
Because svchost is just a "container" for many services, different tasks use these IPs. Here are the most common culprits you’ll see in a netstat or Get-NetUDPEndpoint check:

224.0.0.251 (mDNS): This is the Multicast DNS we discussed. Windows uses it to find printers, smart TVs, and IoT devices.

224.0.0.252 (LLMNR): This is the "fallback" name discovery. If a standard DNS fails, svchost uses this IP to ask everyone on the LAN, "Does anyone know who PC-KITCHEN is?"

239.255.255.250 (SSDP): This is the Simple Service Discovery Protocol. It’s the "engine" behind UPnP. If you see this, your computer is likely looking for a media server, a router's status page, or a smart device.