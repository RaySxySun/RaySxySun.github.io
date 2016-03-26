---
layout: post
title: Centos, SSH and FTP for Remote Administration
date: 2016-03-29
weather: sunny
categories: Java Framework
tags: [Persistence framework ,SQL mapper framework]
description: 
---

# SSH


Steps|Description|Commands
---|---|
1|InstalL & Test|sudo docker run -i -t centos /bin/bash
2|Check system info|cat /etc/redhat-release
3|Check Linux kernel|uname -r
4|update source|yum -y update 
5|install vsftpd|yum -y install vsftpd*
6|edit vsftpd conf|vim /etc/vsftpd/vsftpd.conf
7|restart|systemctl restart vsftpd
8|vsftpd service starts at boot|systemctl enable vsftpd
9|Allow vsftpd through the Firewall|firewall-cmd --permanent --add-port=21/tcp
10|reload the firewall|firewall-cmd --reload




	Disallow anonymous, unidentified users to access files via FTP; change the anonymous_enable setting to NO:

	anonymous_enable=NO

	Allow local uses to login by changing the local_enable setting to YES:

	local_enable=YES

	If you want local user to be able to write to a directory, then change the write_enable setting to YES:

	write_enable=YES

	Local users will be ‘chroot jailed’ and they will be denied access to any other part of the server; change the chroot_local_user setting to YES:

	chroot_local_user=YES

	Exit and save the file with the command :wq .




#FPT
vsftpd stands for very secure FTP daemon

sudo apt-get install vsftpd