###### Check Zone and ipsets
```
sudo firewall-cmd --get-active-zones

firewall-cmd --ipset=DOD_IPs --get-entries
```
###### Configure Rules
```
#multiple IP
sudo firewall-cmd --zone=public --permanent --add-rich-rule='rule family="ipv4" source address="192.168.10.0/24" port protocol="tcp" port="22" accept'

#single IP and port range
sudo firewall-cmd --zone=public --permanent --add-rich-rule='rule family="ipv4" source address="192.168.10.100" port protocol="tcp" port="8000-8080" accept'

```
###### Apply Changes
```
sudo firewall-cmd --reload
```
###### List all rules in public zone
```
sudo firewall-cmd --zone=public --list-all
```
###### Remove a rule
```
sudo firewall-cmd --zone=public --permanent --remove-rich-rule='rule family="ipv4" source address="192.168.10.0/24" port protocol="tcp" port="22" accept'
```
###### Add range to existing zone
```
sudo firewall-cmd --zone=AdminNet --permanent --add-source=10.50.10.0/24	#--add-port, --add-service
```
###### Remove range from existing zone
```
sudo firewall-cmd --zone=AdminNet --permanent --remove-source=10.50.10.0/24	#--remove-port, --remove-service
```
###### Create new ipset
```
firewall-cmd --permanent --ipset=DOD_IPs --add-entry=10.10.10.100
firewall-cmd --reload
```