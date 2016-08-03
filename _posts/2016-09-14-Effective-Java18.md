---
layout: post
title: Effective Java - Item 18 Prefer interfaces to abstract classes
date: 2016-09-14
weather: sunny
categories: Java
tags: [Java]
description:
---

> Chapeter 4 Classes and Interfaces


# Item 18: Prefer interfaces to abstract classes

- 1.Existing classes can be easily retrofitted to implement a new interface.
- 2.Interfaces are ideal for defining mixins.
- 3.Interfaces allow the construction of nonhierarchical type frameworks.
- 4.Interfaces enable safe, powerful functionality enhancements
- 5.Abstract[Interface]: abstract skeletal implementation
	- where Interface is the name of the interface they implement
	- [e.g] bstractCollection , AbstractSet , AbstractList , and AbstractMap .
	- You can combine the virtues of interfaces and abstract classes by providing an abstract skeletal implementation class to go with each nontrivial interface that you export.
	- [Why?] why using abstract classes to define types
		- **(1) It is far easier to evolve an abstract class than an interface. **
			- e.g. in a subsequent release, you want to add a new method to an abstract class, you can always add a concrete method containing a reasonable default implementation All existing implementations of the abstract class will then provide the new method. This does not work for interfaces. Classes implemented the interface will miss the new method and won’t compile anymore
		- **(2) Once an interface is released and widely implemented, it is almost impossible to change.**


					==========================5. Skeletal Implementation=================================
					// (1) it would have made sense to call them SkeletalCollection , SkeletalSet , SkeletalList , and SkeletalMap,but the Abstract convention is now firmly established 	
					// (2) Adapter: the example is an Adapter [Gamma95, p. 139] that allows an int array to be viewed as a list of Integer instances.
					// (3) inaccessible anonymous class: the class AbstractList is an inaccessible anonymous class

					// Concrete implementation built atop skeletal implementation
					static List<Integer> intArrayAsList(final int[] a) {
						if (a == null)
							throw new NullPointerException();
						
						// the class is an inaccessible anonymous class
						return new AbstractList<Integer>() {
							public Integer get(int i) {
								return a[i]; 	// Autoboxing (Item 5)
							}
							@Override public Integer set(int i, Integer val) {
								int oldVal = a[i];
								a[i] = val; 	// Auto-unboxing
								return oldVal; 	// Autoboxing
							}
							public int size() {
								return a.length;
							}
						};
					}
					---------------------------create a skeletal implementation-----------------------------
					// Skeletal Implementation
					public abstract class AbstractMapEntry<K,V>
					implements Map.Entry<K,V> {
						// Primitive operations
						public abstract K getKey();
						public abstract V getValue();
						// Entries in modifiable maps must override this method
						public V setValue(V value) {
							throw new UnsupportedOperationException();
						}
						// Implements the general contract of Map.Entry.equals
						@Override public boolean equals(Object o) {
							if (o == this)
								return true;
							if (! (o instanceof Map.Entry))
								return false;
							Map.Entry<?,?> arg = (Map.Entry) o;
							return 	equals(getKey(), arg.getKey()) &&
									equals(getValue(), arg.getValue());
						}
						private static boolean equals(Object o1, Object o2) {
							return o1 == null ? o2 == null : o1.equals(o2);
						}
						// Implements the general contract of Map.Entry.hashCode
						@Override public int hashCode() {
							return hashCode(getKey()) ^ hashCode(getValue());
						}
						private static int hashCode(Object obj) {
							return obj == null ? 0 : obj.hashCode();
						}
					}

---

- To summarize:
	- (1) **an interface** is generally the best way to define a type that permits multiple implementations.
	- (2) An exception: ease of evolution is deemed more important than flexibility and power.
		- Under these circumstances, you should use an abstract class to define the type
		- you should understand and can accept the limitations
	- (3) When exporting a nontrivial interface
		- providing a skeletal implementation to go with it.
	- (4) Finally, design public interfaces with the utmost care and test them thoroughly 
