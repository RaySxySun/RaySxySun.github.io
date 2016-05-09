---
layout: post
title: Design Patterns - Chain of Resp
date: 2016-05-09
weather: starry
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)

# 13. Chain of Responsibility 

- **Intent**: 
	- Avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an objecthandles it.

- **Applicability**:
	- more than one object may handle a request, and the handler isn't **known a priori**. The handler should be **ascertained** automatically
	- you want to issue a request to one of several objects without specifying the receiver explicitly.
	- the set of objects that can handle a request should be specified dynamically.

- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/ChainOfResponsibility1.png" alt="{{ page.title }} at {{ site.title }}">

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/ChainOfResponsibility2.png" alt="{{ page.title }} at {{ site.title }}">

- **Participants**:
	- Handler
		- defines an interface for handling requests.
		- (optional) implements the successor link.
	- ConcreteHandler
		- handles requests it is responsible for.
		- can access its successor.
		- if the ConcreteHandler can handle the request, it does so; otherwise it forwards the request to its successor.
	- Client
		- initiates the request to a ConcreteHandler object on the chain.

- **Collaborations**
	- When a client issues a request, the request propagates along the chain until a ConcreteHandler object takes responsibility for handling it.
- **Consequences**:
	- 1.Reduced coupling.
	- 2.Added flexibility in assigning responsibilities to objects.
	- 3.Receipt isn't guaranteed.

- **Related Patterns**:
	- Chain of Responsibility is often applied in conjunction with Composite. There, a component's parent can act as its successor.

- Code Sample: Chain of Responsibility 

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/ChainOfResponsibilitySample.png" alt="{{ page.title }} at {{ site.title }}">	

		public interface Handler {  
		    public void operator();  
		} 

		public abstract class AbstractHandler {  
		      
		    private Handler handler;  
		  
		    public Handler getHandler() {  
		        return handler;  
		    }  
		  
		    public void setHandler(Handler handler) {  
		        this.handler = handler;  
		    }  
		      
		}

		public class MyHandler extends AbstractHandler implements Handler {  
		  
		    private String name;  
		  
		    public MyHandler(String name) {  
		        this.name = name;  
		    }  
		  
		    @Override  
		    public void operator() {  
		        System.out.println(name+" test");  
		        if(getHandler()!=null){  
		            getHandler().operator();  
		        }  
		    }  
		}  

		public class Test {  
		  
		    public static void main(String[] args) {  
		        MyHandler h1 = new MyHandler("h1");  
		        MyHandler h2 = new MyHandler("h2");  
		        MyHandler h3 = new MyHandler("h3");  
		  
		        h1.setHandler(h2);  
		        h2.setHandler(h3);  
		  
		        h1.operator();  
		    }  
		} 	

> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)