###### Enumeration
```
sudo nmap --script nfs* <IP> -sV -p111,2049
showmount -e <IP>
```
###### Mounting
```
mkdir <NFS>
sudo mount -t nfs <IP>:/ ./<NFS>/ -o nolock
cd <NFS>
tree .
```
###### Umount
```
sudo umount ./<NFS>
```
