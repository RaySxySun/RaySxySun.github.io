---
layout: post
title: Effective Java - Item 17 Design and document for inheritance or else prohibit it
date: 2016-09-14
weather: sunny
categories: Java
tags: [Java]
description:
---

> Chapeter 4 Classes and Interfaces


# Item 17: Design and document for inheritance or else prohibit it

- ### 1.the class must document its self-use of overridable methods.
- ### 2.a class may have to provide hooks into its internal workings in the form of judiciously chosen protected methods
- ### 3. The only way to test a class designed for inheritance is to write subclasses.
- ### 4. Constructors must not invoke overridable methods, directly or indirectly
- ### 5. It is generally not a good idea for a class designed for inheritance to implement either of these interfaces
	- The Cloneable and Serializable interfaces present special difficulties when designing for inheritance.
	- If you do decide to implement Cloneable or Serializable
		- **neither clone nor readObject may invoke an overridable method, directly or indirectly**
- ### 6.prohibit subclassing: all the constructors private or package-private and to add public static factories in place of the constructors

- ### 7. If you feel that you must allow inheritance from a concrete class does not implement a standard interface, ensure that the class never invokes any of its overridable methods and to document this fact.
	- eliminate the class’s self-use of overridable methods entirely
