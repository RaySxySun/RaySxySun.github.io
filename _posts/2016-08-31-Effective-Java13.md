---
layout: post
title: Effective Java - Item 13 Minimize the accessibility of classes and members
date: 2016-08-31
weather: sunny
categories: Java
tags: [Java]
description:
---

> Chapeter 4 Classes and Interfaces

# Item 13 Minimize the accessibility of classes and members

- 1.A fundamental software design tenet: information hiding or encapsulation
	- it decouples the modules that comprise a system; 
		- (1) allow modules to be [1]developed, [2]tested, [3]optimized, [4]used, [5]understood, and [6]modified in isolation
		- (2) can be developed in parallel 
		- (3) It eases the burden of maintenance
		- (4) modules can be optimized without affecting the correctness of other modules
		- (5) information hiding decreases the risk in building large systems
- 2.facilities to aid in information hiding(access control)
	- The access control mechanism specifies the accessibility of classes, interfaces, and members.
		- access modifiers: private , protected , and public
	- For top-level (non-nested) classes and interfaces
		- TWO possible access levels: package-private and public.
	- For package-private top-level class (or interface) is used by only one class
		- consider making the top-level class a private nested class of the sole class that uses it (Item 22)
	- For members: (fields, methods, nested classes, and nested interfaces)
		- four possible access levels: private, package-private, protected, public
	- For instances:
		- Instance fields should never be public, **classes with public mutable fields are not thread-safe**, even if a field is final and refers to an immutable object
	- For static final fields:
		- he same advice applies to static fields. expose constants via public static final fields. such field can contain either primitive values or references to a immutable objects. If a final field containing a reference to a mutable object has all the disadvantages of a nonfinal field.
	- **The rule of thumb is simple: make each class or member as inaccessible as possible**
	- one rule that restricts your ability to reduce the accessibility of methods.
		- If a method overrides a superclass method, it is not permitted to have a lower access level in the subclass than it does in the superclass
		- **[A special case: implicitly public]** if a class implements an interface, all of the class methods that are also present in the interface must be declared public.

- 3.the exception of public static final fields:
	- public classes should have no public fields.
	- Ensure that objects referenced by public static final fields are immutable.

				// it is wrong for a class to have a public static final array field, or an accessor that returns such a field

				===============security hole======================
				// Potential security hole!
				public static final Thing[] VALUES = { ... };
				==============Alternative 1=======================
				private static final Thing[] PRIVATE_VALUES = { ... };
				public static final List<Thing> VALUES =
					Collections.unmodifiableList(Arrays.asList(PRIVATE_VALUES));
				==============Alternative 2=======================
				private static final Thing[] PRIVATE_VALUES = { ... };
				public static final Thing[] values() {
					return PRIVATE_VALUES.clone();
				}

> Prevent any stray classes, interfaces, or members from becoming a part of the API







