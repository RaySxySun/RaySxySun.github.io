---
layout: post
title: Effective Java - Item 7 Avoid finalizers
date: 2016-08-06
weather: sunny
categories: Java
tags: [Java]
description:
---


# Avoid finalizers

> Finalizers are unpredictable, often dangerous, and generally unnecessary.

- ### 1.As a rule of thumb, you should avoid finalizers
    - do not to think of finalizers as Java’s analog of C++ destructors.
        - **[In C++]** destructors are the normal way to reclaim the resources associated with an object, a necessary counterpart to constructors. it can reclaim other nonmemory resources.
        - **[In Java]** the garbage collector reclaims the storage associated with an object when it becomes unreachable, requiring no special effort on the part of the programmer.

---

- ### 2.One shortcoming of finalizers
    - [1] there is no guarantee finalizers will be executed promptly
        - [arbitrarily long] the time that an object becomes unreachable and the time that its finalizer is executed
        - never do anything **time-critical** in a finalizer.
            - e.g., [grave error] depend on a finalizer to close files
            - If many files are left open because the JVM is tardy in executing finalizers, a program may fail because it can no longer open files.
        - [no guarantees] no guarantee that finalizers will get executed promptly; no guarantee that they’ll get executed at all
        - [low priority] finalizers thread may run at a lower priority
            - e.g., an application may have thousands of graphics objects on its finalizer queue just waiting to be finalized and reclaimed. Finalizers thread may run at a lower priority
    - [2] no warning.
        - if an uncaught exception is thrown during finalization, the exception is ignored
        - Normally, an uncaught exception will terminate the thread and print a stack trace, but not if it occurs in a finalizer—it won’t even print a warning.
    - [3] performance penalty
        - 430 times slower
    - [Conclusion] never depend on a finalizer to update critical persistent state.
        - e.g., depending on a finalizer to release a persistent lock on a shared resource such as a database is a good way to bring your entire distributed system to a grinding halt.

---

- ### 3.Don't
    - Don't use System.gc, System.runFinalization (may increase the odds of finalizers getting executed, but no guarantee)
    - Don't use System.runFinalizersOnExit and Runtime.runFinalizersOnExit (Deprecated; ThreadStop)

---

> So what should you do instead of writing a finalizer for a class whose objects encapsulate resources that require termination, such as files or threads?

- ### 4.What should you do?
    - Just provide an explicit termination method
        - [details] every instance must keep track of whether it has been terminated:
            - the explicit termination method must record in a private field that the object is no longer valid, and other methods must check this field and throw an **IllegalStateException** if they are called after the object has been terminated.
    - Explicit termination methods are typically used in combination with the try-finally construct to ensure termination.

                        // try-finally block guarantees execution of termination method
                        Foo foo = new Foo(...);
                        try {
                            // Do what must be done with foo
                            ...
                        } finally {
                            foo.terminate(); // Explicit termination method
                        }

    - Typical examples
        - The **close methods** on InputStream , OutputStream , and java.sql.Connection .
        - The **cancel method** on java.util.Timer (cause the thread associated with a Timer instance to terminate itself gently)
        - Graphics.dispose and Window.dispose on java.awt. (Image.flush: deallocates all the resources but the instance can still be used)

---

- ### 5.what, if anything, are finalizers good for?(Two)
    - [1]"safety net" finalizer
        - in case the owner of an object forgets to call its explicit termination method.
        - it may be better to free the resource late than never.
        - the finalizer should log a warning if it finds that the resource has not been terminated (indicates a bug in the client code)
        - FileInputStream , FileOutputStream , Timer , and Connection have safe nets but do not log warnings, in case of break existing clients.
        - [?] whether the extra protection is worth the extra cost ?
    - [2] native peers
        - A native peer is a native object to which a normal object delegates via native methods.
        - A native peer is not a normal object; GC doesn't know it and can't reclaim it.
        - The native should have an explicit termination method (can be a native method)
        - A finalizer is an appropriate vehicle for performing this task

---

- ### 6.finalizer chaining
    - If a class (other than Object ) has a finalizer and a subclass overrides it
    - the subclass finalizer must invoke the superclass finalizer **manually**.
    - **using finally block** to ensures that the superclass finalizer gets executed

                    // Manual finalizer chaining
                        @Override protected void finalize() throws Throwable {
                        try {
                            ... // Finalize subclass state
                        } finally {
                            super.finalize();
                        }
                    }

---

- ### 7. using anonymous class (Item 22) to defend against careless or malicious subclasses
    - A single instance of the anonymous class, is created for each instance of the **enclosing class**.
    - it doesn’t matter whether a subclass finalizer calls super.finalize or not.
    - The technique should be considered for every nonfinal public class that has a finalizer.

            // Finalizer Guardian idiom
            public class Foo {
                // Sole purpose of this object is to finalize outer Foo object
                private final Object finalizerGuardian = new Object() {
                @Override protected void finalize() throws Throwable {
                    ... // Finalize outer Foo object
                }
                };
                ... // Remainder omitted
            }

---

- ### Conclusion:
    - don’t use finalizers except as a safety net(remember to log the invalid usage) or to terminate noncritical native resources.
    - for rare instances where you do use a finalizer, remember to invoke **super.finalize**
    - consider using a finalizer guardian (if you need to associate a finalizer with a public, nonfinal class)