---
layout: post
title: Effective Java - Item 23 Don’t use raw types in new code
date: 2016-09-25
weather: cloudy
categories: Java
tags: [Java]
description:
---

> Chapeter 5 Generics

> in the release 1.5. generics were added to Java. you tell the compiler what types of objects are permitted in each collection. The compiler inserts casts for you automatically. 

> programs will be (1) safer & (2) cleaner, BUT the benefits come with complications. how to maximize the
benefits and minimize the complications?

# Item 23 Don’t use raw types in new code

- ### 1.Generic types: Generic classes and interfaces
	- [Definition] A class or interface whose declaration has one or more type parameters is a generic class or interface.
	- [Examples] As of release 1.5,	List interface has a single type parameter, E. Technically the interface should be called List<E> (read "list of E"). But we call it List for short.
	- [Parameterized types] Each generic type defines **a set of** parameterized types
		- Parameterized types consist of the class or interface name followed by an **angle-bracketed** list of actual type parame- ters corresponding to the generic type’s formal type parameters
		- [Example] List<String> is a parameterized type (corresponding to the formal type parameter E)
	- [Raw type] each generic type defines **a** raw type
		- [Purpose] to provide compatibility.
		- [Example] the raw type corresponding to List<E> is List
	
---

- ### 2.Discover errors
	- it pays to discover errors as soon as possible after they are made, ideally **at compile time**
	- [a case], you don’t discover the error till runtime (ClassCastException)
	- [Similiarly] put java.util.Date instance into java.sql.Date Collection.

				==================runtime error=========================
				// Don't do this: using raw type
				private final Collection stamps = ... ;
				stamps.add(new Coin( ... ));
				for (Iterator i = stamps.iterator(); i.hasNext(); ) {
					Stamp s = (Stamp) i.next(); // Throws ClassCastException
					... // Do something with the stamp
				}
				-----------------improve: typesafe----------------------
				private final Collection<Stamp> stamps = ... ;
				// typesafe
				for (Stamp s : stamps) { // No cast
					... // Do something with the stamp
				}
				// or a traditional for loop
				for (Iterator<Stamp> i = stamps.iterator(); i.hasNext(); ) {
					Stamp s = i.next(); // No cast necessary
					... // Do something with the stamp
				}

---

- ### 3.List<Object>
	- to allow insertion of arbitrary objects, using generic type like List<Object>

				===================Example========================
				// Uses raw type (List) - fails at runtime!
				public static void main(String[] args) {
				List<String> strings = new ArrayList<String>();
					unsafeAdd(strings, new Integer(42));
					String s = strings.get(0); // Compiler-generated cast
				}
				private static void unsafeAdd(List list, Object o) {
					list.add(o);
				}

				//Test.java:10: warning: unchecked call to add(E) in raw type List list.add(o);
				------------------Improvement---------------------
				private static void unsafeAdd(List<Object> list, Object o) {
					list.add(o);
				}
				// Test.java:5: unsafeAdd(List<Object>,Object) cannot be applied to (List<String>,Integer) unsafeAdd(strings, new Integer(42));

---

- ###4. unbounded wildcard types (?)
	- [Why using it] it is safe and the raw type isn't
		- [wildcard types] You can’t put any element (other than null ) into a Collection<?>
		- [raw types] You can put any element into a collection with a raw type, easily corrupting the collection’s type invariant
	- [When using it] use a generic type but you don’t know or care what the actual type parameter is
	- [Example] The unbounded wildcard type of Set<E> is Set<?> ("set of some type")

					===================wildcard==================
					// Unbounded wildcard type - typesafe and flexible
					static int numElementsInCommon(Set<?> s1, Set<?> s2) {
						int result = 0;
						for (Object o1 : s1)
							if (s2.contains(o1))
								result++;
						return result;
					}
					------------compile-time message------------
					WildCard.java:13: cannot find symbol
					symbol : method add(String)
					location: interface Collection<capture#825 of ?>
					c.add("verboten");

---

- ###2.Two minor exceptions
	- (1) You must use raw types in **class literals**.
		- [legal] List.class, String[].class, int.class, etc.
		- [illegal] List<String>.class List<?>.class
	- (2) instanceof operator
		- In this case, the angle brackets and question marks are noise

		======================(2) good example====================
		// Legitimate use of raw type - instanceof operator
		if (o instanceof Set) {			// Raw type
			Set<?> m = (Set<?>) o; 		// Wildcard type
			...
		}

---


---

- ### Conclusion
	- (1) using raw types can lead to exceptions at runtime, so don’t use them.
	- (2) Set<Object> is a parameterized type. It can contain objects of any type
	- (3) Set<?> is a wildcard type. It can contain only objects of some unknown type

---

- ### Quick Reference

Term|Example|Item
---|---|---
Parameterized | type List<String> | Item 23
Actual type | parameter String | Item 23
Generic type | List<E> | Items 23, 26
Formal type parameter | E | Item 23
Unbounded wildcard type | List<?> | Item 23
Raw type | List | Item 23
Bounded type parameter | <E extends Number> | Item 26
Recursive type bound | <T extends Comparable<T>> | Item 27
Bounded wildcard type | List<? extends Number> | Item 28
Generic method static | <E> List<E> asList(E[] a) | Item 27
Type token | String.class | Item 29