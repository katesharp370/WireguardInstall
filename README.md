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
