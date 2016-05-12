---
layout: post
title: Design Patterns - Mediator
date: 2016-05-12
weather: rainy
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)

# 17. Mediator

- **Intent**: 
	- Define an object that encapsulates how a set of objects interact.Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently.

- **Applicability**:
    - a set of objects communicate in well-defined but complex ways.
    - reusing an object is difficult because it refers to and communicates with many other objects.
    - a behavior that's distributed between several classes should be customizable without a lot of subclassing.
- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Mediator1.png" alt="{{ page.title }} at {{ site.title }}">

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Mediator2.png" alt="{{ page.title }} at {{ site.title }}">

- **Participants**:
	- Mediator
	   - defines an interface for communicating with Colleague objects.
    - ConcreteMediator
        - implements cooperative behavior by coordinating Colleague objects.
        - knows and maintains its colleagues.
    - Colleague classes
        - each Colleague class knows its Mediator object.
        - each colleague communicates with its mediator whenever it would have otherwise communicated with another colleague.

- **Collaborations**
	- Colleagues send and receive requests from a Mediator object. The mediator implements the cooperative behavior by routing requests between the appropriate colleague(s).
- **Consequences**:
	- 1.It limits subclassing.
	- 2.It decouples colleagues.
	- 3.It simplifies object protocols.
	- 4.It abstracts how objects cooperate.
	- 5.It centralizes control.

- **Related Patterns**:
	- Facade differs from Mediator in that it abstracts a subsystem of objects to provide a more convenient interface. Its protocol is unidirectional; that is, Facade objects make requests of the subsystem classes but not vice versa. In contrast, Mediator enables cooperative behavior that colleague objects don't or can't provide, and the protocol ismultidirectional.
	- Colleagues can communicate with the mediator using the Observer pattern.

- Code Sample: Mediator

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/MediatorSample.png" alt="{{ page.title }} at {{ site.title }}">	

        public interface Mediator {  
            public void createMediator();  
            public void workAll();  
        }  
        
        public class MyMediator implements Mediator {  
          
            private User user1;  
            private User user2;  
              
            public User getUser1() {  
                return user1;  
            }  
          
            public User getUser2() {  
                return user2;  
            }  
          
            @Override  
            public void createMediator() {  
                user1 = new User1(this);  
                user2 = new User2(this);  
            }  
          
            @Override  
            public void workAll() {  
                user1.work();  
                user2.work();  
            }  
        }   
        
        public abstract class User {  
              
            private Mediator mediator;  
              
            public Mediator getMediator(){  
                return mediator;  
            }  
              
            public User(Mediator mediator) {  
                this.mediator = mediator;  
            }  
          
            public abstract void work();  
        }  
        
        public class User1 extends User {  
          
            public User1(Mediator mediator){  
                super(mediator);  
            }  
              
            @Override  
            public void work() {  
                System.out.println("user1 exe!");  
            }  
        }  
        
        public class User2 extends User {  
          
            public User2(Mediator mediator){  
                super(mediator);  
            }  
              
            @Override  
            public void work() {  
                System.out.println("user2 exe!");  
            }  
        }  
        
        public class Test {  
          
            public static void main(String[] args) {  
                Mediator mediator = new MyMediator();  
                mediator.createMediator();  
                mediator.workAll();  
            }  
        }  



> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)