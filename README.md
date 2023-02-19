# Assignment #1

`Vmware Workstation` 

# Assignment #2

1.  I used `Virtual Network Editor` to add Virtual Network and i named it `VMnet2` with Host-only Type

![Screenshot_20230220_020250](https://user-images.githubusercontent.com/107811500/219980597-0b1affd1-fa92-455b-946f-31d4e4939dfc.png)

2.  Install Pfsense Iso image and then Create Virtual machine 
3.  Create New Network Adapter 2 as shows in image 

![Screenshot_20230220_122210](https://user-images.githubusercontent.com/107811500/219975977-619695a5-6f45-4142-8354-95990193c374.png)

4.  lunch the Virtual Machine 

![Screenshot_20230220_121544](https://user-images.githubusercontent.com/107811500/219976076-b58b9b83-c45a-4e0a-8646-61bb0d9a58ce.png)

5.  `WAN` (eM0) DHCP4 : 172.29.174.144/20 
6.  `LAN` (eM1) GW    : 192.168.1.1/24

![Screenshot_20230220_122951](https://user-images.githubusercontent.com/107811500/219976374-59758cc6-c1ba-4170-8501-ed9d3eeaf4eb.png)

#  Assignment 3#
1.  `ubuntu` server1 with ipv4 `192.168.1.100` pinging `192.168.1.1` 

![Screenshot_20230220_010311](https://user-images.githubusercontent.com/107811500/219977916-c5291273-82f7-4811-8234-5395779735b3.png)

2.  `ubuntu` server2 with ipv4 `192.168.1.101` pinging `192.168.1.1`

![Screenshot_20230220_010311](https://user-images.githubusercontent.com/107811500/219977956-ee41eb45-7a28-4a66-8822-5a763343f010.png)


3.  `Centos7` server3 with ipv4 `192.168.1.104` pinging `192.168.1.1`

![Screenshot_20230220_010350](https://user-images.githubusercontent.com/107811500/219977973-296413f9-de07-4a81-8acd-15bb3e76ec65.png)


# Assignment #4
## Hardening bash script for ubuntu 

``` 
#!/bin/bash

#Disable unnecessary services
systemctl disable avahi-daemon.service
systemctl disable cups.service
systemctl disable isc-dhcp-server.service
systemctl disable slapd.service
systemctl disable nfs-server.service

#Configure user accounts and groups
passwd -d root
useradd -m -s /bin/bash user1
usermod -aG sudo user1
useradd -m -s /bin/bash user2
usermod -aG sudo user2
useradd -m -s /bin/bash user3
usermod -aG sudo user3

#Enforce strong password policies
sed -i 's/password    requisite     pam_cracklib.so retry=3 minlen=8 difok=3 ucredit=-1 lc>

#Restrict access to sensitive files and directories
chmod 600 /etc/shadow
chmod 600 /etc/gshadow
chmod 644 /etc/passwd
chmod 644 /etc/group
chmod 644 /etc/security/access.conf
chmod 644 /etc/security/group.conf
chmod 644 /etc/security/passwd
chmod 600 /etc/ssh/sshd_config

# Configure logging and auditing
sed -i 's/#SyslogFacility AUTH/SyslogFacility AUTH/g' /etc/ssh/sshd_config
sed -i 's/#LogLevel INFO/LogLevel INFO/g' /etc/ssh/sshd_config
systemctl restart sshd.service
```


## Hardening bash script for Centos7

```
#!/bin/bash

#Disable unnecessary services
systemctl disable avahi-daemon.service
systemctl disable cups.service
systemctl disable dhcpd.service
systemctl disable slapd.service
systemctl disable nfs-server.service

#Configure user accounts and groups
passwd -d root
useradd -m -s /bin/bash user1
usermod -aG wheel user1
useradd -m -s /bin/bash user2
usermod -aG wheel user2
useradd -m -s /bin/bash user3
usermod -aG wheel user3

#Enforce strong password policies
sed -i 's/password    requisite     pam_cracklib.so retry=3 minlen=8 difok=3 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1/password    requisite     pam_cracklib.so retry=3 minlen=12 difok=4 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1 enforce_for_root/g' /etc/pam.d/system-auth

#Restrict access to sensitive files and directories
chmod 600 /etc/shadow
chmod 600 /etc/gshadow
chmod 644 /etc/passwd
chmod 644 /etc/group
chmod 644 /etc/security/access.conf
chmod 644 /etc/security/group.conf
chmod 644 /etc/security/passwd
chmod 600 /etc/ssh/sshd_config

#Configure logging and auditing
sed -i 's/#SyslogFacility AUTH/SyslogFacility AUTH/g' /etc/ssh/sshd_config
sed -i 's/#LogLevel INFO/LogLevel INFO/g' /etc/ssh/sshd_config
systemctl restart sshd.service
```

# Assignment #5

1. `docker-compose.yml`
```
version: '3'
services:
  grafana:
    image: grafana/grafana:latest
    ports:
      - "3001:3000"
    restart: always
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9901:9090"
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    restart: always
```

2.  `prometheus.yml`

```
global:
  scrape_interval:     15s
  evaluation_interval: 15s
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']

``` 

![Screenshot_20230220_013122](https://user-images.githubusercontent.com/107811500/219979287-097c0253-d59f-4c87-a9be-dff52a6f2a83.png)

![Screenshot_20230220_013138](https://user-images.githubusercontent.com/107811500/219979293-0fde5a66-7d86-4062-aa0f-989bb5a91271.png)


# Assignment #6

## exporters container for `ubuntu Server` and Grafana Dashboard 

![Screenshot_20230220_013801](https://user-images.githubusercontent.com/107811500/219979806-6010ceb2-d2b3-4a50-b330-5a5e34509382.png)

![Screenshot_20230220_014219](https://user-images.githubusercontent.com/107811500/219979811-c2a71212-21d8-4cf9-8e14-87ec29b8be75.png)

## exporters container for `Centos7 Server` and Grafana Dashboard 

![Screenshot_20230220_014243](https://user-images.githubusercontent.com/107811500/219980112-c231fc5f-f5a0-4271-83b8-0d87dfae3f81.png)

![Screenshot_20230220_013841](https://user-images.githubusercontent.com/107811500/219980123-32a5689f-453d-4e7f-982b-f0317e75d1aa.png)

# Assignment #7

## Dockerfile to create HTML static page 

```
FROM nginx:alpine

# Copy index.html to the container
COPY index.html /usr/share/nginx/html

# Expose port 1309
EXPOSE 1309

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
```

![Screenshot_20230220_015644](https://user-images.githubusercontent.com/107811500/219980346-4b50b659-b5bf-48da-b23f-79e20b2e894f.png)


# Assignment #8 

## Create Dockerfile that writes the numbers from 1 to 10 on a text file named numbers.txt and maps the file to the host directory /opt when the docker-compose up command is run:

```
FROM alpine:latest

RUN apk add --no-cache bash

CMD ["bash", "-c", "seq 1 10 > /data/numbers.txt && tail -f /dev/null"]
```

## Create docker-compose.yml file that maps the /data/numbers.txt file in the container to the /opt directory on the host:

```
version: '3'

services:
  numbers:
    build: .
    volumes:
      - type: bind
        source: /opt
        target: /data
```

![Screenshot_20230220_020139](https://user-images.githubusercontent.com/107811500/219980554-83623dcf-835e-4f9e-8437-846d109d7087.png)



