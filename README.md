# Wireguard Install
## Installing Docker on Ubuntu

```
sudo apt update
```

* install proper https packages

  ```
  sudo apt install apt-transport-https ca-certificates curl software-properties-common
  ```

* install gpg key for the Docker repo

  ```
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  ```

* add the Docker APT repo 

  ```
  sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
  ```

* Run upate command again

  ```
  sudo apt update
  ```

* install Docker

  ```
  sudo apt install docker-ce docker-ce-cli containerd.io
  ```
* check docker is running (if it says "active" then everything is Gucci)

  ```
  systemctl is-active docker
  ```

## Installing and Configuring WordPress
* Note: I had to use sudo in front of all these because I did not add users to a docker group

* Install Docker Compose

  ```
  sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  ```

* Change Executable Bits

  ```
  sudo chmod +x /usr/local/bin/docker-compose
  ```

* Check the installation

  ```
  docker-compose --version
  ```

## Wireguard Install
* create directory
``` 
mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
nano ~/wireguard/docker-compose.yml
```
* copy and paste this into the docker-compose.yml file
```
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Hong_Kong
      - SERVERURL=1.2.3.4
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
```
  * Change the "TZ" to your preferred timezone code
  * Change "ServerURL" to your server IP address (found on the Digital Ocean droplet dashboard)
  * Add and remove your peers
  * save the file
  
* Start wireguard
```
cd ~/wireguard/
docker-compose up -d
```
## Create Tunnel on PC
* Navigate to conf file from ~/wireguard/config/peer_pc1
```
nano peer_pc1.conf
```
* Copy and paste the contents into a new tunnel on the desktop app

## Create Tunnel on phone
* Run this in ~/wireguard
```
docker-compose logs -f wireguard
```
* Scan the QR code
