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
    - [-] a client  can create a instance using private Constructor by Reflection
        - To resolve this problem
            - 1.declare all instance fields transient
            - 2.provide a readResolve method

                        // readResolve method to preserve singleton property
                        private Object readResolve() {
                            // Return the one true Elvis and let the garbage collector
                            // take care of the Elvis impersonator.
                            return INSTANCE;
                        }

- [The Best]As of release 1.5, make an enum type with one element to implement singletons (even in the face of sophisticated serialization or reflection attacks.)
    - [+] more concise
    - [+] provides the serialization machinery for free
    - [+] provides an ironclad guarantee against multiple instantiation,
    - [-] **Enums often require more than twice as much memory as static constants. You should strictly avoid using enums on Android.**

            // Example 1
            // Enum singleton - the preferred approach
            public enum Elvis {
                INSTANCE;
                public void leaveTheBuilding() { ... }
            }

            // Example 2
            public enum Singleton {
                INSTANCE;
                private String name;
                public String getName(){
                    return name;
                }
                public void setName(String name){
                    this.name = name;
                }
            }


# Examples:

> all these examples are need extra workload.

> 1. Serializable: (Serializable、transient、readResolve())
> 2. Reflection: create an instance using private Constructor.

---


                        // Example 1
                        //create instance when first loading. [-] memory cost when loading
                        public class Singleton {
                            private static Singleton = new Singleton();
                            private Singleton() {}
                            public static getSignleton(){
                                return singleton;
                            }
                        }

---

                        // Example 2
                        //[-] non-thread safe

                        public class Singleton {
                            private static Singleton singleton = null;
                            private Singleton(){}
                            public static Singleton getSingleton() {
                                if(singleton == null) singleton = new Singleton();
                                return singleton;
                            }
                        }

---

                    // Example 3
                    // [-]thread safe but not efficient
                    public class Singleton {
                        private static volatile Singleton singleton = null;

                        private Singleton(){}

                        public static Singleton getSingleton(){
                            synchronized (Singleton.class){
                                if(singleton == null){
                                    singleton = new Singleton();
                                }
                            }
                            return singleton;
                        }
                    }

---

                    /*
                    Example 4
                    Improvement: Double check
                    Problem: In JVM, creation and assignment are two independent operations. (instance = new Singleton();)
                    Example:
                        1. Thread A, B execute IF condition at the same time.
                        2. Thread A find instance is null. Then JVM will create a space in Hotspot JVM Heap, while the instace has not been initialized at the moment.
                        3. Thread B find there is a instance and it try to call the instance's method. (ERROR!!!)
                    */

                    public class Singleton {
                        private static volatile Singleton singleton = null;

                        private Singleton(){}

                        public static Singleton getSingleton(){
                            if(singleton == null){
                                synchronized (Singleton.class){
                                    if(singleton == null){
                                        singleton = new Singleton();
                                    }
                                }
                            }
                            return singleton;
                        }
                    }


---

                    /*
                    Example 5
                    INNER class, thread mutex (Thread-Safe)
                    Problem: 1. concern about (Serializable、transient、readResolve())
                             2. If Class Singleton's creator throws an exception. We will never get a instance.
                    Nothing is perfect!!!
                    */

                    public class Singleton {

                        private Singleton() {
                        }

                        /* inner class is responsible for managing Singleton instance */
                        private static class SingletonFactory {
                            private static Singleton instance = new Singleton();
                        }

                        /* get instance */
                        public static Singleton getInstance() {
                            return SingletonFactory.instance;
                        }

                        public Object readResolve() {
                            return getInstance();
                        }
                    }

---

                    // Example 6: Two method
                    public class SingletonSample6 {

                            private static SingletonSample6 instance = null;

                            private SingletonSample6() {
                            }

                            private static synchronized void syncInit() {
                                if (instance == null) {
                                    instance = new SingletonSample3();
                                }
                            }

                        public static SingletonSample6 getInstance() {
                            if (instance == null) {
                                syncInit();
                            }
                            return instance;
                        }
                    }