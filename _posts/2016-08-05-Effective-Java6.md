---
layout: post
title: Effective Java - Item 6 Eliminate obsolete object references
date: 2016-08-05
weather: cloudy
categories: Java
tags: [Java]
description:
---

# Eliminate obsolete object references

> manual memory management Language (C/C++) VS. garbage-collected language (Java)

- Java will automatically reclaim objects.
    - It seems good that you don’t have to think about memory management. But this isn’t quite true.
    - Memory leaks in garbage-collected languages (more properly known as unintentional object retentions) are insidious.
        - e.g. If a stack grows and then shrinks, the objects that were popped off the stack will not be garbage collected, even if the program using the stack has no more references to them.

                    // Can you spot the "memory leak"? it can cause disk paging and even program failure
                    public class Stack {
                        private Object[] elements;
                        private int size = 0;
                        private static final int DEFAULT_INITIAL_CAPACITY = 16;
                        public Stack() {
                            elements = new Object[DEFAULT_INITIAL_CAPACITY];
                        }
                        public void push(Object e) {
                            ensureCapacity();
                            elements[size++] = e;
                        }
                        public Object pop() {
                            if (size == 0)
                                throw new EmptyStackException();
                            return elements[--size];
                        }

                        /**
                        * Ensure space for at least one more element, roughly
                        * doubling the capacity each time the array needs to grow.
                        */

                        private void ensureCapacity() {
                            if (elements.length == size)
                                elements = Arrays.copyOf(elements, 2 * size + 1);
                        }
                    }

                    ---------------------------Fix---------------------------------
                    // null out references once they become obsolete.
                    // Nulling out object references should be the exception rather than the norm.

                    public Object pop() {
                        if (size == 0)
                                throw new EmptyStackException();
                            Object result = elements[--size];
                            elements[size] = null; // Eliminate obsolete reference
                            return result;
                    }

- Source of memory leak
    - (1) Whenever a class manages its own memory, the programmer should be alert for memory leaks.
    - (2) Another common source of memory leaks is caches. (WeakHashMap cache)
        - [solution 1]so long as there are references to its key outside of the cache, represent the cache as a WeakHashMap ; entries will be removed automatically after they become obsolete.
        - [solution 2]background thread(a Timer or ScheduledThreadPoolExecutor)
            - The LinkedHashMap class facilitates the latter approach with its removeEldestEntry method
            - For more sophisticated caches, you may need to use java.lang.ref directly.
    - (3) A third common source of memory leaks is listeners and other callbacks.
        - where clients register callbacks but don’t deregister them explicitly
            - store only weak references to them (by storing them only as keys in a WeakHashMap)


> a debugging tool heap profiler
maintaining your own object pools **clutters your code, increases memory footprint, and harms performance**. Modern JVM implementations have highly optimized garbage collectors that easily outperform such object pools on lightweight objects.