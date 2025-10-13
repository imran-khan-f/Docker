# Docker
Docker is a set of platform as a service (PaaS) products that use OS-level virtualization to deliver software in packages called containers. The service has both free and premium tiers. The software that hosts the containers is called Docker Engine. It was first released in 2013 and is developed by Docker

Docker is a tool that is used to automate the deployment of applications in lightweight containers so that applications can work efficiently in different environments in isolation.

Background
Containers are isolated from one another and bundle their own software, libraries and configuration files; they can communicate with each other through well-defined channels. Because all of the containers share the services of a single operating system kernel, they use fewer resources than virtual machines.

Linux Building Blocks

Containers leverage linux kernel features cgroups and namespaces to provide resource constraints and application isolation respectively. They also use an union filesystem that enables images to be built upon common layers, making building and sharing images fast and efficient.

Note: Docker did not invent containers. For example, LXC containers (https://linuxcontainers.org/) was implemented in 2008, five years before Docker launched. That being said, Docker made huge strides in developer experience, which helped container technologies gain mass adoption and remains one the most popular containerization platforms.

Cgroups

Cgroups are a Linux kernel feature which allow processes to be organized into hierarchical groups whose usage of various types of resources can then be limited and monitored.

With cgroups, a container runtime is able to specify that a container should be able to use (for example):

Use up to XX% of CPU cycles (cpu.shares)
Use up to YY MB Memory (memory.limit_in_bytes)
Throttle reads to ZZ MB/s (blkio.throttle.read_bps_device)

Namespaces

A namespace wraps a global system resource in an abstraction that makes it appear to the processes within the namespace that they have their own isolated instance of the global resource.

Changes to the global resource are visible to other processes that are members of the namespace, but are invisible to other processes.

With namespaces, a container runtime is able to keep processes outside of the container invisible within the container or map the user inside the container to a different user on the host (among other things).

Union filesystems

A union filesystem allows files and directories of separate file systems, known as branches, to be transparently overlaid, forming a single coherent file system.

Contents of directories which have the same path within the merged branches will be seen together in a single merged directory, within the new, virtual filesystem.

This approach allows for efficient use of space because common layers can be shared. For example, if multiple containers from the same image are created on a single host, the container runtime only has to allocate a thin overlay specific to each container, while the underlying image layers can be shared. More detail on understanding the implications of these filesystem on data persistence can be found in 04-using-3rd-party-containers.

Docker Application Architecture

It is useful to break down the various components within the Docker ecosystem. The first distinction to make is between "Docker Desktop" and "Docker Engine".

Docker Desktop is an application you install on development systems that provides:

A client application:
Docker CLI (command line interface for interacting with Docker)
GUI for configuring various system settings
Credential helpers for accessing registries
Extensions (3rd party plugins)
A Linux virtual machine containing:
Docker daemon (dockerd), exposing the Docker API
(Optional) Kubernetes cluster
Docker Desktop is free for personal use, but requires a subscription for certain commercial use cases.

Docker Engine refers to a subset of those component which are free and open source and can be installed only on Linux. Specifically Docker Engine includes:

Docker CLI
Docker daemon (dockerd), exposing the Docker API
Docker Engine can build container images, run containers from them, and generally do most things that Docker Desktop but is Linux only and doesn't provide all of the developer experience polish that Docker Desktop provides.

Container image registries are not part of Docker itself, but because they are the primary mechanism for storing and sharing container images it is worth including it here. Docker runs a registry named DockerHub, but there are many other registries as well. More info on these can be found in 07-container-registries.
===========================================================================================================================
Docker Installation

server private ip and public ip

hostnamectls set-hostname "name"

apt update 

ifconfig

cat /etc/os-release

https://docs.docker.com/engine/install/ubuntu/
==============

# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
===========================

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
=====================================

docker -- version
docker info

docker file is in root: /var/lib/docker

docker ps : any process is running 

docker image ls : for all the image

docker create 

docker container run -dit --name docname -p 8005:80 nginx
-d detach
-i interact 
-t terminal
-p port

docker events: to capture events

docker container exec -it "conname" bash : to connect to the container 

docker stop "docname"
docker image : docker pull nginx:stable-alpine3.21-perl

image: is a lightweight, standalone, executable package of software 
container: is what you run from the image  
===========================

non familiar method to install docker : apt install docker.io
no compose* and buld*, then we need to install separately.

====================================================================================================
Docker Commands

graphdriver is the overlay2 which is help to create container with R/W layer from image,

docker info: give all the details with how many images, containers and etc

docker search "MySQL" : will share the results from the hub.docker.com

docket search "apache2"

docker pull "ngnix": will pull the image from the hub.docker.com

docker container run --name "name" -dit "image"

-d detach - run in background
-i interact 
-t terminal
-p port

docker image : app+os(lightweight) 
watch docker ps: help to update any container every 2 seconds

docker exec -it "docname" bash : to interact inside docker image

docker info : show details
storage layer| overlay2|thin read write layer
ping command to opt : apt install iputils-ping

docker image inspect "image name/image no": to get the version of the image 

docker logs "docname" : show what happened to docserver

docker rm "docname" "image": to delete the docserver

docker ps : show all continer running

docker ps -a : show all continer running and stopped \

-e : environment variable 

rmi: remove container image

docker container inspect "dockername": inspect the details on the container

graph driver=storage driver=overlay2

docker stats: show cpu n memory

docker run -dit --name docsql04 -e MYSQL_ROOT_PASSWORD=India@123 -p 8002:3306 MySQL

docker events: 1) pull image 2) create container 3) network attach 4) container start


mkdir project 
vi Dockerfile
i=insert 
=================
FROM
LABEL (dummy version)
RUN
RUN
RUN
WORKDIR
COPY
EXPOSE
CMD
ENTRYPOINT
=================
all the above are the docker file 
===============
FROM nginx or nginx:"version"
LABEL version="1.0" or "com.example.version=
RUN apt-get update -y
RUN apt-get install tree -y
RUN apt-get install get -y
RUN mkdir /tmp/webapp
COPY index.html /usr/share/nginx/html
EXPOSE 80
=====================
get into project folder
create the .html where the Dockerfile is present

-t = tag

docker build -t "dockerimagename" .

on different file : docker build -t "name" -f "path\filename" .

buildkit: helps to build the image

docker image history "name": to inspect the image  

https://docs.docker.com/reference/dockerfile/

docket container rm -f $(docket ps -a -q) : to remove all the running container
-q :quit
==============================================================================
docker run -dit --name docsql04 -P :80 nginx : -P is for auto port.

we cant have multiple CMD and ENTRYPOINT : if it does it will only execute the last CMD or ENTRYPOINT
ADD: its can be copied and downloaded from url and destination 
COPY: only use to copy from source to destination

docker login -u vimal 2019

personal access tokens for login

to build an image without cache: --no-cache

intermediate container for each execution on the Dockerfile.

DOCKER_BUILDKIT=0 docker built -t "name" . : to see the intermediate container

docker samples: for sample files 
=============================================================================

cmd, entrypoint, expose only when the container is running

cmd we can overwrite and entrypoint we will not overwrite
-c command
"/bin/sh" :entry point to shell

docker run -it --name "dockername" --entrypoint "/bin/sh" "image" -c "echo world"


docker image build
docker container run (container 6)

play with docker (playground):alt+enter fullscreen

for docker compose: vi compose.yml
version:
services:
networks:
volumes:
secrets:
configs:

all the above are top-level command
version: "3.0"
services:
  webapp:
    image: nginx
    ports:
      - "8006:80"
  dbapp:
    image: MySQL
    ports:
      - "3306:3306"
  mobileapp:
    build: .(current working directory for Dockerfile)
    networks:
      - myownnw
    deploy:
      mode: replicated
      replicas : "04"
networks:
  myownnetwork: {}
volumes:
secrets:
configs:

rm -rd name.yml: to remove 
https://docs.docker.com/compose/
https://docs.docker.com/reference/compose-file/


docker compose up -d: to run the compose file
docker compose down : to stop the compose file
docker compose ps : all the compose container
docker network ls : for network
bridge,host,none by default network
docker network inspect "network name"
docker compose up -d : to run the compose file while the other containers are running 

docker compose -f example.yml up -d

====================================

docker compose -f example.yml down: to down the compose container 
docker system --help
docker system prune: all dangling image, no using container, cache
docker system df: space 
docker system : to know what all we can see

docker swarm (cluster)
1) init (master) 2)join (worker)

docker swarm init : becomes master.
docker node : we can promote and demote etc

docker swarm kit
https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/

docker swarm tls
docker node ls

docker cluster services and tasks
in AWS its ELASTIC CONTAINER SERVICE
in AZURE its CONTAINER INSTANCE 

worker communicate with eachother using GOOSIP NETWORK
master communicate and data is stored in internal storage which uses RAFT CONSENSUS GROUP algorithm

SWARM MANAGER
1)API 2)Orchestrator 3)Allocator 4)Dispatcher 5)Scheduler

Worker Node 
1)worker 2)Executer 

Port 2377: TCP for communication with and between manager nodes.

Port 7946: TCP/UDP for overlay network node discovery.

Port 4789: UDP (configurable) for overlay network traffic.

To add worker to the SWARM : docker swarm init
to add manager to swarm: docker swarm join-token manager
docker swarm join-token (worker|manager) to add worker or manager

docker node ls : to see master and workers(can we seen on Manger only)

docker node : we can promote and demote etc 

docket node inspect "hostname": to get detailed information 

docker service and task
docker service ls 

docker node ls 
for swarm
docker stack deploy -c "compose file.yml" "stack name" -d 
docker stack rm "stackname": to delete 
docker service ls : give you all the stack name and the replica and details 

docker service ps "stackname": give you all the container running in master and worker.

docker stack ls : give all the name of stack and services
docker stack ps "stackname": give all the container details 

docker service create --name "name" --replicas 8 "image"(imperative way): tocreate
docker service rm "name": to delete
=========================

docker network ls : 
1) Bridge(default 172.17.0.0) = IPAM, Gives IP
2) Host
3) none
when added in cluster we get 
4) overlay
5) ipvlan
6) macvlan

enX: ethernet ip that connects internet(local machine ip )
docker0:(virtual eathnet bridge) gives the docker ip (172.17.0.2): comes in when docker is installed.


docker0 is by default virtual ethernet bridge 

RFC 1918
224.0.0.0 multicast ip 

docker network create "n/wname" :new network 

docker network inspect "n/wname": network inspect

docker network create "n/wname" --subnet 10.0.0.0/16 --gateway 10.0.0.1 -d bridge : custom n/w 

docker container run -dit --name "name" -- network "network" nginx

routing mesh for ingress: internal routing to another host if the current host is busy or not avaliable  
in swam you get network 1) ingress 2) docker_bridge


imperative (cli)and declarative (yaml file )

1) well known port 2) registered port 3) dynamic ports

docker volume ls 
docker volume create "name"
docker container "inspect"dockername"
named volume= /var/lib/docker/volume
anonamas volume 

tmpfs mount : making memory as storage, for having like cache, read and write faster.
bind mount 

volume 
mount 

Podman container file = Dockerfile
=====================
git clone "url"
