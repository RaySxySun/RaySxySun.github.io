---
layout: post
title: Effective Java - Item 14 In public classes, use accessor methods, not public fields
date: 2016-09-05
weather: sunny
categories: Java
tags: [Java]
description:
---

> Chapeter 4 Classes and Interfaces

# Item 14: In public classes, use accessor methods, not public fields

1. Degenerate classes: 
	- serve no purpose other than to group instance fields
	- it does not offer the benefits of encapsulation
	- should always be replaced by classes with private fields and public accessor methods (getters)
					
					====================Degenerate classes===================
					// Degenerate classes like this should not be public!
					class Point {
						public double x;
						public double y;
					}

					=============accessor methods and mutators================
					// Encapsulation of data by accessor methods and mutators
					// to preserve the flexibility to change the class’s internal representation.
					class Point {
						private double x;
						private double y;
						public Point(double x, double y) {
							this.x = x;
							this.y = y;
						}
						public double getX() { return x; }
						public double getY() { return y; }
						public void setX(double x) { this.x = x; }
						public void setY(double y) { this.y = y; }
					}

2. violate the advice that public classes should not expose fields directly 

	- if a class is package-private or is a private nested class, there is nothing inherently wrong with exposing its data fields
	- the Point and Dimension classes in the java.awt package
		- it resulted in a serious performance problem that is still with us today.
	- it is
	less harmful if the fields are immutable

> In summary, public classes should never expose mutable fields. 

> (1) For public classes to expose immutable fields. It is less harmful, though still questionable,

> (2) Sometimes desirable for package-private or private nested classes to expose fields, whether mutable or immutable.
