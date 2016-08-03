---
layout: post
title: Vagrant+DB2+PuPHPe
date: 2016-09-27
weather: cloudy
categories: Linux
tags: [Linux]
description: 
---

# Vagrant+DB2+PuPHPe

> Wormhole: [vagrant](https://www.vagrantup.com) & [PuPHPet](https://puphpet.com/#frontpage) 

### 1.Vagrant

- Getting started 
	- [Step 1] Installing a box
	- [Step 2] Using a box
	- [Step 3] An workaround against the [Authentication failure](https://github.com/mitchellh/vagrant/issues/7610)
		
			// step 1: Installing a box
			vagrant box add centos/7
			
			// step 2: Using a box
			vagrant init centos/7
			
			// step 3: workarount: Authentication failure
			 setting "config.ssh.insert_key = false" in Vagrantfile 

			 // frequently used commands
			 vagrant box add [options] <name, url, or path>
			 vagrant init [options]
			 vagrant global-status			// check the running instance
			 vagrant halt [name|id]			// stop an instance by name/id
			 vagrant destroy [name|id]		// destroy an instance
			 vagrant reload					// modify an instance and reload it		

---

### 2.DB2

- (1) Check system info
- (2) Change hostname

- (2) Install v10.5fp6_linuxx64_server_t.tar.gz
	- [uncompress]

	=======================================(1)=====================================
	// Kernel Version
	$ uname -a
	$ uname -r
	$ cat /etc/redhat-release 
	// print sizes in human readable format
	$ df -h
			/dev/mapper/VolGroup00-LogVol00   37G  3.8G   32G  11% /
			devtmpfs                         234M     0  234M   0% /dev
			tmpfs                            245M     0  245M   0% /dev/shm
			tmpfs                            245M  4.4M  241M   2% /run
			tmpfs                            245M     0  245M   0% /sys/fs/cgroup
			/dev/sda2                        477M  103M  346M  23% /boot
			tmpfs                             49M     0   49M   0% /run/user/1000
			tmpfs                             49M     0   49M   0% /run/user/0
	=======================================(2)=====================================
	$ cat /etc/resolv.conf 
	$ cat /etc/sysconfig/network
	$ /etc/sysconfig/network-scripts/ifcfg-*
	$ ifconfig
	$ echo "hostname db2agent.localdomain" >>/etc/sysconfig/network
	$ reboot
	$ ifconfig
		-bash: ifconfig: command not found
	$ yum search ifconfig
	$ yum install net-tools.x86_64
	$ netstat



	========================================(3)=====================================
	// uncompress
	sudo tar -zxvf v10.5fp6_linuxx64_server_t.tar.gz ./temp

	===============================(4)trouble shooting===============================
	// userdel: Creating mailbox file: File exists
	userdel -r [username]
---	

PuPHPet is ideal for those who don’t know how to write Puppet manifests. You download the resultant Puppet configuration and run vagrant up .[Alternative Vaprobash]



