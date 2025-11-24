---
id: trabajoDesligue
aliases:
  - Trabajo Web Architecture
tags: []
---
# Trabajo Web Architecture

##  Deploy AWS Server

For deploy the server you need to use AWS Academy, for that you need to access to the AWS Laboratory and click in the bottom Start Lab.

![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251118141521.png)

The AWS machine is ready when the "led" color on AWS become green.
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251118141830.png)
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123231153.png)
## Connect With SSH to the Machine

Click in AWS (the one in the images before) to access to the AWS Dashboard.
In the AWS Dashboard click in Key pairs.

![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122182546.png)

Click in Create Key pair bottom.
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123231722.png)

Write the name of your key and click in create Key pair bottom.
With the documento .pem you can access to the machine with SSH.
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122182940.png)

In the terminal you need to go to the directory that have the .pem key, and write the followin command.  Change the image's IP with your machine's IP.

![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122183651.png)
```
ssh -i daw.pem ubuntu@98.80.13.213
```

Once you connect with the machine yo will have something like this.
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122184435.png)
## Obtain Your Machine's Public IP

Go to the AWS dashboard, click in Instances (running) and click in Instances ID. You will have the next menu.
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122184115.png)

## Containers With Docker 

For install docker you need to configure the apt repositories with the following commands.

```
sudo apt update
sudo apt install ca-certificates curl 
sudo install -m 0775 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

With that you added the GPG keys

```
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

To install docker you need to run the next command.

```
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

And write this in the terminal `sudo docker run hello-world` to verify that the installation is correct.
## Add Ubuntu User in Docker

You need to use this to use docker without sudo.

Create a group called docker

```
sudo groupadd docker
```

After add the user docker to the group.

```
sudo usermod -aG docker $USER
```

You will activate the changes to groups with the following command.
```
newgrp docker
```

And there you have it, you don't need to execute docker with with sudo no more.

## Install Portainer

Portainer is a tool that help you with the management of docker containers.

For install Portainer you need to execute the following commands:

```
docker volume create portainer_data

```

```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:lts
```

Once you install Portainer is good to know if the installation actually works, for that you need to execute
this command `docker ps`.

If the command shows you this output, you did the installation fine.

```
CONTAINER ID   IMAGE                        COMMAND        CREATED         STATUS         PORTS                                                                                                NAMES
7963585688a9   portainer/portainer-ce:lts   "/portainer"   8 seconds ago   Up 8 seconds   0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp, 0.0.0.0:9443->9443/tcp, [::]:9443->9443/tcp, 9000/tcp   portainer
```

### Configuring Firewall
Before you use use Portainer you need to configure your machine's firewall. 
For configure de AWS firewall you need to go to the dashboard. In the dashboard you click in security groups
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122213912.png)

In there you click in the Scurity group ID of the security group Launch Wizard
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122214049.png)

Now you edit the in inbound rules and in the page will be open you need to click on add rule
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122214258.png)
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122214548.png)

And add the following rule: 
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122214704.png)

You need to spicify that is the port 9443 because is the port that use Portainer.

For logging In on Portainer you need to write your machine IP on the browser.
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122180331.png)

The browser may throw you this error, don't worry, click on the advance bottom
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122180643.png)
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122180743.png)

And you will have a logging screen like this: 
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122180958.png)

Write your username and password and there have your Portainer ready to use.
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122181646.png)

## Nginx

### Install Nginx

For install Nginx you gonna use the following docker commands:
```
docker pull nginx:trixie-perl
```

And with this command create the nginx docker container 
```
$ docker run --name some-nginx -v /some/content:/usr/share/nginx/html:ro -d nginx

```

And for acces to nginx dashboard we need to configure the firewall of AWS like we did before, but 
this time with port 8181 instead of the port 9443, after you did that you need to write your 
machine's IP and the port 8181. You will have a log in menu like this

![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123214600.png)
For log in nginx you only need to write an email, a name  and password. You will acces in to the dashboard
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123214847.png)

## Deploy Nginx with Stakc

Go to stacks on Portainer dashboard, and ad Stack

![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123230058.png)

And the next configuration in the web editor

```
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    environment:
      TZ: "Europe/Madrid"
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - data:/data
      - letsencrypt:/etc/letsencrypt
    networks: 
      daw:
        ipv4_address: 192.168.12.3
volumes: 
  data:
  letsencrypt:

networks:
  daw: 
    external: true
```

![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123230306.png)
## Create Containers Network

You need to go to the Portainer's dashboard and click on networks 

![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123215648.png)

And click in create new network
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123215756.png)

And create the network with the name daw, the subnet 192.16812.0/24 with the gateway 192.168.12.1. Enable the access control.
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123220216.png)

To add Portainer and Nginx to the daw network you need to go containers and click in the Portainer or Nginx container. You need to do the same steps to add both to the daw network.
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123221635.png)

Once you click in the container you need to go all down in the page, and click in join network
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123221756.png)

## Add SSL Certificate to Nginx

### Get SSL Cerficate and Key From Cloudflare
Youre need to go to SSL/TLS > Client Certificate in to the Claudflare domain dashboard
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123223653.png)
Click in create cerficate
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251122214258.png)
and create
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123223915.png)

And you obtain two files, one is the certificate and the other the key.

### Add SSL Certificate In Nginx

In Nginx dashboard you need to go to certificates
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123224412.png)

Click in add certificate > Custom certificate
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123224443.png)
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123224513.png)

And add the certificate, the key certificate and the name of your custom certificate.
![Preview](https://github.com/Cristian31310/Trabajo_Despliegue_Web_Architecture/blob/main/Pasted%20image%2020251123224720.png)

## Deploy Dinamic DNS With Cloudflare's API
