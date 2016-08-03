---
layout: post
title: Effective Java - Item 20 Prefer class hierarchies to tagged classes
date: 2016-09-21
weather: sunny
categories: Java
tags: [Java]
description:
---

> Chapeter 4 Classes and Interfaces


# Item 20: Prefer class hierarchies to tagged classes

> tagged classes are verbose, error-prone, and inefficient.
> Occasionally you may run across a class whose instances come in two or more flavors and contain a tag field indicating the flavor of the instance.

- ### 1.Tagged Classes
	- [Typical Structure] cluttered with boilerplate
		- including (1)enum declarations, (2)tag fields, and (3)switch statements.
	- An Example
						
						===========================Constant Interface===========================
						// Tagged class - vastly inferior to a class hierarchy!
						class Figure {
							enum Shape { RECTANGLE, CIRCLE };
							
							// Tag field - the shape of this figure
							final Shape shape;
							
							// These fields are used only if shape is RECTANGLE
							double length;
							double width;
							
							// This field is used only if shape is CIRCLE
							double radius;
							
							// Constructor for circle
							Figure(double radius) {
								shape = Shape.CIRCLE;
								this.radius = radius;
							}
							
							// Constructor for rectangle
							Figure(double length, double width) {
								shape = Shape.RECTANGLE;
								this.length = length;
								this.width = width;
							}
							
							double area() {
								switch(shape) {
									case RECTANGLE:
									return length * width;
									case CIRCLE:
									return Math.PI * (radius * radius);
									default:
									throw new AssertionError();
								}
							}
						}

---

- ### 2.shortcomings of tagged classes
	- (1) Readability is further harmed 
		- [Reason] because multiple implementations are jumbled together in a single class.
	- (2) Memory footprint is increased
		- [Reason] because instances are burdened with irrelevant fields belonging to other flavors.
	- (3) Fields can’t be made final
		- Fields can’t be made final unless constructors initialize irrelevant fields, resulting in more boilerplate.
	- (4) no help from the compiler
		- Constructors must set the tag field and initialize the right data fields with no help from the compiler
		- [i.e.] if you initialize the wrong fields, the program will fail at runtime.
	- (5) can’t add a flavor to a tagged class unless modifying its source file
	- (6) If you do add a flavor, you must remember to add a case to every switch statement
	- (7) the data type of an instance gives no clue as to its flavor

---

- ###3.A better alternative: class hierarchy 
	- object-oriented languages can offer better way to define a single data type capable of representing objects of multiple flavors
	- [alternative] subtyping
	- [Benefits]
		- (1) Containing none of the boilerplate
		- (2) The implementation of each flavor is allotted its own class
		- (3) These classes are encumbered by irrelevant data fields.
		- (4) All fields are final
		- (5) The compiler ensures that each class’s constructor initializes its data fields
		- (6) eliminates the possibility of a runtime failure due to a missing switch case.
		- (7) can extend the hierarchy independently without access to the source for the root class

> A tagged class is just a pallid imitation of a class hierarchy

---

- ###4.Method: To transform a tagged class into a class hierarchy
	- (1) define an abstract class containing an abstract method for each method in the tagged class whose behavior depends on the tag value. 
		- 1) If there are any methods whose behavior does not depend on the value of the tag, put them in this class
		- 2) if there are any data fields used by all the flavors, put them in this class.
	- (2) Next, define a concrete subclass of the root class for each flavor of the original tagged class.

	===================Transform a tagged class===================
	
	// It corrects every shortcoming of tagged classes noted previously.
	// containing none of the boilerplate

	abstract class Figure {
		abstract double area();
	}

	class Circle extends Figure {
		final double radius;
		Circle(double radius) { this.radius = radius; }
		double area() { return Math.PI * (radius * radius); }
	}

	class Rectangle extends Figure {
		final double length;
		final double width;
		Rectangle(double length, double width) {
			this.length = length;
			this.width = width;
		}
		double area() { return length * width; }
	}

	class Square extends Rectangle {
		Square(double side) {
			super(side, side);
		}
	}

> Note: the fields in the above hierarchy are **accessed directly rather than by accessor methods**. This was done for brevity and would be **unacceptable** if the hierarchy were public [(Item 14)](http://raysxysun.github.io/java/2016/09/05/Effective-Java14/) .

---


- ### Conclusion
	- tagged classes are seldom **appropriate**.
	