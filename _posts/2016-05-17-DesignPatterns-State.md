---
layout: post
title: Design Patterns - State
date: 2016-05-17
weather: sunny
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)

# 20. State

- **Intent**: 
	- Allow an object to alter its behavior when its internal state changes.The object will appear to change its class.

- **Also Known As**:
    - Objects for States

- **Applicability**:
    - An object's behavior depends on its state, and it must change its behavior at run-time depending on that state.
    - Operations have large, multipart conditional statements that depend onthe object's state. This state is usually represented by one or moreenumerated constants.
- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/State.png" alt="{{ page.title }} at {{ site.title }}">


- **Participants**:
	- Context
	   - defines the interface of interest to clients. 
       - maintains an instance of a ConcreteState subclass that defines the current state.
    - State
        - defines an interface for encapsulating the behavior associated with aparticular state of the Context.
    - ConcreteState subclasses
        - each subclass implements a behavior associated with a state ofthe Context.

- **Collaborations**
    - Context delegates state-specific requests to the current ConcreteState object.
    - A context may pass itself as an argument to the State object handling the request. This lets the State object access the context if necessary.
    - Context is the primary interface for clients. Clients can configure a context with State objects. Once a context is configured, its clients don't have to deal with the State objects directly.
    - Either Context or the ConcreteState subclasses can decide which state succeeds another and under what circumstances.

- **Consequences**:
	- 1.It localizes state-specific behavior and partitions behavior for different states.
	- 2.It makes state transitions explicit.
	- 3.State objects can be shared.

- **Related Patterns**:
	- The Flyweight pattern explains when and how State objects can be shared.
    - State objects are often Singletons.
- **Code Sample: State**

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/StateSample.png" alt="{{ page.title }} at {{ site.title }}">	

        package com.sample.state;  
          
        public class State {  
              
            private String value;  
              
            public String getValue() {  
                return value;  
            }  
          
            public void setValue(String value) {  
                this.value = value;  
            }  
          
            public void method1(){  
                System.out.println("execute method1");  
            }  
              
            public void method2(){  
                System.out.println("execute method2");  
            }  
        }  

        ---

        package com.sample.state;  
          
        public class Context {  
          
            private State state;  
          
            public Context(State state) {  
                this.state = state;  
            }  
          
            public State getState() {  
                return state;  
            }  
          
            public void setState(State state) {  
                this.state = state;  
            }  
          
            public void method() {  
                if (state.getValue().equals("state1")) {  
                    state.method1();  
                } else if (state.getValue().equals("state2")) {  
                    state.method2();  
                }  
            }  

        ---

        public class Test {  
          
            public static void main(String[] args) {  
                  
                State state = new State();  
                Context context = new Context(state);  
                  
                //switch state1  
                state.setValue("state1");  
                context.method();  
                  
                //switch state2 
                state.setValue("state2");  
                context.method();  
            }  
        }  


> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)
