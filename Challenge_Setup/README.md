# Container Challenges Setup

## Author
Ze Xiang (Zac) Ng, @@zzDFHJ8

## Instructions to replicate Setup
1. Complete the docker-compose guide from digitalocean first(Just step 1) (Dir -> MP/docker-template/README.md > Environment Setup)
2. Download/Pull my docker-compose.yml file
3. Change the yml file according to your own needs
   - test
  


## Docker compose file (Networking)
1. To understand networking watch 1. [NetworkChuck](https://www.youtube.com/watch?v=bKFMS5C4CG0&t=1202s&ab_channel=NetworkChuck) Else 2. The [Digital Life](https://www.youtube.com/watch?v=5grbXvV_DSk&ab_channel=TheDigitalLife)
2. To understand the comparsion between docker run commands and docker compose file [Skip to 1:23](https://www.youtube.com/watch?v=MVIcrmeV_6c&ab_channel=TechWorldwithNana)

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
