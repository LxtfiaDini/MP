# Container Challenges Setup

## Author
Ze Xiang (Zac) Ng, @@zzDFHJ8

> **_NOTE:_** 
> Please test and understand what you are doing don't just blindly copy!! (Each Container has it's own structured script files and configurations for docker compose file)
## Instructions to replicate Setup
1. Complete the docker-compose guide from digitalocean first(Just step 1) (Dir -> MP/docker-template/README.md > Environment Setup)
2. Download/Pull my docker-compose.yml file
3. Change my yml file according to your own needs [(Compose Specification)](https://docs.docker.com/compose/compose-file/)
   - `image` include your own docker container image here
   - `ports` still unsure if there's a need to expose the port but export the port according to ur serivces
   - `command` it's necessary for our case and please include your own startup.bat script file in your container to startup the services required for that challenge
   - `networks` `ipv4_address` add according to the new network adapter added network in your vm
   - `networks(top-level element(same level as services))`
      - add your new network adapter first in your vm
      - `driver_opts` add according to VM's ethernet interface name that points to your new network
      - `config` `subnet` change accordingly (we don't need to reserve any ip addresses actually so `ip_range` not that impt)
4. Pull my docker container image if needed
> **_NOTE:_**
>Best is you find our own images search up exisiting ones (Eg. [LAMP Stack docker container](https://hub.docker.com/r/mattrayner/lamp) Or Convert CTF VM to container images)
   - `docker pull zcloudyx/ubuntu_apache2`
5. Add this line to `crontab -e` or `/var/spool/cron/crontabs` (auto run compose on reboot)
   - `@reboot sleep 60 && /usr/local/bin/docker-compose -f ~/compose-demo/docker-compose.yml up -d`

## Docker compose file (Networking)
1. To understand networking watch 1. [NetworkChuck](https://www.youtube.com/watch?v=bKFMS5C4CG0&t=1202s&ab_channel=NetworkChuck) Else 2. The [Digital Life](https://www.youtube.com/watch?v=5grbXvV_DSk&ab_channel=TheDigitalLife)
2. To understand the comparsion between docker run commands and docker compose file [Skip to 1:23](https://www.youtube.com/watch?v=MVIcrmeV_6c&ab_channel=TechWorldwithNana)
3. Refer to [Compose Specification](https://docs.docker.com/compose/compose-file/) if needed

## Example of Macvlan in compose file
```
version: "2.1"

services:
  nginx_10:
    image: linuxserver/nginx
    container_name: nginx_macvlan_10
    environment:
      - TZ=Americas/Los_Angeles
    ports:
      - 80:80
      - 443:443
    restart: unless-stopped
    mac_address: 02:42:c0:a8:84:22
    networks: 
      nginx_vlan:
        ipv4_address: 192.168.1.10

  nginx_45:
    image: linuxserver/nginx
    container_name: nginx_macvlan_45
    environment:
      - TZ=Americas/Los_Angeles
    ports:
      - 80:80
      - 443:443
    restart: unless-stopped
    mac_address: 02:42:c0:a8:84:23
    networks: 
      nginx_vlan:
        ipv4_address: 192.168.1.45

networks:
  nginx_vlan:
    driver: macvlan
    driver_opts:
      parent: eth0
    ipam:
      driver: default
      config:
        - subnet: 192.168.1.0/24
          gateway: 192.168.1.1

```
