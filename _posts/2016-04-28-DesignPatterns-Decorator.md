---
layout: post
title: Design Patterns - Decorator
date: 2016-04-28
weather: cloudy
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)

# 9. Decorator 

- **Intent**: 
	- Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.
- **Also Known As**:
	- Wrapper
- **Applicability**:
	- to add responsibilities to individual objects dynamically and transparently, that is, without affecting other objects.
	- for responsibilities that can be withdrawn(cancel/remove these responsibilities).
	- when extension by subclassing is impractical. Sometimes a large number of independent extensions are possible and would produce an explosion of subclasses to support every combination. Or a class definition may be hidden or otherwise unavailable for subclassing.

- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Decorator.png" alt="{{ page.title }} at {{ site.title }}">

- **Participants**:
	- Component
		- defines the interface for objects that can have responsibilities added to them dynamically.
	- ConcreteComponent
		- defines an object to which additional responsibilities can be attached.
	- Decorator 
		- maintains a reference to a Component object and defines an interface that conforms to Component's interface.
	- ConcreteDecorator
		- adds responsibilities to the component.

- **Collaborations**
	- Decorator forwards requests to its Component object. It may optionally perform additional operations before and after forwarding the request.

- **Consequences**:
	- 1.More flexibility than static inheritance.
	- 2.Avoids feature-laden(multifunction) classes high up in the hierarchy
	- 3.A decorator and its component aren't identical.
	- 4.Lots of little objects. They can be hard to learn and debug.

- **Related Patterns**:
	- Adapter: A decorator is different from an adapter in that a decorator only changes an object's responsibilities, not its interface; an adapter will give an object a completely new interface.
	- Composite: A decorator can be viewed as a degenerate composite with only one component. However, a decorator adds additional responsibilities—it isn't intended for object aggregation.
	- Strategy: A decorator lets you change the skin of an object; a strategy lets you change the guts. These are two alternative ways of changing an object

- Code Sample: Composite

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/DecoratorSample.png" alt="{{ page.title }} at {{ site.title }}">	

		public interface Sourceable {  
		    public void method();  
		}  

		public class Source implements Sourceable {  
		  
		    @Override  
		    public void method() {  
		        System.out.println("the original method!");  
		    }  
		}  

		public class Decorator implements Sourceable {  
		  
		    private Sourceable source;  
		      
		    public Decorator(Sourceable source){  
		        super();  
		        this.source = source;  
		    }  
		    @Override  
		    public void method() {  
		        System.out.println("before decorator!");  
		        source.method();  
		        System.out.println("after decorator!");  
		    }  
		} 

		public class DecoratorTest {  
		  
		    public static void main(String[] args) {  
		        Sourceable source = new Source();  
		        Sourceable obj = new Decorator(source);  
		        obj.method();  
		    }  
		} 

> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)