---
layout: post
title: Effective Java - Item 11 Override clone judiciously
date: 2016-08-18
weather: cloudy
categories: Java
tags: [Java]
description:
---

# Item 11: Override clone judiciously

> The Cloneable interface is intended to permit cloning. Unfortunately, it fails to serve this purpose. Its primary flaw is that it lacks a clone method.

> Cloneable fails to function as an interface because it lacks a public clone method.

- ### 1.The disadvantage of the Cloneable interface:
    - lacks a clone method && Object ’s clone method is protected.
        - (Without resorting to reflection) CANNOT invoke the clone method on an object merely because it implements Cloneable.
        - (Even with reflective) there is no guarantee that the object has an accessible clone method.

---

- ### 2.What does Cloneable do? (given that it contains no methods?)
    - if a class implements Cloneable , Object ’s clone method returns **a field-by-field copy** of the object;
    - otherwise, it throws **CloneNotSupportedException**.

> The Cloneable interface is a highly atypical use of interfaces

---

- ### 3.The precise meaning of "copy"(The general contract)
    - x.clone() !/= x
    - [Too weak] x.clone().getClass() /=/= x.getClass()
        - they extend a class and invoke super.clone from the subclass, the returned object will be an instance of the subclass.
        - [like automatic constructor chaining]if you override the clone method in a nonfinal class, you should return an object obtained by invoking super.clone .
    - x.clone().equals(x)
    - [Too strong: Normally, no constructors are called] A well-behaved clone method can call constructors, If the class is final, clone can even return an object created by a constructor.

---

- ### 4.In practice
    - A class that implements Cloneable is expected to provide a properly functioning **public** clone method.
        - [Method 1] superclasses provide well-behaved clone methods
            - covariant return types: Object => PhoneNumber(as of release 1.5)
            - an overriding method’s return type to be a subclass of the overridden method’s return type.

                        // Suppose you want to implement Cloneable in a class whose superclasses provide well-behaved clone methods.
                        // If every field contains a primitive value or a reference to an immutable object, the returned object may be exactly what you need
                        // provide public access to Object ’s protected clone method:
                        @Override public PhoneNumber clone() {
                            try {
                                return (PhoneNumber) super.clone();
                            } catch(CloneNotSupportedException e) {
                                throw new AssertionError(); // Can't happen
                            }
                        }

                        ----------------------------------------------------------------------
                        // If an object contains fields that refer to mutable objects, using the simple
                        // clone implementation shown above can be disastrous.

                        public class Stack {
                            private Object[] elements;
                            private int size = 0;
                            private static final int DEFAULT_INITIAL_CAPACITY = 16;

                            public Stack() {
                                this.elements = new Object[DEFAULT_INITIAL_CAPACITY];
                            }

                            public void push(Object e) {
                                ensureCapacity();
                                elements[size++] = e;
                            }

                            public Object pop() {
                                if (size == 0)
                                    throw new EmptyStackException();
                                Object result = elements[--size];
                                elements[size] = null; // Eliminate obsolete reference
                                return result;
                            }

                            // Ensure space for at least one more element.
                            private void ensureCapacity() {
                                if (elements.length == size)
                                    elements = Arrays.copyOf(elements, 2 * size + 1);
                                }
                        }

                        // Merely returns super.clone()
                        // (1)size field [correct]
                        // (2)elements field [wrong: it refers to the same array]
                        // could result in NullPointerException

        - [Method 2] clone recursively
            - In effect, the clone method functions as another constructor;
            - you must ensure no harm to establish invariants on the clone.
            - [NOTICE]the clone architecture is incompatible with normal use of final fields referring to mutable objects
                - In order to make a class cloneable, it may be necessary to remove final modifiers from some fields.

                        @Override public Stack clone() {
                            try {
                                Stack result = (Stack) super.clone();
                                result.elements = elements.clone();  //As of release 1.5
                                return result;
                            } catch (CloneNotSupportedException e) {
                                throw new AssertionError();
                            }
                        }

                    //NOT WORK if the elements field were final

            - [Problem] clone recursively.
                - [For Example]a hash table whose internals consist of an array of buckets, each of which references the first entry in a linked list of key-value pairs or is null if the bucket is empty.

                            // works fine if the buckets aren’t too long
                            // it consumes one stack frame for each element in the list
                            // If the list is long, this could easily cause a stack overflow.
                            public class HashTable implements Cloneable {
                                private Entry[] buckets = ...;
                                private static class Entry {
                                    final Object key;
                                    Object value;
                                    Entry next;

                                    Entry(Object key, Object value, Entry next) {
                                        this.key = key;
                                        this.value = value;
                                        this.next = next;
                                    }

                                    // Recursively copy the linked list headed by this Entry
                                    Entry deepCopy() {
                                        return new Entry(key, value, next == null ? null : next.deepCopy());
                                    }
                                }

                                @Override public HashTable clone() {
                                    try {
                                        HashTable result = (HashTable) super.clone();
                                        result.buckets = new Entry[buckets.length];
                                        for (int i = 0; i < buckets.length; i++)
                                            if (buckets[i] != null)
                                                result.buckets[i] = buckets[i].deepCopy();
                                        return result;
                                    } catch (CloneNotSupportedException e) {
                                        throw new AssertionError();
                                    }
                                }
                                ... // Remainder omitted
                            }

                            ---------------------------------------------------------------------------------
                            //replace the recursion in deepCopy with iteration
                            // Iteratively copy the linked list headed by this Entry
                            Entry deepCopy() {
                                Entry result = new Entry(key, value, next);
                                for (Entry p = result; p.next != null; p = p.next)
                                    p.next = new Entry(p.next.key, p.next.value, p.next.next);
                                return result;
                            }

        - [Method 3] "helper method"
            - it should be either final or private
            - In the case of our HashTable example, a put(key, value) could be used to initialize the buckets field.
            - [Problem]clone may invoke an overridden method  => lead to corruption

---

- ### 5.CloneNotSupportedException
    - If a class that is designed for **inheritance** (Item 17) overrides clone ,
      - it should be declared protected ,
      - it should be declared to throw CloneNotSupportedException
      - the class should not implement Cloneable .

---

- ### 6.A thread-safe class implement Cloneable
    - its clone method must be properly **synchronized** just like any other method (Item 66).

---

- ### 7.Options
    - All classes that implement Cloneable should override clone with a **public method** whose return **type is the class itself**
        - first **call super.clone**
        - **fix any fields that need to be fixed
        - [Exception] unique ID / creation time
    - A copy constructor or copy factory
        - A copy constructor: public Yum(Yum yum);
        - A copy factory: public static Yum newInstance(Yum yum);
        - [Advantages]:
            - don’t rely on a risk-prone object creation mechanism
            - don’t demand unenforceable adherence to conventions
            - don’t conflict with the proper use of final fields
            - don’t throw unnecessary checked exceptions
            - don’t require casts
            - conversion constructors & conversion factories
                - Interface-based copy constructors and factories allow the client to choose the implementation type of the copy rather than forcing the client to accept the implementation type of the original.
                - You want to copy HashSet s as a TreeSet [clone method X] [conversion constructor V]
        - [Disadvantages]:
            - impossible to put a copy constructor or factory in an interface
    - [NO Implementation] it doesn’t make sense for immutable classes to support object copying


> Given all of the problems associated with Cloneable , it’s safe to say that other interfaces should not extend it, and that classes designed for **inheritance** (Item 17) should not implement it.

---

- ### Conclusion:
    - (1) Never to override the clone method and never to invoke it (Exception: copy arrays)
    - (2) Design a class for inheritance. If not to provide a well-behaved protected clone method, it will be impossible for subclasses to implement Cloneable .