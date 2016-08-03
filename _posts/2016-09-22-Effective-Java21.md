---
layout: post
title: Effective Java - Item 21 Use function objects to represent strategies
date: 2016-09-22
weather: sunny
categories: Java
tags: [Java]
description:
---

> Chapeter 4 Classes and Interfaces


# Item 21: Use function objects to represent strategies

> Some languages support function pointers, delegates, lambda expressions, or similar facilities
> They are typically used to allow the caller of a function to specialize its behavior by passing in a second function.
> For example, the **qsort function in C’s standard library** takes a pointer to **a comparator function**
> This is the [Strategy pattern](http://raysxysun.github.io/design/2016/05/17/DesignPatterns-Strategy/)

- ### 1.object references
	- [Workaround]Java does not provide function pointers, but object references can be used to achieve a similar effect.
	- [Essential]It is possible to define an object whose methods perform operations on **other objects**
						
						===========================function object===========================
						// a StringLengthComparator instance is a concrete strategy for string comparison
						// it is stateless
						class StringLengthComparator {
							public int compare(String s1, String s2) {
								return s1.length() - s2.length();
							}
						}
						--------------------[Item 3 & 5]Singleton----------------------------
						// stateless: it has no fields, hence all instances of the class are functionally equivalent. Thus it should be a singleton to save on unnecessary object creation costs (Item 3, Item 5)

						class StringLengthComparator {
							private StringLengthComparator() { }
							public static final StringLengthComparator INSTANCE = new StringLengthComparator();
							public int compare(String s1, String s2) {
								return s1.length() - s2.length();
							}
						}

						// Disadvantage: To pass a StringLengthComparator instance to a method, we need an appropriate type for the parameter.
						// [Improvement] we need to define a Comparator interface

						----------------------strategy interface----------------------------
						// Strategy interface
						// T is a formal type parameter
						public interface Comparator<T> {
							public int compare(T t1, T t2);
						}

						class StringLengthComparator implements Comparator<String> {
							... // class body is identical to the one shown above
						}

						// However the Concrete strategy classes are often declared using anonymous classes (Item22)
						---------------------anonymous classes-------------------------------
						Arrays.sort(stringArray, new Comparator<String>() {
							public int compare(String s1, String s2) {
								return s1.length() - s2.length();
							}
						});

> note that using an anonymous class in this way will create a new instance each time the call is executed. If it is to be executed repeatedly, consider storing the function object in a private static final field and reusing it.

---

- ### 2.strategy interface
	- (1) [pros] the strategy interface serves as a type, so a concrete strategy class needn’t be made public to export a concrete strategy
		- [executed repeatedly] a “host class” can export a public static field (or static factory method) whose type is the strategy interface
		- [executed recursively] the concrete strategy class can be a private nested class of the host

					======================Exporting a concrete strategy=======================
					// Exporting a concrete strategy
					class Host {
						private static class StrLenCmp implements Comparator<String>, Serializable {
							public int compare(String s1, String s2) {
								return s1.length() - s2.length();
							}
						}
						// Returned comparator is serializable
						public static final Comparator<String>
						STRING_LENGTH_COMPARATOR = new StrLenCmp();
						...
						// Bulk of class omitted
					}
		
---

- ### Conclusion
	- a primary use of function pointers is to implement the Strategy pattern
	- To implement this pattern in Java
		- (1) declare an interface
		- (2) declare a class that implements this interface
			- if used only once, using an anonymous class.
			- if designed for repeated use, it should be implemented as a private static member class and exported in a public static final field (whose type is the strategy interface)
	