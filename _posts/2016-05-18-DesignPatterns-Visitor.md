---
layout: post
title: Design Patterns - Visitor
date: 2016-05-18
weather: starry
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)

# 23. Visitor

- **Intent**: 
	- Represent an operation to be performed on the elements of an objects tructure. Visitor lets you define a new operation without changing the classes of the elements on which it operates.

- **Applicability**:
    - an object structure contains many classes of objects with differing interfaces, and you want to perform operations on these objects that depend on their concrete classes.
    - many distinct and unrelated operations need to be performed on objects in an object structure, and you want to avoid "polluting" their classes with these operations.
    - the classes defining the object structure rarely change, but you often want to define new operations over the structure.
- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Visitor.png" alt="{{ page.title }} at {{ site.title }}">


- **Participants**:
	- Visitor
	   - declares a Visit operation for each class of ConcreteElement in the object structure. The operation's name and signature identifies the class that sends the Visit request to the visitor. That lets the visitor determine the concrete class of the element being visited. Then the visitor can access the element directly through its particular interface.
    - ConcreteVisitor
        - implements each operation declared by Visitor. Each operation implements a fragment of the algorithm defined for the corresponding class of object in the structure. ConcreteVisitor provides the context for the algorithm and stores its local state. This state often accumulates results during the traversal of the structure.
    - Element
        - defines an Accept operation that takes a visitor as an argument.
    - ConcreteElement
        - implements an Accept operation that takes a visitor as an argument.
    - ObjectStructure
        - can enumerate its elements.
        - may provide a high-level interface to allow the visitor to visit its elements.
        - may either be a composite or a collection such as a list or a set.
- **Collaborations**
    - A client that uses the Visitor pattern must create a ConcreteVisitorobject and then traverse the object structure, visiting each element with the visitor.
    - When an element is visited, it calls the Visitor operation that corresponds to its class. The element supplies itself as an argument to this operation to let the visitor access its state, if necessary.

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Visitor2.png" alt="{{ page.title }} at {{ site.title }}">

- **Consequences**:
	- 1.Visitor makes adding new operations easy.
	- 2.A visitor gathers related operations and separates unrelated ones.
	- 3.Adding new ConcreteElement classes is hard.
	- 4.Visiting across class hierarchies.

- **Related Patterns**:
	- Composite: Visitors can be used to apply an operation over an object structure defined by the Composite pattern.
    - Interpreter: Visitor may be applied to do the interpretation.
- **Code Sample: Strategy**

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/VisitorSample.png" alt="{{ page.title }} at {{ site.title }}">	

        public interface Visitor {  
            public void visit(Subject sub);  
        }  

        public class MyVisitor implements Visitor {  
          
            @Override  
            public void visit(Subject sub) {  
                System.out.println("visit the subject："+sub.getSubject());  
            }  
        }  

        public interface Subject {  
            public void accept(Visitor visitor);  
            public String getSubject();  
        }    


        public class MySubject implements Subject {  
          
            @Override  
            public void accept(Visitor visitor) {  
                visitor.visit(this);  
            }  
          
            @Override  
            public String getSubject() {  
                return "love";  
            }  
        } 

        public class Test {  
          
            public static void main(String[] args) {  
                  
                Visitor visitor = new MyVisitor();  
                Subject sub = new MySubject();  
                sub.accept(visitor);      
            }  
        }  

> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)
