---
layout: post
title: Design Patterns - Builder
date: 2016-04-20
weather: starry
categories: Design 
tags: [Design]
description: 
---

> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)

# 2. Builder

- **Intent**: 
	- Separate the construction of a complex object from its representation so that the same construction process can create different representations.

- **Applicability**:
	- the algorithm for creating a complex object should be independent of the parts that make up the object and how they're assembled.
	- the construction process must allow different representations for the object that's constructed.

- **Related Patterns**:
	- **A Composite is what the builder often builds.**
		- Abstract Factory is similar to Builder in that it too may construct complex objects. The primary difference is that the Builder pattern focuses on constructing a complex object step by step. Abstract Factory's emphasis is on families of product objects (either simple or complex). Builder returns the product as a final step, but as far as the Abstract Factory pattern is concerned, the product gets returned immediately.

- **Motivation**:	
	- A reader for the RTF (Rich Text Format) document exchange format should be able to convert RTF to many text formats. The reader might convert RTF documents into plain ASCII text or into a text widget that can be edited interactively. The problem, however, is that the number of possible conversions is open-ended. So it should be easy to add a new conversion without modifying the reader.

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/Builder.png" alt="{{ page.title }} at {{ site.title }}">

- **Structure**:	

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/BuilderStructure.png" alt="{{ page.title }} at {{ site.title }}">

- **Participants**:
	- Builder (TextConverter)
		- specifies an abstract **interface** for creating parts of a Product object.
	- ConcreteBuilder (ASCIIConverter, TeXConverter, TextWidgetConverter)
		- constructs and assembles parts of the product by implementing the Builder interface.
		- defines and keeps track of the representation it creates.
		- provides an interface for retrieving the product (e.g., GetASCIIText, GetTextWidget).
	- Director (RTFReader)
		- constructs an object using the Builder interface.
	- Product (ASCIIText, TeXText, TextWidget)
		- represents the complex object under construction. ConcreteBuilder builds the product's internal representation and defines the process by which it's assembled.
		- includes classes that define the constituent parts, including interfaces for assembling the parts into the final result.
- **Collaborations**
	- The client creates the Director object and configures it with the desired Builder object.
	- Director notifies the builder whenever a part of the product should be built.
	- Builder handles requests from the director and adds parts to the product.
	- The client retrieves the product from the builder.

> Interaction diagram

<img src="{{ site.url }}/assets/img/2016-04-18-DesignPatterns/BuilderCollaborations.png" alt="{{ page.title }} at {{ site.title }}">

- **Consequences**:
	- 1. It lets you vary a product's internal representation.
	- 2. It isolates code for construction and representation.
	- 3. It gives you finer control over the construction process.


- Code Sample
		
		package com.test.DesignPattern; 
		 
		class Product 
		{ 
		    @Override 
		    public String toString() { 
		        return "Product"; 
		    }  
		} 
		 
		interface Builder 
		{ 
		    void BuildPart(); 
		} 
		 
		class ConcreteBuilder implements Builder 
		{ 
		     
		    @Override 
		    public void BuildPart() { 
		        System.out.println("Building Product's part"); 
		    } 
		     
		    public Product GetResult() 
		    { 
		        System.out.println("Product's Building has finished"); 
		        return new Product(); 
		          
		    } 
		} 
		 
		class Director 
		{ 
		    public Product Construct(ConcreteBuilder cb) 
		    { 
		        cb.BuildPart(); 
		        return cb.GetResult(); 
		    } 
		} 
		 
		public class BuilderDemo { 
		 
		    /** 
		     * @param args 
		     */ 
		    public static void main(String[] args) { 
		        // TODO Auto-generated method stub 
		        Director d=new Director(); 
		        ConcreteBuilder cb=new ConcreteBuilder(); 
		        Product p=d.Construct(cb); 
		        System.out.println(p); 
		    } 
		} 

> Build a car

		package com.test.DesignPattern; 
		 
		 
		class Car{ 
		 
		    @Override 
		    public String toString() { 
		        return "汽车产品"; 
		    } 
		} 
		 
		interface  IBuilder 
		{ 
		    void BuildWheel(); 
		    void BuildCarFrame(); 
		    void BuildEngine();  
		} 
		 
		class CarBuilder implements IBuilder 
		{ 
		 
		    private Car car=new Car(); 
		    @Override 
		    public void BuildCarFrame() { 
		        // TODO Auto-generated method stub 
		        System.out.println("建造汽车框架"); 
		    } 
		 
		    @Override 
		    public void BuildEngine() { 
		        // TODO Auto-generated method stub 
		        System.out.println("建造汽车轮子"); 
		    } 
		 
		    @Override 
		    public void BuildWheel() { 
		        // TODO Auto-generated method stub 
		        System.out.println("建造汽车引擎"); 
		    } 
		     
		    public Car CarBuildOver() 
		    { 
		        System.out.println("汽车组装完毕"); 
		        return car; 
		    } 
		} 
		 
		class CarDirector 
		{ 
		     
		    public Car BuildCar(CarBuilder cb) 
		    { 
		        cb.BuildCarFrame(); 
		        cb.BuildEngine(); 
		        cb.BuildWheel(); 
		        return cb.CarBuildOver(); 
		    } 
		} 
		 
		public class BuliderDemoInLife { 
		 
		    /** 
		     * @param args 
		     */ 
		    public static void main(String[] args) { 
		        // TODO Auto-generated method stub 
		        CarDirector cd=new CarDirector(); 
		        Car c=cd.BuildCar(new CarBuilder()); 
		        System.out.println(c); 
		    } 
		} 

> [Design Patterns Category](http://raysxysun.github.io/categories/#Design)

> [Design Patterns Index](http://raysxysun.github.io/design/2016/04/18/DesignPatterns/)