https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/configure

- NIC

- Firewall
- DNS, Dynmaic Keyword, DoH, **zero trust DNS**
- VPN
- Random MAC

```shell
Get-DnsClientCache

Get-Content -Path "C:\Windows\System32\drivers\etc\hosts"
```

```shell
microsoft.com
$fqdn = 'learn.microsoft.com'
$id = '{' + (new-guid).ToString() + '}'
New-NetFirewallDynamicKeywordAddress -id $id -Keyword $fqdn -AutoResolve $true
New-NetFirewallRule -DisplayName "allow $fqdn" -Action Allow -Direction Outbound -RemoteDynamicKeywordAddresses $id

Get-NetFirewallDynamicKeywordAddress -AllAutoResolve

Get-NetFirewallDynamicKeywordAddress -AllAutoResolve |`
ForEach-Object {
  if(!$_.Keyword.Contains("*")) {
    Write-Host "Getting" $_.Keyword
    resolve-dnsname -Name $_.Keyword -DNSOnly | out-null
  }
}
```

If using nslookup.exe, you must create an outbound firewall rule when using the block all outbound posture. Here's the command to create the outbound rule for nslookup.exe:

```shell
$appName = 'nslookup'
$appPath = 'C:\Windows\System32\nslookup.exe'
New-NetFirewallRule -DisplayName "allow $appName" -Program $appPath -Action Allow -Direction Outbound -Protocol UDP -RemotePort 53
```