---
layout: post
title: Linux command collection
date: 2016-08-20
weather: sunny
categories: Linux
tags: [shell, linux, automation, docker]
description: 
---

# 1.SSH Shortcuts

				Host centdev  
				        HostName 192.168.12.22  
				        Port    2345  
				        User    root  
				Host fc5dev  
				        HostName 192.168.12.11  
				        Port   4567  
				        User    btbt  
				Host svn  
				        HostName 192.168.12.22
				        User    root  

				Host svn2  
				        HostName 192.168.12.232  
				        User    root  s
				        PreferredAuthentications publickey  
				        PubkeyAuthentication yes  
				        IdentityFile ~/.ssh/id_rsa

				=======config target machine======
				/etc/ssh/sshd_config
				RSAAuthentication yes
				PubkeyAuthentication yes
				AuthorizedKeysFile	%h/.ssh/pub_keys

				// OR
				AuthorizedKeysFile	~/.ssh/pub_keys

				//chmod 700 .ssh 

---

# 2. SSH download/upload

			scp -r root@cnwbzp1048.cn.dst.ibm.com:/home/ucdagent/ucd_cli/* ./


