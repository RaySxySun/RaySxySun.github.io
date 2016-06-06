---
layout: post
title: Design Patterns - Template Method
date: 2016-05-18
weather: starry
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)

# 22. Template Method

- **Intent**: 
	- Define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.

- **Applicability**:
    - to implement the invariant parts of an algorithm once and leave it upto subclasses to implement the behavior that can vary.
    - when common behavior among subclasses should be factored and localized in a common class to avoid code duplication.
    - to control subclasses extensions.
- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/TemplateMethod.png" alt="{{ page.title }} at {{ site.title }}">


- **Participants**:
	- AbstractClass 
	   - defines abstract primitive operations that concrete subclasses define to implement steps of an algorithm.
       - implements a template method defining the skeleton of an algorithm.The template method calls primitive operations as well as operations defined in AbstractClass or those of other objects.
    - ConcreteClass
        - implements the primitive operations to carry out subclass-specific steps of the algorithm.

- **Collaborations**
    - ConcreteClass relies on AbstractClass to implement the invariant steps of the algorithm.

- **Consequences**:

> Template methods are a fundamental technique for code reuse. Template methods lead to an inverted control structure that's sometimes referred to as "the Hollywood principle," that is, "Don'tcall us, we'll call you"

- **Related Patterns**:
	- Factory Methods are often called by template methods. In the Motivation example,the factory method DoCreateDocument is called by the template method OpenDocument.
    - Strategy: Template methods use inheritance to vary part of an algorithm. Strategies use delegation to vary the entire algorithm.
- **Code Sample: Template Method**

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/TemplateMethodSample.png" alt="{{ page.title }} at {{ site.title }}">	

        public abstract class AbsCalculator {  
              
            public final int calculate(String exp,String opt){  
                int array[] = split(exp,opt);  
                return calculate(array[0],array[1]);  
            }  
               
            abstract public int calculate(int num1,int num2);  
              
            public int[] split(String exp,String opt){  
                String array[] = exp.split(opt);  
                int arrayInt[] = new int[2];  
                arrayInt[0] = Integer.parseInt(array[0]);  
                arrayInt[1] = Integer.parseInt(array[1]);  
                return arrayInt;  
            }  
        }    

        public class Plus extends AbsCalculator {  
          
            @Override  
            public int calculate(int num1,int num2) {  
                return num1 + num2;  
            }  
        } 

        public class StrategyTest {  
          
            public static void main(String[] args) {  
                String exp = "10+2";  
                AbsCalculator cal = new Plus();  
                int result = cal.calculate(exp, "\\+");  
                System.out.println(result);  
            }  
        }  


> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)