### Runas
```
runas /netonly /user:<DOMAIN>\<USER> powershell
```
### PSCredential
```
$password = ConvertTo-SecureString '<PASS>' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('<DOMAIN>\<USER>', $password)
```