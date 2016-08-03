---
layout: post
title: Effective Java - Item 16 Favor composition over inheritance
date: 2016-09-06
weather: sunny
categories: Java
tags: [Java]
description:
---

> Chapeter 4 Classes and Interfaces

> To summarize, inheritance is powerful, but it is problematic because it violates encapsulation.

> Inheriting from ordinary concrete classes across package boundaries is dangerous.

# Item 16: Favor composition over inheritance

- ### 1.Unlike method invocation, inheritance violates encapsulation
	- it is safe to extend a class if merely adding new methods and refraining from overriding existing methods.
	- [Best Solution]composition-and-forwarding: the class itself and a reusable forwarding class
		- [Design Pattern] composition, give your new class a private field that references an instance of the existing class. 
		- [Design pattern] Decorator pattern, The InstrumentedSet class is known as a wrapper class because each InstrumentedSet instance contains (“wraps”) another Set instance
		- [Comparison]
			- [1] inheritance-based class works only for a single concrete class and requires a separate constructor for each supported constructor in the superclass
			- [2] the wrapper class can be used to instrument any Set implementation and will work in conjunction with any preexisting constructor
		- [Disadvantages ?] wrapper classes are not suited for use in **callback frameworks**
			- a wrapped object doesn’t know of its wrapper, it passes a reference to itself ( this ) and callbacks elude the wrapper.


					// Sometimes the combination of composition and forwarding is loosely referred to asdelegation.
					// Technically it’s not delegation unless the wrapper object passes itself to the wrapped object
					=============composition-and-forwarding: robust & flexible============
					// Wrapper class - uses composition in place of inheritance
					public class InstrumentedSet<E> extends ForwardingSet<E> {
						private int addCount = 0;
						public InstrumentedSet(Set<E> s) {
							super(s);
						}
						@Override public boolean add(E e) {
						addCount++;
							return super.add(e);
						}
						@Override public boolean addAll(Collection<? extends E> c) {
							addCount += c.size();
							return super.addAll(c);
						}
						public int getAddCount() {
							return addCount;
						}
					}

					// Reusable forwarding class
					public class ForwardingSet<E> implements Set<E> {
						private final Set<E> s;
						public ForwardingSet(Set<E> s) { this.s = s; }

						public void clear() { s.clear(); }
						public boolean addAll(Collection<? extends E> c) { return s.addAll(c);}
						...
					}

					// The design of the InstrumentedSet class is enabled by the existence of the Set interface
					=============composition-and-forwarding: robust & flexible============

- ### 2.Is every B really an A?
	- If you cannot truthfully answer yes to this question, B should not extend A.
	- If the answer is no, it is often the case that B should contain a private instance of A and expose a smaller and simpler API

