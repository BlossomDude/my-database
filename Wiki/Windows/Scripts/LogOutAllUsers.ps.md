
``` Powershell
# Скрипт разрывает все RDP соеденения на Windows Server.
# Очень полезно когда пользователи работают на сервере и не выходят из учеток а просто закрывают окно rdp.
Get-TSSession -ComputerName Work-PC -filter {$_.sessionID -ne 0 -AND $_.sessionID -ne 1 -AND $_.sessionID -ne 65536}| Stop-TSSession -Force
```
