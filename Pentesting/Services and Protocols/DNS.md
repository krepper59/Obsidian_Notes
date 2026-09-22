###### Zone Transfer
```
dig AXFR @<HOSTNAME/IP> <HOSTANEM/IP>
fierce --domain <DOMAIN>
```
###### Subdomain Enumeration
```
#Subfinder
./subfinder -d <FQDN> -v 

#Subbrute
git clone https://github.com/TheRook/subbrute.git >> /dev/null 2>&1
cd subbrute
echo "<NS_FQDN>" > ./resolvers.txt
./subbrute.py <FQDN> -s ./names.txt -r ./resolvers.txt

#Built-in
nslookup <FQDN>
host <FQDN>
```
###### Subdomain Bruteforce
```
dnsenum --dnsserver <IP> --enum -p 0 -s 0 -o <OUTPUT>.txt -f <WORDLIST>
dnsenum --enum <FQDN> -f <WORDLIST> 
gobuster vhost -u http://<FQDN>:31664 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain
```
###### DNS Cache Poisoning
```
#map target domain name to attacker IP
nano /etc/ettercap/etter.dns

#start ettercap and scan for hosts
#add the target IP to target 1 and default gateway to target 2
#dns_spoof with Plugins > manage plugins
```
###### Dig
```
dig any <FQDN> @<IP> #Any - all available records
dig soa <URL> #SOA - dns zone info and admin email
dig ns <FQDN> @<IP> #NS - search for other nameservers
dig CH TXT version.bind <IP> #CH - query DNS server version
dig AXFR @<NS_FQDN> <FQDN> #zone transfer, shows zone file
```
###### Find Hidden Hosts
```
for i in {1..254}; do dig -x 10.10.110.$i @10.10.110.3 +short; done
```
###### Find virtual hosts
```
gobuster vhost -u http://<URL>:31664 -w <WORDLIST> --append-domain
```
