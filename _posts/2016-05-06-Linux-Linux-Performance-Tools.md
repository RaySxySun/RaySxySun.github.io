---
layout: post
title: Linux - Performance Tools
date: 2016-05-06
weather: sunny
categories: Linux 
tags: [Linux]
description: 
---

# Linux Performance Tools


<img src="{{ site.url }}/assets/img/2016-05-06-LinuxPerformance/LinuxPerformance1.png" alt="{{ page.title }} at {{ site.title }}">		

<img src="{{ site.url }}/assets/img/2016-05-06-LinuxPerformance/LinuxPerformance2.png" alt="{{ page.title }} at {{ site.title }}">	

<img src="{{ site.url }}/assets/img/2016-05-06-LinuxPerformance/LinuxPerformance3.png" alt="{{ page.title }} at {{ site.title }}">	

<img src="{{ site.url }}/assets/img/2016-05-06-LinuxPerformance/LinuxPerformance4.png" alt="{{ page.title }} at {{ site.title }}">	


# Q&A
		# Problem 1: No history log; [default] using sar will check the log.
		ray@[๑'ㅂ'و]✧ sudo sar 
		Cannot open /var/log/sysstat/sa06: No such file or directory
		Please check if data collecting is enabled in /etc/default/sysstat
		# Solution: Generate corresponding log
		ray@[๑'ㅂ'و]✧ sudo sar -o 06


> [Refference warmhole](http://www.brendangregg.com/blog/index.html)