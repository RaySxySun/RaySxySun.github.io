---
layout: post
title: TrinityCore
date: 2016-03-01
categories: C++
tags: [C++, Language C]
description: 
---

# Build & Installation

> Docker Images: [HERE](https://hub.docker.com/search/?isAutomated=0&isOfficial=0&page=1&pullCount=0&q=trinitycore&starCount=0)

Steps|Description|Commands
---|---|
1|setup dev env| sudo docker pull ubuntu:16.04
|| docker run -d  --dns 8.8.8.8 --dns 8.8.4.4 -p 80:80 -v /home/ray:/home/ray --name=wow -h WOW ubuntu /bin/bash -c "while true;do sleep 1000; done"
2|Network Config:docker run|1. sudo docker run --net:host --name ubuntu_bash -i -t ubuntu:latest /bin/bash 
||2. sudo docker run --dns 8.8.8.8 --dns 8.8.4.4 --name ubuntu_bash -i -t ubuntu:latest /bin/bash
||3. vi /etc/default/docker  然后去掉“docker_OPTS="--dns 8.8.8.8 --dns 8.8.4.4"”前的#号
||4. vi /etc/NetworkManager/NetworkManager.conf  在dns=dnsmasq前加个#号注释掉 然后 sudo restart network-manager && sudo restart docker
||5. pkill docker && iptables -t nat -F && ifconfig docker0 down && brctl delbr docker0 && docker -d
||6. 直接在docker内修改/etc/hosts
3|Docker network|sudo vim /etc/resolvconf/resolv.conf.d/base (nameserver 8.8.8.8 nameserver 8.8.4.4)

docker run -d  --dns 8.8.8.8 --dns 8.8.4.4 -v /home/ray:/home/ray --name=wow -h WOW ubuntu /bin/bash -c "while true;do sleep 1000; done"


> Linux Requirements

	Processor with SSE2 support 
	Boost ≥ 1.55
	MySQL ≥ 5.1.0 
	OpenSSL ≥ 1.0.0 
	CMake ≥ 2.8.9
	ZeroMQ ≥ 2.2.6 (6.x branch only)
	GCC ≥ 4.7.2 or Clang  ≥ 3.3
	zlib ≥ 1.2.7
	
	sudo apt-get install build-essential autoconf libtool gcc g++ make cmake git-core wget p7zip-full libncurses5-dev zlib1g-dev libbz2-dev
	sudo apt-get install openssl libssl-dev mysql-server mysql-client libmysqlclient-dev libmysql++-dev libreadline6-dev
	sudo apt-get install libboost-dev libboost-thread-dev libboost-system-dev libboost-filesystem-dev libboost-program-options-dev libboost-iostreams-dev

> Core installation

	sudo adduser <username>

	cd TrinityCore
	mkdir build
	cd build 

	cmake ../ -DCMAKE_INSTALL_PREFIX=/home/<username>/server -DWITH_WARNINGS=1
 
	#Another Example Below:
	cmake ../ -DCMAKE_INSTALL_PREFIX=/home/wow/server -DCONF_DIR=/home/wow/server/etc -DTOOLS=1 -DWITH_WARNINGS=1

	#BUILDING
	make
	make install
	
	# or multiple CPU
	make -j <number of cores>
	make install
	# alternative way
	make -j$(nproc) install