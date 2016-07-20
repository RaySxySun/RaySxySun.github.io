---
layout: post
title: Effective Java - Item 3 Singleton Property
weather: rainy
categories: Java
tags: [Java]
description:
---

# Item 3: Enforce the singleton property with a private constructor or an enum type

> A singleton is simply a class that is instantiated exactly once

- Before 1.5: There were two ways to implement singletons.
    - 1.the member is a final field: (simpler)
        - [-] a privileged client can invoke the private constructor reflectively with the aid of the AccessibleObject.setAccessible method.
        - to defend against this attack, modify the constructor to make it throw an exception if it’s asked to create a second instance.

                // Singleton with public final field
                public class Elvis {
                    public static final Elvis INSTANCE = new Elvis();
                    private Elvis() { ... }
                    public void leaveTheBuilding() { ... }
                }

    - 2.the public member is a static factory method

                // Singleton with static factory
                public class Elvis {
                    private static final Elvis INSTANCE = new Elvis();
                    private Elvis() { ... }
                    public static Elvis getInstance() { return INSTANCE; }
                    public void leaveTheBuilding() { ... }
                }
    - [-] sb can create a instance using private Constructor by Reflection
        - To resolve this problem
            - 1.declare all instance fields transient
            - 2.provide a readResolve method

            // readResolve method to preserve singleton property
            private Object readResolve() {
                // Return the one true Elvis and let the garbage collector
                // take care of the Elvis impersonator.
                return INSTANCE;
            }

- As of release 1.5, there is a third approach to implementing singletons
    - [-] _Enums often require more than twice as much memory as static constants. You should strictly avoid using enums on Android._

            // Enum singleton - the preferred approach
            public enum Elvis {
                INSTANCE;
                public void leaveTheBuilding() { ... }
            }