---
layout: post
title: Design Patterns - Memento
date: 2016-05-15
weather: sunny
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)

# 18. Mediator

- **Intent**: 
	- Without violating encapsulation, capture and externalize an object's internal state so that the object can be restored to this state later.

- **Also Known As**:
    - Token

- **Applicability**:
    - a snapshot of (some portion of) an object's state must be saved so that it can be restored to that state later, and
    - a direct interface to obtaining the state would expose implementation details and break the object's encapsulation.
- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Memento.png" alt="{{ page.title }} at {{ site.title }}">


- **Participants**:
	- Memento
	   - stores internal state of the Originator object. The memento may store as much or as little of the originator's internal state as necessary at its originator's discretion.
       - protects against access by objects other than the originator. Mementos have effectively two interfaces. Caretaker sees a narrow interface to the Memento—it can only pass the memento to other objects. Originator, in contrast, sees a wide interface, one that lets it access all the data necessary to restore itself to its previous state. Ideally, only the originator that produced the memento would be permitted to access the memento's internal state.
    - Originator
        - creates a memento containing a snapshot of its current internal state.
        - uses the memento to restore its internal state.
    - Caretaker
        - is responsible for the memento's safekeeping.
        - never operates on or examines the contents of a memento.

- **Collaborations**
	- A caretaker requests a memento from an originator, holds it for a time, and passes it back to the originator. Sometimes the caretaker won't pass the memento back to the originator,because the originator might never need to revert to an earlier state.
     - Mementos are passive. Only the originator that created a memento will assign or retrieve its state.

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Memento2.png" alt="{{ page.title }} at {{ site.title }}">


   

- **Consequences**:
	- 1.Preserving encapsulation boundaries.
	- 2.It simplifies Originator.
	- 3.Using mementos might be expensive.
	- 4.Defining narrow and wide interfaces.
	- 5.Hidden costs in caring for mementos.

- **Related Patterns**:
	- Command: Commands can use mementos to maintain state for undoable operations.
	- Iterator: Mementos can be used for iteration as described earlier.

- Code Sample: Memento

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/MementoSample.png" alt="{{ page.title }} at {{ site.title }}">	

        public class Original {  
              
            private String value;  
              
            public String getValue() {  
                return value;  
            }  
          
            public void setValue(String value) {  
                this.value = value;  
            }  
          
            public Original(String value) {  
                this.value = value;  
            }  
          
            public Memento createMemento(){  
                return new Memento(value);  
            }  
              
            public void restoreMemento(Memento memento){  
                this.value = memento.getValue();  
            }  
        }  

        public class Memento {  
              
            private String value;  
          
            public Memento(String value) {  
                this.value = value;  
            }  
          
            public String getValue() {  
                return value;  
            }  
          
            public void setValue(String value) {  
                this.value = value;  
            }  
        }  

        public class Storage {  
              
            private Memento memento;  
              
            public Storage(Memento memento) {  
                this.memento = memento;  
            }  
          
            public Memento getMemento() {  
                return memento;  
            }  
          
            public void setMemento(Memento memento) {  
                this.memento = memento;  
            }  
        }  

        public class Test {  
          
            public static void main(String[] args) {  
                  
                // create original instance
                Original origin = new Original("origin");  
          
                // create a memento  
                Storage storage = new Storage(origin.createMemento());  
          
                // change the original object 
                System.out.println("current status：" + origin.getValue());  
                origin.setValue("modified");  
                System.out.println("modified status：" + origin.getValue());  
          
                // after recovery  
                origin.restoreMemento(storage.getMemento());  
                System.out.println("after recovery:" + origin.getValue());  
            }  
        }  



> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)