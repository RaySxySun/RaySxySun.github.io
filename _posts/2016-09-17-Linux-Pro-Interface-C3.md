---
layout: post
title: The Linux Programming Interface - Chapter 3 System Programming Concepts
date: 2016-09-17
weather: cloudy
categories: Linux
tags: [Linux]
description:
---

> Chapter 3 System Programming Concepts


- ### 3.1 System Calls
	- [Concept] A system call is a controlled entry point into the kernel, allowing a process to request that the kernel perform some action on the process's behalf.
	- [How] How a system call works?
		- (1) [Kernel Model] it changes the processor state from user model to kernel model. So CPU can access protected kernel memory.
		- (2) [a unique #] The set of system calls is fixed. Each system call is identified by a unique number.
		- (3) [arguments] Each system call may have a set of arguments that specify information to be transferred from user space
	- [Steps] The steps of execution of a system call.
		- (1) Invoking a wrapper function in the C library.
		- (2) The wrapper func should pass all the system call arguments into the sys call trap-handling rountine. via the **statck** & copy the args to **registers**.
		- (3) The wrapper func copies the sys call # into a specific CPU register.
		- (4) The wrapper executes a trap machine instruction (int 0x80). switch to kernel model
			- [up-to-date] more recent x86-32 sys introduces the sysenter instruction. it is more faster than 0x80 trap instruction.(2.6 kernel & from glibc2.3.2 onward)
		- (5) The kernel invokes its system_call(). (arch/i386/entry.S)
			- (5.1) save register value onto the kernel stack.
			- (5.2) check the validity of the system call #
			- (5.3) invokes appropriate sys call found by sys call # in sys_call_table. check validity of arguments if any. return value
			- (5.4) restore register values from the kernel stack and places the sys call return value on the stack.
			- (5.5) return to the wrapper func, and return the processor to user model.
			- (5.6) if the returning value is an error. the wrapper sets global variable "errno" using this value. return to caller. return an integer indicating success or failure. (non-negative success) (-1 failure)

<img src="{{ site.url }}/assets/img/2016-09-17-Linux-Pro-Interface/ C3_1.png" alt="{{ page.title }} at {{ site.title }}">	

---

- ### 3.2 Library Functions	
	- [Concept] A library function constitutes the standard C library	
		- e.g. opening a file, converting a time to a human-readable format, and comparing two character strings.

---

- ### 3.3 The Standard C Library & The GNU C Library ( glibc )
	- 1.[Many] There are different implementations of the standard C library on the various UNIX implementations.
	- 2.[Most Common] [glibc](http://www.gnu.org/software/libc/): The most commonly used implementation on Linux is the GNU C library
	- 3.[Version] running the glibc shared library file as an executable program
		- [note] the GNU C library resides at other pathname in diff Linux destribution.
		- [solution] 
			- (1) run "ldd" to inspect the resulting library dependency list to find the location of the glibc shared library
			- (2) run the dependency link
	- 4.[TWO Constants] glibc defines two constants, \_\_GLIBC\_\_ and \_\_GLIBC_MINOR\_\_
		- [Example] glibc 2.12:  \_\_GLIBC\_\_ (2) & \_\_GLIBC_MINOR\_\_ (12)
	- 5.[Reliable run time func] gnu_get_libc_version() 




						========================3.Version======================
						$ /lib/libc.so.6
						GNU C Library stable release version 2.10.1, by Roland McGrath et al.
						Copyright (C) 2009 Free Software Foundation, Inc.
						This is free software; see the source for copying conditions.
						There is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A
						PARTICULAR PURPOSE.
						Compiled by GNU CC version 4.4.0 20090506 (Red Hat 4.4.0-4).
						Compiled on a Linux >>2.6.18-128.4.1.el5<< system on 2009-08-19.
						Available extensions:
						The C stubs add-on version 2.1.2.
						crypt add-on version 2.1 by Michael Glad and others
						GNU Libidn by Simon Josefsson
						Native POSIX Threads Library by Ulrich Drepper et al
						BIND-8.2.3-T5B
						RT using linux kernel aio
						For bug reporting instructions, please see:
						<http://www.gnu.org/software/libc/bugs.html>.
						---------------------3.solution "ldd"---------------------
						$ ldd ~/Tools/pycharm-5.0.4/bin/libyjpagent-linux.so | grep libc
						> libc.so.6 => /lib/i386-linux-gnu/libc.so.6 (0xf720c000)
						$ /lib/i386-linux-gnu/libc.so.6 
						========================5.Version======================
						#include <gnu/libc-version.h>
						const char *gnu_get_libc_version(void);
						//Returns pointer to null-terminated, statically allocated string containing GNU C library version number

---

- To summarize:
	