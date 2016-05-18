---
layout: post
title: Design Patterns - Adapter
date: 2016-04-22
weather: cloudy
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)

# 6. Adapter 

- **Intent**: 
	- Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.
- **Applicability**:
	- you want to use an existing class, and its interface does not match the one you need.
	- you want to create a reusable class that cooperates with unrelated or unforeseen classes, that is, classes that don't necessarily have compatible interfaces.
	- you need to use several existing subclasses, but it's impractical to adapt their interface by subclassing every one. An object adapter can adapt the interface of its parent class

- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Adapter.png" alt="{{ page.title }} at {{ site.title }}">

- **Participants**:
	- Target (Shape)
		- defines the domain-specific interface that Client uses.
	- Client (DrawingEditor)
		- collaborates with objects conforming to the Target interface.
	- Adaptee (TextView)
		- defines an existing interface that needs adapting.
	- Adapter (TextShape)
		- adapts the interface of Adaptee to the Target interface.	

- **Collaborations**
	- Clients call operations on an Adapter instance. In turn, the adapter calls Adaptee operations that carry out the request.

- **Consequences**:
	- A class adapter
		- 1.adapts Adaptee to Target by committing to a concrete Adapter class. As a consequence, a class adapter won't work when we want to adapt a class and all its subclasses.
		- 2.lets Adapter override some of Adaptee's behavior, since Adapter is a subclass of Adaptee.
		- 3.introduces only one object, and no additional pointer indirection is needed to get to the adaptee.
	- An object adapter
		- 1.lets a single Adapter work with many Adaptees—that is, the Adaptee itself and all of its subclasses (if any). The Adapter can also add functionality to all Adaptees at once.
		- 2.makes it harder to override Adaptee behavior. It will require subclassing Adaptee and making Adapter refer to the subclass rather than the Adaptee itself.

- **Issue to consider:**
	- 1.How much adapting does Adapter do?
	- 2.Pluggable adapters?
	- 3.Using two-way adapters to provide transparency.

- **Related Patterns**:
	- Bridge has a structure similar to an object adapter, but **Bridge** has a different **intent: It is meant to separate an interface from its implementation so that they can be varied easily and independently.** An adapter is meant to change the interface of an existing object.
	- Decorator enhances another object without changing its interface. A decorator is thus more transparent to the application than an adapter is. As a consequence, Decorator supports recursive composition, which isn't possible with pure adapters
	- Proxy defines a representative or surrogate for another object and does not change its interface.

- Code Sample 1: Class Adapter(extends Source)

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Adapter.jpg" alt="{{ page.title }} at {{ site.title }}">	

		public class Source {  
		  
		    public void method1() {  
		        System.out.println("this is original method!");  
		    }  
		}  

		public interface Targetable {  
		  
		    public void method1();   
		    public void method2();  
		} 		

		public class Adapter extends Source implements Targetable {  
		  
		    @Override  
		    public void method2() {  
		        System.out.println("this is the targetable method!");  
		    }  
		}

		public class AdapterTest {  
		  
		    public static void main(String[] args) {  
		        Targetable target = new Adapter();  
		        target.method1();  
		        target.method2();  
		    }  
		}  

---

- Code Sample 2: Object Adapter(private Source source)

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Adapter2.jpg" alt="{{ page.title }} at {{ site.title }}">	

		public class Wrapper implements Targetable {  
		  
		    private Source source;  
		      
		    public Wrapper(Source source){  
		        super();  
		        this.source = source;  
		    }  
		    @Override  
		    public void method2() {  
		        System.out.println("this is the targetable method!");  
		    }  
		  
		    @Override  
		    public void method1() {  
		        source.method1();  
		    }  
		} 

		public class AdapterTest {  
		  
		    public static void main(String[] args) {  
		        Source source = new Source();  
		        Targetable target = new Wrapper(source);  
		        target.method1();  
		        target.method2();  
		    }  
		}  

---

- Code Sample 3: Interface Adapter(interface Sourceable)

> Additional Content:  Interface Adapter(too many methods in the interface Sourceable) 

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Adapter3.jpg" alt="{{ page.title }} at {{ site.title }}">	

		public interface Sourceable {  
		      
		    public void method1();  
		    public void method2();  
		} 

		public abstract class Wrapper2 implements Sourceable{  
		      
		    public void method1(){}  
		    public void method2(){}  
		}  

		public class SourceSub1 extends Wrapper2 {  
		    public void method1(){  
		        System.out.println("the sourceable interface's first Sub1!");  
		    }  
		} 

		public class SourceSub2 extends Wrapper2 {  
		    public void method2(){  
		        System.out.println("the sourceable interface's second Sub2!");  
		    }  
		}

		public class WrapperTest {  
		  
		    public static void main(String[] args) {  
		        Sourceable source1 = new SourceSub1();  
		        Sourceable source2 = new SourceSub2();  
		          
		        source1.method1();  
		        source1.method2();  
		        source2.method1();  
		        source2.method2();  
		    }  




> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)