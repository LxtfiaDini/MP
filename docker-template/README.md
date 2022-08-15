# Docker Template Setup

## Author
Sayed Hamzah, @@xxbaemaxx

## Environment Setup
- Using VirtualBox/VMWare, go and download the Ubuntu Server 20.04 LTS ISO: https://ubuntu.com/download/server
- Follow these guides to install BOTH docker and docker-compose: 
	- Docker: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04
	- docker-compose: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04
- Once you have installed Docker, do a VM Snapshot just in case if anything messes up. You can even export this file to OVA or something so that everyone has a pre-built VM with docker and docker-compose installed (BIG BRAIN MOMENT :))

## How to get started with docker & docker-compose
You can start off by using this: https://docs.docker.com/compose/gettingstarted/

## Component Breakdown
In this template folder there are three files:
- appcode/ folder: This is where you store your HTML pages, PHP server-side code
- Dockerfile: This is the file responsible for specifying what kind of docker image that you want the docker container to be built with, along with the additional OS commands that you want to run before the container is deployed (e.g package installs, etc)
- docker-compose.yml: Configuration file for docker-compose to use when building and deploying the docker container (includes port mapping, etc)

## Deployment Instructions
1. Make sure that your server has docker and docker-compose installed.
2. In docker-compose.yml, you can set to any ports that you want by changing to:
```<any port number that you want other clients to access>:80
```
3. Deploy the application within this github folder by running: 
```docker-compose up -d
```
