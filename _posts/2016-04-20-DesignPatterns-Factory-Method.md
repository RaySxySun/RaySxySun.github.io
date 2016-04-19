---
layout: post
title: Design Patterns - Factory Method
date: 2016-04-20
weather: starry
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)

# 3. Factory Method

- **Intent**: 
	- Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.
- **Also Known As**:
	- Virtual Constructor
- **Applicability**:
	- a class can't anticipate the class of objects it must create.
	- a class wants its subclasses to specify the objects it creates.
	- classes delegate responsibility to one of several helper subclasses, and you want to localize the knowledge of which helper subclass is the delegate.

- **Motivation**:	
	- Frameworks use abstract classes to define and maintain relationships between objects. A framework is often responsible for creating these objects as well.

- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Factory.png" alt="{{ page.title }} at {{ site.title }}">

- **Participants**:
	- Product (Document)
		- defines the interface of objects the factory method creates.
	- ConcreteProduct (MyDocument)
		- implements the Product interface.
	- Creator (Application)
		- declares the factory method, which returns an object of type Product. Creator may also define a default implementation of the factory method that returns a default ConcreteProduct object.
		- may call the factory method to create a Product object. 
	- ConcreteCreator (MyApplication)
	- overrides the factory method to return an instance of a ConcreteProduct.

- **Collaborations**
	- ConcreteCreator to define the factory method(Creator declares) so that it returns an instance of the appropriate ConcreteProdu

> Interaction diagram

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/FactoryMethod.jpg" alt="{{ page.title }} at {{ site.title }}">
	

- **Consequences**:
	- 1. Provides hooks for subclasses.
	- 2. Connects parallel class hierarchies.


- Code Sample

	//product interface
	public interface Sender {  
	    public void Send();  
	} 

	//two concrete product classes
	public class MailSender implements Sender {  
	    @Override  
	    public void Send() {  
	        System.out.println("this is mailsender!");  
	    }  
	}  

	public class SmsSender implements Sender {  
	  
	    @Override  
	    public void Send() {  
	        System.out.println("this is sms sender!");  
	    }  
	}  

	//provider interface
	public interface Provider {  
	    public Sender produce();  
	}  

	//two factory concrete class
	public class SendMailFactory implements Provider {  
	      
	    @Override  
	    public Sender produce(){  
	        return new MailSender();  
	    }  
	}  

	public class SendSmsFactory implements Provider{  
	  
	    @Override  
	    public Sender produce() {  
	        return new SmsSender();  
	    }  
	}  

	//test
	public class Test {  
	    public static void main(String[] args) {  
	        Provider provider = new SendMailFactory();  
	        Sender sender = provider.produce();  
	        sender.Send();  
	    }  
	}  