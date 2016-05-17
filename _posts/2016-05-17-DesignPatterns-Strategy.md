---
layout: post
title: Design Patterns - Strategy
date: 2016-05-17
weather: starry
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)

# 21. Strategy

- **Intent**: 
	- Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

- **Also Known As**:
    - Policy

- **Applicability**:
    - many related classes differ only in their behavior.
    - you need different variants of an algorithm.
    - an algorithm uses data that clients shouldn't know about.
    - a class defines many behaviors, and these appear as multipleconditional
statements in its operations.
- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Strategy.png" alt="{{ page.title }} at {{ site.title }}">


- **Participants**:
	- Strategy
	   - declares an interface common to all supported algorithms. Context uses this interface to call the algorithm defined by a ConcreteStrategy.
    - ConcreteStrategy
        - implements the algorithm using the Strategy interface.
    - Context
        - is configured with a ConcreteStrategy object.
        - maintains a reference to a Strategy object.
        - may define an interface that lets Strategy access its data.

- **Collaborations**
    - Strategy and Context interact to implement the chosen algorithm.
    - A context forwards requests from its clients to its strategy.

- **Consequences**:
	- 1.Families of related algorithms.
	- 2.An alternative to subclassing.
	- 3.Strategies eliminate conditional statements.
	- 4.A choice of implementations.
	- 5.Clients must be aware of different Strategies.
	- 6.Communication overhead between Strategy and Context.
	- 7.Increased number of objects.

- **Related Patterns**:
	- Flyweight: Strategy objects often make good flyweights.
- **Code Sample: Strategy**

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/StrategySample.png" alt="{{ page.title }} at {{ site.title }}">	

        public interface ICalculator {  
            public int calculate(String exp);  
        }  
        
        public abstract class AbstractCalculator {  
              
            public int[] split(String exp,String opt){  
                String array[] = exp.split(opt);  
                int arrayInt[] = new int[2];  
                arrayInt[0] = Integer.parseInt(array[0]);  
                arrayInt[1] = Integer.parseInt(array[1]);  
                return arrayInt;  
            }  
        }  
        
        
        public class Plus extends AbstractCalculator implements ICalculator {  
          
            @Override  
            public int calculate(String exp) {  
                int arrayInt[] = split(exp,"\\+");  
                return arrayInt[0]+arrayInt[1];  
            }  
        }  
        
        public class Minus extends AbstractCalculator implements ICalculator {  
          
            @Override  
            public int calculate(String exp) {  
                int arrayInt[] = split(exp,"-");  
                return arrayInt[0]-arrayInt[1];  
            }  
          
        }  
        
        public class Multiply extends AbstractCalculator implements ICalculator {  
          
            @Override  
            public int calculate(String exp) {  
                int arrayInt[] = split(exp,"\\*");  
                return arrayInt[0]*arrayInt[1];  
            }  
        }  
        
        public class StrategyTest {  
          
            public static void main(String[] args) {  
                String exp = "2+8";  
                ICalculator cal = new Plus();  
                int result = cal.calculate(exp);  
                System.out.println(result);  
            }  
        }  


> [Design Patterns Index](http://raysxysun.github.io/categories/#Design)
