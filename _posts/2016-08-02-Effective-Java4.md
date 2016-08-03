---
layout: post
title: Effective Java - Item 4 noninstantiability
date: 2016-08-02
weather: cloudy
categories: Java
tags: [Java]
description:
---

# Item 4: Enforce noninstantiability with a private constructor

> Occasionally you’ll want to write a class that is just a grouping of static methods and static fields. Such classes have acquired a bad reputation because some people abuse them to avoid thinking in terms of objects, but they do have valid uses.

- [purpose]They can be used to group related methods on primitive values or arrays, in the manner of java.lang.Math or java.util.Arrays .
    - to group static methods
    - implement a particular interface
    - to group methods on a final class
    - e.g. an utility classes were not designed to be instantiated

- [mechanism] explicitly or implicitly: In the absence of explicit constructors, however, the compiler provides a public, parameterless default constructor.

- [method] a class can be made noninstantiable by including a private constructor
    - Because the explicit constructor is private, it is inaccessible outside of the class.
    - this is counterintuitive, It is therefore wise to include a comment.
    - [side effect] this idiom also prevents the class from being subclassed.

            // Noninstantiable utility class
            public class UtilityClass {
                // Suppress default constructor for noninstantiability
                private UtilityClass() {
                    throw new AssertionError();
                }
                ... // Remainder omitted
            }