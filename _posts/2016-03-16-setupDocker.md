---
layout: post
title: setup docker
date: 2016-03-16
weather: starry
categories: Docker
tags: [CI, Docker]
description: 
---

# Build & Installation

> Docker Images: [HERE](https://hub.docker.com/search/?isAutomated=0&isOfficial=0&page=1&pullCount=0&q=trinitycore&starCount=0)


Steps|Description|Commands
---|---|
1|Install docker| apt-get install docker.io
2|Launch service| service docker.io status
|| service docker.io start
3|softe link| ln -sf /usr/bin/docker.io /usr/local/bin/docker
4|import docker| cat ubuntu14.tar 
|| sudo docker import - ray/ubuntu14
|OR pull a images| sudo docker pull ubuntu:16.04
||sudo docker images
5|run docker container| docker run -d  --dns 8.8.8.8 --dns 8.8.4.4  -p 80:80 -p 9200:9200 -p 50000:50000 -v /home/ray/www:/home/ray/www --name=ibmsc -h SC ray/ubuntu14 /bin/bash -c "while true;do sleep 1000; done"
|| google DNS: --dns 8.8.8.8 --dns 8.8.4.4
6| start & login|sudo docker start ibmsc
|| login-docker.sh ibmsc
7|save| docker commit wow wowserver
8|export docker image| sudo docker ps -a
||sudo docker export 7691a814370e > ubuntu14.tar
9|remove docker images| docker stop $(docker ps -a -q)
|| sudo docker rmi -f 87bce9b2c54c
10|reomve all container|docker rm $(docker ps -a -q)
11|remove a image| docker rmi <image id>
12|remove all images|docker rmi $(docker images -q)
13|Network Config:docker run|1. sudo docker run --net:host --name ubuntu_bash -i -t ubuntu:latest /bin/bash 
||2. sudo docker run --dns 8.8.8.8 --dns 8.8.4.4 --name ubuntu_bash -i -t ubuntu:latest /bin/bash
||3. vi /etc/default/docker  然后去掉“docker_OPTS="--dns 8.8.8.8 --dns 8.8.4.4"”前的#号
||4. vi /etc/NetworkManager/NetworkManager.conf  在dns=dnsmasq前加个#号注释掉 然后 sudo restart network-manager && sudo restart docker
||5. pkill docker && iptables -t nat -F && ifconfig docker0 down && brctl delbr docker0 && docker -d
||6. 直接在docker内修改/etc/hosts
14|Docker network|sudo vim /etc/resolvconf/resolv.conf.d/base (nameserver 8.8.8.8 nameserver 8.8.4.4)
15|Docker Version|docker version
16|Download & test latest centos server|sudo docker run -i -t centos /bin/bash
