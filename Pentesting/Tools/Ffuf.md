###### Directory Fuzzing
```
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ
ffuf -w <SNIP> -u http://SERVER_IP:PORT/FUZZ
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ
```
###### Page Fuzzing
```
ffuf -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ <SNIP>
ffuf -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://academy.htb:31302/indexFUZZ
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt:FUZZ -u http://academy.htb:31302/FUZZ.php
```
###### Recursive Fuzzing
```
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt:FUZZ -u http://academy.htb:31302/FUZZ -recursion -recursion-depth 1 -e .php,.php7 -v
```
###### Sub-Domain Fuzzing
```
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://FUZZ.academy.htb:31302/
```
###### Vhost Fuzzing 
```
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:31302/ -H 'Host: FUZZ.academy.htb' -fs 985
```
###### Filtering Results
```
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs 900
```
###### Parameter Fuzzing - GET
```
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://faculty.academy.htb:31302/courses/linux-security.php7?FUZZ=key -fs xxx
```
###### Parameter Fuzzing - POST
```
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
curl http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'
```
###### Value Fuzzing
```
for i in $(seq 1 1000); do echo $i >> ids.txt; done
ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:30314/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs 768
curl http://admin.academy.htb:30314/admin/admin.php -X POST -d 'id=73' -H 'Content-Type: application/x-www-form-urlencoded'
```