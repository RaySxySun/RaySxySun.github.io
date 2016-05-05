---
layout: post
title: Design Patterns - Façade
date: 2016-05-04
weather: starry
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)

# 10. Façade 

- **Intent**: 
	- Provide a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.
- **Motivation**:
	- Structuring a system into subsystems helps reduce complexity. A common design goal is to minimize the communication and dependencies between subsystems.
- **Applicability**:
	- you want to provide a simple interface to a complex subsystem.
	- there are many dependencies between clients and the implementation classes of an abstraction.
	- you want to layer your subsystems.

- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Facade.png" alt="{{ page.title }} at {{ site.title }}">

- **Participants**:
	- Facade
		- knows which subsystem classes are responsible for a request.
		- delegates client requests to appropriate subsystem objects.
	- subsystem classes
		- implement subsystem functionality.
		- handle work assigned by the Facade object.
		- have no knowledge of the facade; that is, they keep no references to it.

- **Collaborations**
	- Clients communicate with the subsystem by sending requests to Facade, which forwards them to the appropriate subsystem object(s). Although the subsystem objects perform the actual work, the facade may have to do work of its own to translate its interface to subsystem interfaces.
	- Clients that use the facade don't have to access its subsystem objects directly.

- **Consequences**:
	- 1.It shields clients from subsystem components, thereby reducing the number of objects that clients deal with and making the subsystem easier to use.
	- 2.It promotes **weak coupling** between the subsystem and its clients.
	- 3.It doesn't prevent applications from using subsystem classes if they need to. Thus you can choose between ease of use and generality.

- **Related Patterns**:
	- Abstract Factory can be used with Facade to provide an interface for creating subsystem objects in a subsystem-independent way. Abstract Factory can also be used as an alternative to Facade to hide platform-specific classes.
	- Mediator is similar to Facade in that it abstracts functionality of existing classes. However, Mediator's purpose is to abstract arbitrary communication between colleague objects, often centralizing functionality that doesn't belong in any one of them.

- Code Sample: Façade

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/FacadeSample.png" alt="{{ page.title }} at {{ site.title }}">	

		public class CPU {  
		      
		    public void startup(){  
		        System.out.println("cpu startup!");  
		    }  
		      
		    public void shutdown(){  
		        System.out.println("cpu shutdown!");  
		    }  
		} 

		public class Memory {  
		      
		    public void startup(){  
		        System.out.println("memory startup!");  
		    }  
		      
		    public void shutdown(){  
		        System.out.println("memory shutdown!");  
		    }  
		}  

		public class Disk {  
		      
		    public void startup(){  
		        System.out.println("disk startup!");  
		    }  
		      
		    public void shutdown(){  
		        System.out.println("disk shutdown!");  
		    }  
		} 

		public class Computer {  
		    private CPU cpu;  
		    private Memory memory;  
		    private Disk disk;  
		      
		    public Computer(){  
		        cpu = new CPU();  
		        memory = new Memory();  
		        disk = new Disk();  
		    }  
		      
		    public void startup(){  
		        System.out.println("start the computer!");  
		        cpu.startup();  
		        memory.startup();  
		        disk.startup();  
		        System.out.println("start computer finished!");  
		    }  
		      
		    public void shutdown(){  
		        System.out.println("begin to close the computer!");  
		        cpu.shutdown();  
		        memory.shutdown();  
		        disk.shutdown();  
		        System.out.println("computer closed!");  
		    }  
		}  

		public class User {  
		  
		    public static void main(String[] args) {  
		        Computer computer = new Computer();  
		        computer.startup();  
		        computer.shutdown();  
		    }  
		}  
		

> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)