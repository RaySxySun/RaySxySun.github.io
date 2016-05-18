---
layout: post
title: Design Patterns - Singleton
date: 2016-04-22
weather: sunny
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)

# 5. Singleton 

> Simple Concept, Complex Implementation

- **Intent**: 
	- Ensure a class only has one instance, and provide a global point of access to it.
- **Applicability**:
	- there must be exactly one instance of a class, and it must be accessible to clients from a well-known access point.
	- when the sole instance should be extensible by subclassing, and clients should be able to use an extended instance without modifying their code.

- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Singleton.png" alt="{{ page.title }} at {{ site.title }}">

- **Participants**:
	- Singleton
		- defines an Instance operation that lets clients access its unique instance. Instance is a class operation.
		- may be responsible for creating its own unique instance.

- **Collaborations**
	- Clients access a Singleton instance solely through Singleton's Instance operation.

- **Consequences**:
	- 1.Controlled access to sole instance.
	- 2.Reduced name space.
	- 3.Permits refinement of operations and representation.
	- 4.Permits a variable number of instances.
	- 5.More flexible than class operations.

- **Related Patterns**:
	- Many patterns can be implemented using the Singleton pattern.(Abstract Factory, Builder, Prototype)

> Interaction diagram

	

- Code Sample

> Sample 1: Simple Singleton (Non Thread-Safe)

		public class Singleton {  
		  
		    /* private instance for lazy load */  
		    private static Singleton instance = null;  
		  
		    /* do not allow instantiation*/  
		    private Singleton() {  
		    }  
		  
		    /* static method create instance */  
		    public static Singleton getInstance() {  
		        if (instance == null) {  
		            instance = new Singleton();  
		        }  
		        return instance;  
		    }  
		  
		    /*instance will keep the same before/after serialization*/  
		    public Object readResolve() {  
		        return instance;  
		    }  
		}  

> An Improvement 
	
	/*
	Improvement: Thread-Safe(synchronized)
	Problem: Bad Performance(calling the method will lock instance every time)
	*/

		public static synchronized Singleton getInstance() {  
		        if (instance == null) {  
		            instance = new Singleton();  
		        }  
		        return instance;  
		}  


> Another Improvement

	/*
	Improvement: the above problem is resolved
	Problem: In JVM, creation and assignment are two independent operations. (instance = new Singleton();)
	Example:
		1. Thread A, B execute IF condition at the same time.
		2. Thread A find instance is null. Then JVM will create a space in Hotspot JVM Heap, while the instace has not been initialized at the moment.
		3. Thread B find there is a instance and it try to call the instance's method. (ERROR!!!)
	*/

		public static Singleton getInstance() {  
	        if (instance == null) {  
	            synchronized (instance) {  
	                if (instance == null) {  
	                    instance = new Singleton();  
	                }  
	            }  
	        }  
	        return instance;  
	    }  

> Sample 2: inner class, thread mutex (Thread-Safe)
	
		/*
		A mutex is a programming concept that is frequently used to solve multi-threading problems.
		The process of The Java Virtual Machine dynamically loads, links and initializes classes is thread-safe
		private static Singleton instance = new Singleton();  will execute exactly once.
		*/

		/*
		Problem: If Class Singleton's creator throws an exception. We will never get a instance.
		Nothing is perfect!!!
		*/

		public class Singleton {  
		  
		    private Singleton() {  
		    }  
		  
		    /* inner class is responsible for managing Singleton instance */  
		    private static class SingletonFactory {  
		        private static Singleton instance = new Singleton();  
		    }  
		  
		    /* get instance */  
		    public static Singleton getInstance() {  
		        return SingletonFactory.instance;  
		    }  
		  
		    public Object readResolve() {  
		        return getInstance();  
		    }  
		}  

> Sample 3: separate the method getInstance into two methods getInstance and syncInit (Thread-Safe)

		public class SingletonSample3 {  
		  
		    private static SingletonSample3 instance = null;  
		  
		    private SingletonSample3() {  
		    }  
		  
		    private static synchronized void syncInit() {  
		        if (instance == null) {  
		            instance = new SingletonSample3();  
		        }  
		    }  
		  
		    public static SingletonSample3 getInstance() {  
		        if (instance == null) {  
		            syncInit();  
		        }  
		        return instance;  
		    }  
		}  

> Sample 4: Using shadow instance to update Singleton instance

		public class SingletonShadowTest {  
		  
		    private static SingletonShadowTest instance = null;  
		    private Vector properties = null;  
		  
		    public Vector getProperties() {  
		        return properties;  
		    }  
		  
		    private SingletonShadowTest() {  
		    }  
		  
		    private static synchronized void syncInit() {  
		        if (instance == null) {  
		            instance = new SingletonShadowTest();  
		        }  
		    }  
		  
		    public static SingletonShadowTest getInstance() {  
		        if (instance == null) {  
		            syncInit();  
		        }  
		        return instance;  
		    }  
		  
		    public void updateProperties() {  
		        SingletonShadowTest shadow = new SingletonShadowTest();  
		        properties = shadow.getProperties();  
		    }  
		}  


> What is a mutex?:

	When I am having a big heated discussion at work, I use a rubber chicken which I keep in my desk for just such occasions. The person holding the chicken is the only person who is allowed to talk. If you don't hold the chicken you cannot speak. You can only indicate that you want the chicken and wait until you get it before you speak. Once you have finished speaking, you can hand the chicken back to the moderator who will hand it to the next person to speak. This ensures that people do not speak over each other, and also have their own space to talk.

	Replace Chicken with Mutex and person with thread and you basically have the concept of a mutex.

	Of course, there is no such thing as a rubber mutex. Only rubber chicken. My cats once had a rubber mouse, but they ate it.

	Of course, before you use the rubber chicken, you need to ask yourself whether you actually need 5 people in one room and it would not just be easier with one person in the room on their own doing all the work. Actually, this is just extending the analogy, but you get the idea.

> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)