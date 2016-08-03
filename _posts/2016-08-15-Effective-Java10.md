---
layout: post
title: Effective Java - Item 10 Always override toString
date: 2016-08-15
weather: cloudy
categories: Java
tags: [Java]
description:
---

# Item 10: Always override toString

> java.lang.Object provides an implementation of the toString method, but it returns is generally not what the user of your class wants to see.

> the returns will be followed by an ( @ ) and the unsigned hexadecimal representation of the hash code. like PhoneNumber@163b91

---

- ### We like a concise but informative representation.

> It is recommended that all subclasses override this method
    - automatically invoked
        - when an object is passed to println , printf , the string concatenation operator, or assert , or printed by a debugger.
    - clearly document
        - Whether or not you decide to specify the format, you should clearly document your intentions.
    - programmatic access
        - provide programmatic access to all of the information contained in the value returned by toString.

---


