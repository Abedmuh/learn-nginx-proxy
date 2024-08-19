# Preresquite
Project Welcome ASDP Infrastrcture IT Support.
before we configure our theres a software that u need to install or download

- Hypervisor(Vmware,Vrtualbox,UTM)
- Docker
- OS Image (in this case we will using Centos9 as Guest OS for The Hypervisor)
# Steps
Im using Arch btw 
so all the step probably different 
## Hypervisor Installation
installing hypervisor just go terminal and type 
```
yay -S vmware
```
if things go wrong just go to [Click Me](https://support.broadcom.com/group/ecx/productfiles?subFamily=VMware%20Workstation%20Player&displayGroup=VMware%20Workstation%20Player%2017&release=17.5.2&os=&servicePk=520446&language=EN) and download from there
## OS Installation

Click the file > new VM 
Then follow all the instruction use the reccomended setting
>makesure it connected to network if cant type on your linux 
```
sudo systemctl start vmware-networks
```

## Docker Installation

to make it easy use pacman yum type

```
sudo yum install docker
```
for make it sure ``docker --version``
then dont forget to install docker compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
dont forget follow the lastest version of docker-compose

## Services Installation

### Webserver

to install webserver use docker-compose.yml that provided on this repo.then type

```
docker-compose up --build
```
actually ``up`` is enough but ``--build`` for restart the docker if there something changed

### Reverse Proxy

access the localhost or hypervisor ip address for example type in your browser ``192.168.xxx.xxx:<ports>`` here port list
- 80 : nginx proxy manager
- 81 : admin nginx
- 443 : forwarded and certified nginx

**create custom domain**
on your laptop go to ./etc/host add
```
<ip host> <custom-domain such example.com>
```

**forwarding nginx to nginx proxy manager**  
login to port `80` the use `admin@example.com` as username and `changeme` as password.  
go to host add host like this setting
| Config | Value |
|-|-|
| hostname | example.com| 
| protocol | Http| 
| Hostforwarded | name_of_container |
| Port | 80 |

**creating Custom SSL Certificate**  
in your arch install `openssl` then create your custom certs
```
mkdir certsssl
cd certsssl
openssl genpkey -algorithm RSA -out server.key -aes256  
openssl req -new -x509 -key server.key -out server.crt -days 365
```

follow the instruction, remember passphrase and your name of certs if forgot type
```
openssl x509 -in ./certs/server.crt -text -noout
```

**Register custom SSL Certificate to nginx proxy manager**  
to register just go tab SSL Certificate > add custom Certificate.
upload all te certificate that need to upload such key and stuff


