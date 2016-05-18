---
layout: post
title: Design Patterns - Observer
date: 2016-05-15
weather: sunny
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)

# 19. Observer

- **Intent**: 
	- Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

- **Also Known As**:
    - Dependents, Publish-Subscribe

- **Applicability**:
    - When an abstraction has two aspects, one dependent on the other.
    - When a change to one object requires changing others, and you don't know how many objects need to be changed.
    - When an object should be able to notify other objects without making assumptions about who these objects are.
- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Observer.png" alt="{{ page.title }} at {{ site.title }}">


- **Participants**:
	- Subject
	   - knows its observers. Any number of Observer objects may observe a subject.
       - provides an interface for attaching and detaching Observer objects.
    - Observer
        - defines an updating interface for objects that should be notified of changes in a subject.
    - ConcreteSubject
        - stores state of interest to ConcreteObserver objects.
        - sends a notification to its observers when its state changes.
    - ConcreteObserver
        - maintains a reference to a ConcreteSubject object.
        - stores state that should stay consistent with the subject's.
        - implements the Observer updating interface to keep its state consistent with the subject's.

- **Collaborations**
    - ConcreteSubject notifies its observers whenever a change occurs that could make its observers' state inconsistent with its own.
    - After being informed of a change in the concrete subject, a ConcreteObserver object may query the subject for information.ConcreteObserver uses this information to reconcile its state with that of the subject.

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Observer2.png" alt="{{ page.title }} at {{ site.title }}">


   

- **Consequences**:
	- 1.Abstract coupling between Subject and Observer.
	- 2.Support for broadcast communication.
	- 3.Unexpected updates.
	- 4.Defining narrow and wide interfaces.
	- 5.Hidden costs in caring for mementos.

> This problem is aggravated by the fact that the simple update protocolprovides no details on what changed in the subject. Without additional protocol to help observers discover what changed, they maybe forced to work hard to deduce the changes

- **Related Patterns**:
	- Mediator: By encapsulating complex update semantics, the ChangeManager acts as mediator between subjects and observers.
	- Singleton:The ChangeManager may use the Singleton pattern to make it uniqueand globally accessible.

- **Code Sample: Observer**

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/ObserverSample.png" alt="{{ page.title }} at {{ site.title }}">	

        public interface Observer {  
            public void update();  
        }

        public class Observer1 implements Observer {  
          
            @Override  
            public void update() {  
                System.out.println("observer1 has received the notification.");  
            }  
        }  

        public class Observer2 implements Observer {  
          
            @Override  
            public void update() {  
                System.out.println("observer2 has received the notification.");  
            }  
          
        }    

        public interface Subject {  
              
            /*add observer*/  
            public void add(Observer observer);  
              
            /*remove observer*/  
            public void del(Observer observer);  
              
            /*notify ob*/  
            public void notifyObservers();  
              
            /*do sth*/  
            public void operation();  
        }  

        public abstract class AbstractSubject implements Subject {  
          
            private Vector<Observer> vector = new Vector<Observer>();  
            @Override  
            public void add(Observer observer) {  
                vector.add(observer);  
            }  
          
            @Override  
            public void del(Observer observer) {  
                vector.remove(observer);  
            }  
          
            @Override  
            public void notifyObservers() {  
                Enumeration<Observer> observers = vector.elements();  
                while(observers.hasMoreElements()){  
                    observers.nextElement().update();  
                }  
            }  
        }  

        public class MySubject extends AbstractSubject {  
          
            @Override  
            public void operation() {  
                System.out.println("update self!");  
                notifyObservers();  
            }  
          
        } 


> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)
