---
layout: post
title: Effective Java - Item 5 Unnecessary Objects
date: 2016-08-03
weather: cloudy
categories: Java
tags: [Java]
description:
---

# Item 5: Avoid creating unnecessary objects

> Item 5 says, "Don’t create a new object when you should reuse an existing one.", while Item 39 says, “Don’t reuse an existing object when you should create a new one.”

> Failing to make defensive copies(Item 39) where required can lead to **insidious bugs and security holes**; creating objects unnecessarily(Item 5) merely affects **style and performance**.

> It is often appropriate to reuse a single object instead of creating a new functionally equivalent object each time it is needed.

- ### An Example:
    - in a loop

            String s = new String("stringette");// DON'T DO THIS!

    - instead:

            String s = "stringette";

> it is guaranteed that the object will be reused by any other code running in the same virtual machine that happens to contain the same string literal [JLS, 3.10.5].

---

- ### 1.[FOR immutable classes]using static factory methods (Item 1) in preference to constructors
    - e.g. the static factory method Boolean.valueOf(String) **is almost always preferable to** the constructor Boolean(String) .
    - The constructor creates a new object each time it’s called, while the static factory method is never required to do so and won’t in practice.

---

- ### 2.[FOR mutable objects] reuse mutable objects if you know they won’t be modified
    - e.g. The isBabyBoomer method unnecessarily creates a new Calendar , TimeZone , and two Date instances each time it is invoked.

                public class Person {
                    private final Date birthDate;
                    // Other fields, methods, and constructor omitted
                    // DON'T DO THIS!
                    public boolean isBabyBoomer() {
                    // Unnecessary allocation of expensive object
                        Calendar gmtCal = Calendar.getInstance(TimeZone.getTimeZone("GMT"));
                        gmtCal.set(1946, Calendar.JANUARY, 1, 0, 0, 0);
                        Date boomStart = gmtCal.getTime();
                        gmtCal.set(1965, Calendar.JANUARY, 1, 0, 0, 0);
                        Date boomEnd = gmtCal.getTime();

                        return birthDate.compareTo(boomStart) >= 0 && birthDate.compareTo(boomEnd) < 0;
                    }
                }

    - avoids this inefficiency with  (32,000 ms for 10 million invocations) VS. (130 ms)
        - Calendar instances are particularly expensive to create
        - eliminate the unnecessary initializations by **lazily initializing** these fields (Item 71) the first time the isBabyBoomer method is invoked, but it is not recommended.
            - complicate the implementation
            - unlikely to result in a noticeable performance improvement

                        class Person {
                            private final Date birthDate;

                            // Other fields, methods, and constructor omitted

                            /**
                            * The starting and ending dates of the baby boom.
                            */

                            private static final Date BOOM_START;
                            private static final Date BOOM_END;

                            static {
                                Calendar gmtCal = Calendar.getInstance(TimeZone.getTimeZone("GMT"));
                                gmtCal.set(1946, Calendar.JANUARY, 1, 0, 0, 0);
                                BOOM_START = gmtCal.getTime();
                                gmtCal.set(1965, Calendar.JANUARY, 1, 0, 0, 0);
                                BOOM_END = gmtCal.getTime();
                            }

                            public boolean isBabyBoomer() {
                                return birthDate.compareTo(BOOM_START) >= 0 && birthDate.compareTo(BOOM_END) < 0;
                            }
                        }

---

- ### 3.Consider the case of adapters, also known as views.
    - Because an adapter has no state beyond that of its backing object, there’s no need to create more than one instance of a given adapter to a given object
    - e.g. the **keySet method** of the Map interface returns a Set view of the Map object, consisting of all the keys in the map. it would seem that every call to keySet would have to create a new Set instance, but every call to keySet on a given Map object may return **the same Set instance**.

---

- ### 4. Prefer primitives to boxed primitives, and watch out for unintentional autoboxing (The lesson is clear)

                // The variable sum is declared as a Long instead of a long , which means that the program constructs about 2^31 unnecessary Long instances
                // Hideously slow program! Can you spot the object creation?
                public static void main(String[] args) {
                    Long sum = 0L;
                    for (long i = 0; i < Integer.MAX_VALUE; i++) {
                        sum += i;
                    }
                    System.out.println(sum);
                }

> The creation and reclamation of small objects whose constructors do little explicit work is cheap

> Avoiding object creation by maintaining your own object pool is a bad idea unless the objects in the pool are extremely heavyweight. The classic example of an object that **does justify** an object pool is a **database connection**. database **license may limit** you **to** a fixed number of connections.

> Generally speaking, however, maintaining your own object pools **clutters your code, increases memory footprint, and harms performance**. Modern JVM implementations have highly optimized garbage collectors that easily outperform such object pools on lightweight objects.