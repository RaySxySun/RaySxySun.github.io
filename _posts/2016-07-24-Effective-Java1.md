---
layout: post
title: Effective Java - Item 1 static factory
date: 2016-07-24
weather: rainy
categories: Java
tags: [Java]
description:
---

# Item 1 :  Consider static factory methods instead of constructors

> A class can provide a public *static factory method* (it is not the same as the Factory Method pattern from Design Patterns)

- Providing a static factory method instead of a public constructor has both advantages and disadvantages.
	- Advantages
		- [1] A well-chosen name is easier to use and to read
		- [2] [instance-controlled] They are not required to create a new object each time they're invoked.
			- [How?] How to control over instances
				- to user preconstructed instances
				- to cache instances as they're constructed
				- dispense to avoid creating unnecessary duplicate objects
				- [Similarly] Flyweight pattern [Improve] Improve performance if they are expensive to create
			- [Why] Reasons to write instance-controlled classes
				- [singleton/noninstantiable] Instance control allows a class to guarantee that it is a singleton or noninstantiable
				- [No 2 equal instances] it allows an immutable class (Item 15) to make the guarantee that no two equal instances exist: a.equals(b) if and only if a==b .
					- [Improve] If a class makes this guarantee, then its clients can use **the \=\= operator** instead of the equals(Object) method
		- [3] Can return an object of any subtype of their return type.
			- [nonpublic] interface-based frameworks
				- (compact: java.util.Collections) The Java Collections Framework has thirty-two convenience implementations of its collection interfaces, The classes of the returned objects are all nonpublic.
			- [add/remove] add/remove subclass with no ill effects (e.g. java.util.EnumSet RegularEnumSet/JumboEnumSet)
			- [decoupling] decoupling them from the implementations.(e.g.JDBC)
				- service provider frameworks
					- 1.service interface; 2.provider registration API; 3.service access API 4.service provider interface(optional)
					- In the case of JDBC, **Connection** plays the part of the **service interface**, **DriverManager.registerDriver** is the **provider registration API**, **DriverManager.getConnection** is the **service access API**, and **Driver** is the **service provider interface**. 


								// Service provider framework sketch
								// Service interface

								public interface Service {
									... // Service-specific methods go here
								}

								// Service provider interface

								public interface Provider {
									Service newService();
								}

								// Noninstantiable class for service registration and access
								public class Services {
									private Services() { } // Prevents instantiation
									
									// Maps service names to services
									private static final Map<String, Provider> providers = new ConcurrentHashMap<String, Provider>();
									public static final String DEFAULT_PROVIDER_NAME = "<def>";

									// Provider registration API
									public static void registerDefaultProvider(Provider p) {
										registerProvider(DEFAULT_PROVIDER_NAME, p);
									}

									public static void registerProvider(String name, Provider p){
										providers.put(name, p);	
									}

									// Service access API
									public static Service newInstance() {
										return newInstance(DEFAULT_PROVIDER_NAME);
									}

									public static Service newInstance(String name) {
										Provider p = providers.get(name);
										if (p == null)
											throw new IllegalArgumentException("No provider registered with name: " + name);
										return p.newService();
									}
								}

		- [4] Can return an object of any subtype of their return type.
			- reduce the verbosity of creating parameterized type instances(e.g. type inference)
				- Example:

							public static <K, V> HashMap<K, V> newInstance() {
								return new HashMap<K, V>();
							}

				- Alternative: (as of release 1.6, it does not.)

							public static <K, V> HashMap<K, V> newInstance() {
								return new HashMap<K, V>();
							}
							
							Map<String, List<String>> m = HashMap.newInstance();

	- Disadvantage
		- [1]classes without public or protected constructors cannot be subclassed.
			- it encourages programmers to use composition instead of inheritance
		- [2]they are not readily distinguishable from other static methods.
			- The Javadoc tool may someday draw attention to static factory methods. 
				- draw attention to static factories in class or interface comments.
				- by adhering to common naming conventions.


> In summary, static factory methods and public constructors both have their uses, and it pays to understand their relative merits. Often static factories are preferable, so avoid the reflex to provide public constructors without first considering static factories.