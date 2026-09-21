```
sudo nmap -sU --script ipmi-version -p 623 <DOMAIN>

#Metasploit
use auxiliary/scanner/ipmi/ipmi_version 
use auxiliary/scanner/ipmi/ipmi_dumphashes
```