---
layout: post
title: Effective Java - Item 22 Favor static member classes over nonstatic
date: 2016-09-22
weather: sunny
categories: Java
tags: [Java]
description:
---

> Chapeter 4 Classes and Interfaces


# Item 22: Favor static member classes over nonstatic

> This item tells you when to use which kind of nested class and why.


- ### 1.Nested Class
	- [Principle] A nested class should exist only to serve its enclosing class.
		- [otherwise] it should be a top-level class
	- [Types] 4 kinds of nested classes
		- (1) static member classes
		- (2) nonstatic member classes 
		- (3) anonymous classes 
		- (4) local classes
		- [Note] All but the first kind are known as inner classes.

---

- ###2.Static Member Class
	- [comment] it is the simplest kind of nested class.
	- [essential] an ordinary class that happens to be declared inside another class
	- [feature] it has access to all of the enclosing class’s members, even those declared private
	- [common use] 
		- (1) a public helper class
		- (2) private static member classes is to represent components of the object
			- **[Example] Many Map implementations have an internal Entry object**
				- Entry object has getKey, getValue , and setValue
	- [For example] 
		- (1) an enum describing the operations supported by a calculator
		- (2) The Operation enum should be a public static member class of the Calculator class.
		- (3) using operation like Calculator.Operation.PLUS and Calculator.Operation.MINUS

> If you declare a member class that does not require access to an enclosing instance, always put the static modifier in its declaration

---

- ###3.Nonstatic Member Classes
	- (1) without the modifier static
	- (2) it is implicitly associated with an enclosing instance of its containing class.
	- (3) obtain a reference to the enclosing instance using the qualified this construct
	- (4) it is impossible to create an instance of a nonstatic member class without an enclosing instance.
	- [common use] define an Adapter
		- it allows an instance of the outer class to be viewed as an instance of some unrelated class.
		- [For example] implementations of the Map interface typically use nonstatic member classes to implement their collection views

		- An Example: implementations of the collection interfaces, such as Set and List , typically use nonstatic member classes to implement their iterators
						
						===========================Constant Interface===========================
						// Typical use of a nonstatic member class
						public class MySet<E> extends AbstractSet<E> {
							... // Bulk of the class omitted
							public Iterator<E> iterator() {
								return new MyIterator();
							}

							private class MyIterator implements Iterator<E> {
								...
							}
						}
							
> Note: Storing this reference costs time and space, and can result in the enclosing instance being retained when it would otherwise be eligible for garbage collection (Item 6)					

---

- Anonymous Classes:
	- Features:
		- (1) no name
		- (2) not a member of its enclosing class
		- (3) declared and instantiated at the point of use
		- (4) if they occur in a static context, they cannot have any static members.
	- Limitations:
		- (1) can’t instantiate them except at the point they’re declared
		- (2) can’t perform instanceof tests or do anything else that requires you to name the class.
		- (3) can’t declare an anonymous class to implement multiple interfaces, tor o extend a class and implement an interface at the same time
		- (4) can’t invoke any members except those it inherits from its supertype
	- Recommendation 
		- [readability] Because anonymous classes occur in the midst of expressions, they must be kept short (<= 10 lines)
	- Common Use
		- (1) create function objects (Item 21)
		- (2) create process objects, such as **Runnable, Thread, or TimerTask** instances
		- (3) within static factory methods

						===============extends abstract class=======================
						abstract class Person {
						    public abstract void eat();
						}
						 
						class Child extends Person {
						    public void eat() {
						        System.out.println("eat something");
						    }
						}
						 
						public class Demo {
						    public static void main(String[] args) {
						        Person p = new Child();
						        p.eat();
						    }
						}
						===================abstract===================================
						abstract class Person {
						    public abstract void eat();
						}
						 
						public class Demo {
						    public static void main(String[] args) {
						        Person p = new Person() {
						            public void eat() {
						                System.out.println("eat something");
						            }
						        };
						        p.eat();
						    }
						}
						=========================interface============================

						interface Person {
						    public void eat();
						}
						 
						public class Demo {
						    public static void main(String[] args) {
						        Person p = new Person() {
						            public void eat() {
						                System.out.println("eat something");
						            }
						        };
						        p.eat();
						    }
						}
						==============================Thread=============================

						public class Demo {
						    public static void main(String[] args) {
						        Thread t = new Thread() {
						            public void run() {
						                for (int i = 1; i <= 5; i++) {
						                    System.out.print(i + " ");
						                }
						            }
						        };
						        t.start();
						    }
						}
						===============================Runnable===========================
						public class Demo {
						    public static void main(String[] args) {
						        Runnable r = new Runnable() {
						            public void run() {
						                for (int i = 1; i <= 5; i++) {
						                    System.out.print(i + " ");
						                }
						            }
						        };
						        Thread t = new Thread(r);
						        t.start();
						    }
						}

---

- [Local Classes](http://docstore.mik.ua/orelly/java-ent/jnut/ch03_11.htm)
	- Less frequently used
	- Features:
		- Like member classes, they have names and can be used repeatedly
		- Like anonymous classes, they 
			- (1)have enclosing instances only if defined in a nonstatic context
			- (2) cannot contain static members
			- (3) should be kept short so as not to harm readability.
		- without the modifier public，private，protected，static
		- can add final, abstract
		- have access to outer class member
		- NO access to enclosing function's local variable, except final variable.
		- [Most Important] local classes are declared **in functions** 


					==========================Local Classes============================
					public class outerTolocal{
						public String string;
						public int localInt;
						public void outerTolocal(){}

						public void localMthod(final int m ,int n){

							class local{
								// note the variable m must have final 
								int methodInt = m;
								void localInner(){
									System.out.println("local method");
								}
							}
							new local().localInner();
						}
					}



- ### Conclusion
	- (1) [member class]If a nested class needs to be **visible outside of a single method** or is **too long** to fit comfortably inside a method, use a **member class**. 
		- If each instance of the member class **needs a reference** to its enclosing instance, make it **nonstatic**; otherwise, make it **static**.
	- (2) [anonymous & local class] Assuming the class belongs inside a method:
		- if you need to create instances from only one location and **there is a preexisting type that characterizes the class**, make it an anonymous class; otherwise, make it a local class.
	