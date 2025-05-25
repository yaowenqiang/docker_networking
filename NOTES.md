# Docker Networking

> DCA(Docker Certificate Associate)

> https://training.mirantis.com/certification/dca-certification-exam/
> https://kodekloud.com/wp-content/uploads/2021/05/Docker-Certified-Associate.pdf
> https://www.5axxw.com/wiki/content/f9g6hx

> docker UCP
> https://github.com/collabnix/dockerlabs

> https://github.com/studygolang/GCTT

> https://studygolang.com/articles/23458 (Docker 参考架构：设计可扩展、可移植的 Docker 容器网络)

## Native Network Drivers

+ bridge
+ host
+ overlay
+ macvlan
+ none


### Bridge

+ Connects container to the LAN and other containers
  + The default network type
  + Great for most use cases

### host

+ Remove network isolation between container and host
  + Only one container (or application on the host) can use a port at the same time
  + Useful for specific applications, such as a management container that you want to run on every host

### Overlay

+ Connect ultiple Docker hosts(and their containers) together and enable swarm
  + Only avaialble with Docker EE and Swarm enabled
  + Multihost network using VXLAN

### Macvlan

+ Assign a MAC address, appear as physical host
  + Clones host interfaces to create virtual interfaces, avaialble in the container
  + Supports connecting to VLANs

### None

+ Connects the container to an isolated network with only that container on it
  + Container cannot communicate with any other networks or networked devices

## Third-Party Network Plugins

Examples on the Docker Store

+ infobox IPAM plugin
+ Weave Net
+ Contiv Network plugin

> https://www.docker.com/blog/understanding-docker-networking-drivers-use-cases/


> docker network inspect bridge

> docker network create --driver bridge app-net

> docker run -dit --name app1 --network app-net alpine ash
> docker run -dit --name app2 --network app-net alpine ash

> docker attach app1

> type Ctrl-PQ to exit the container, not exit

> docker network create --driver overlay app-overlay

> docker service create --network app-overlay --name app1 --replicas=6 nginx


## PUblishing Ports


> https://docs.docker.com/engine/containers/run/#exposed-ports


## Comparing Host and Ingress Port Publishing

### Host Port Publishing

+ Typically used with a global mode service
+ Used to publish a single port on each host
+ When publishing ports, specify mode=host


### Ingress Port Publishing

+ Used with replicated mode services(docker swarm)
+ Used to publish a single port across all hosts that goes to a pool of containers 

> https://docs.docker.com/engine/swarm/ingress/#bypass-the-routing-mesh

## DNS

> docker run -dit --dns 1.1.1.1 centos /bin/bash

> vim /etc/docker/daemon.json 

```json
{
    "dns": ['1.1.1.1']
}

```

## Configuring Load Balancing

### Load Balancing

+ Docker Swarm provides load balancing of worloads as they are instantiated 
+ Scaling of new worloads is very easy
+ However Swarm hasn't traditionally saled up or scaled down containers, based on demand

> https://www.haproxy.com/blog/haproxy-on-docker-swarm-load-balancing-and-dns-service-discovery

> https://success.docker.com/article/ucp-service-discovery

> https://web.archive.org/web/20181026174833/http://success.docker.com/article/ucp-service-discovery

> https://docs.docker.com/engine/network/tutorials/host/

> docker run -d --name nginx --network host nginx
> docker port  nginx

## Troubleshooting Docker Networking


