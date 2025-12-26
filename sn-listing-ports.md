Get-NetTCPConnection | 
    Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, State, 
                  @{Name='ProcessName';Expression={(Get-Process -Id $_.OwningProcess).ProcessName}}, 
                  OwningProcess |
    Sort-Object OwningProcess

Get-NetTCPConnection | Sort-Object OwningProcess