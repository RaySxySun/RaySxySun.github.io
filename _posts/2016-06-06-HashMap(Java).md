---
layout: post
title: HashMap(Java)
date: 2016-06-06
weather: cloudy
categories: Java
tags: [Java]
description:
---

# HashMap

- Internal structure
	- (1) The interface Map<K,V>: The Java HashMap class implements it main methods:
		- V put(K key, V value)
		- V get(Object key)
		- V remove(Object key)
		- Boolean containsKey(Object key)
	- (2) the Entry<K, V>: is used to keep data.
		- key-value pair
		- two extra data:
			- a reference to another Entry(like linked list)
			- a hash value that represents the hash value of the key ( avoid the computation of the hash every time)

		static class Entry<K,V> implements Map.Entry<K,V> {
		        final K key;
		        V value;
		        Entry<K,V> next;
		        int hash;
		…
		}
-
	- (3) buckets or bins: multiple singly linked lists of **nullable** entries ; **default capacity is 16**
		- Three Steps:
			- gets the hashcode of the key
			- rehashes the hashcode to prevent against a bad hashing function
			- rehashed hash hashcode and bit-masks it(optimized modulo function)


			// the "rehash" function in JAVA 7 that takes the hashcode of the key
			static int hash(int h) {
			    h ^= (h >>> 20) ^ (h >>> 12);
			    return h ^ (h >>> 7) ^ (h >>> 4);
			}
			// the "rehash" function in JAVA 8 that directly takes the key
			static final int hash(Object key) {
			    int h;
			    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
			    }
			// the function that returns the index from the rehashed hash
			static int indexFor(int h, int length) {
			    return h & (length-1);
			}

<img src="{{ site.url }}/assets/img/2016-06-06-HashMap/HashMap1.png" alt="{{ page.title }} at {{ site.title }}">

> the size of the inner array needs to be a power of 2. For Example
> size is 17: bitwise formula “H AND 16” is going to be either 16 or 0.
> size is 16: the range of the bitwise index formula “H AND 15” is from 0 to 15

# HashMap Auto Resizing

> the HashMap is able to increase its inner array in order to keep very short nullable linked lists.
> public HashMap(int initialCapacity, float loadFactor): the default initialCapacity is **16** && default loadFactor is **0.75**.

- if it needs to increase the capacity of the inner array?:
	- Each time we add a new key/value in your Map with put(…)
	- The size of the map & A threshold
	- the resizing of the array creates twice more buckets
	- redistributes all the existing entries into the buckets (the old ones and the newly created).
	- the HashMap only increase the size of the inner array, it wouldn't **decrease** it.

# Thread Safety

- (1)The **HashMap** is not threads safe. (Because of auto-resizing mechanism)
- (2)The **HashTable** is a thread safe. (SLOW: CRUD methods are synchronized)
- (3)The **ConcurrentHashMap** is smater thread safe implementation. (JAVA5:synchronized bucket)
	- multiples threads can get(), remove() or put(),  if not accessing the same bucket or resizing
	- It is better choice for multithreaded application.

> [Worst] when 2 threads save/put value & the 2 put() calls auto resizing at the same time, which would create a inner-loop in a bucket. Trying to get a data, the get(), will never end.

# Unchangeable Key

> If we create your own Key class and don’t make it changeable. Modifying key may cause value "lost"(exist but cannot be found).

# HashMap in JAVA 8

>  The lines of code about HashMap increases from 1k (JAVA 7) to 2k (JAVA 8)

- A BIG difference: Nodes can be extended to TreeNodes.
	- TreeNodes	is a red-black tree structure.O(log(n)) for add, delete or get

		static class Node<K,V> implements Map.Entry<K,V> {
	     final int hash;
	     final K key;
	     V value;
	     Node<K,V> next;


> more exhaustive list of the TreeNodes. Red black trees are self-balancing binary search trees. their length is always in log(n). The inner table can contain both Node (linked list; if 6- nodes in a bucket) and TreeNode (red-black tree; if 8+ nodes in a bucket).

> **The  main advantage: in the same index (bucket), search in a tree will cost O(log(n)) VS. cost O(n) with a linked list in Java 7.**

> **The  main disadvantage: the tree takes really more space.**

		static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
		    final int hash; // inherited from Node<K,V>
		    final K key; // inherited from Node<K,V>
		    V value; // inherited from Node<K,V>
		    Node<K,V> next; // inherited from Node<K,V>
		    Entry<K,V> before, after;// inherited from LinkedHashMap.Entry<K,V>
		    TreeNode<K,V> parent;
		    TreeNode<K,V> left;
		    TreeNode<K,V> right;
		    TreeNode<K,V> prev;
		    boolean red;

> Red black trees are self-balancing binary search trees. their length is always in log(n)

<img src="{{ site.url }}/assets/img/2016-06-06-HashMap/HashMap2.png" alt="{{ page.title }} at {{ site.title }}">


# Memory
