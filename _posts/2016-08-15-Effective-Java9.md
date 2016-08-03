---
layout: post
title: Effective Java - Item 9  Override hashCode when overriding equals
date: 2016-08-15
weather: cloudy
categories: Java
tags: [Java]
description:
---

# Item 9: Always override hashCode when you override equals

> Failure to override hashCode in every class that overrides equals will prevent classes from functioning properly in conjunction with all hash-based collections. (HashMap , HashSet , and Hashtable)

- ### 1.the general contract for Object.hashCode: (3 provisions)
    - (1)the hashCode method must consistently return the same integer (invoked more than once)
    - (2)Equal objects must have equal hash codes.
    - (3)[Not required] unequal objects must have unequal hash codes.

> The key provision that is violated when you fail to override hashCode is the second one: equal objects must have equal hash codes.

                        public final class PhoneNumber {
                            private final short areaCode;
                            private final short prefix;
                            private final short lineNumber;

                            public PhoneNumber(int areaCode, int prefix, int lineNumber) {
                                rangeCheck(areaCode, 999, "area code");
                                rangeCheck(prefix, 999, "prefix");
                                rangeCheck(lineNumber, 9999, "line number");
                                this.areaCode = (short) areaCode;
                                this.prefix = (short) prefix;
                                this.lineNumber = (short) lineNumber;
                            }

                            private static void rangeCheck(int arg, int max, String name) {
                                if (arg < 0 || arg > max)
                                    throw new IllegalArgumentException(name +": " + arg);
                            }

                            @Override public boolean equals(Object o) {
                                if (o == this)
                                    return true;
                                if (!(o instanceof PhoneNumber))
                                    return false;
                                PhoneNumber pn = (PhoneNumber)o;

                                return pn.lineNumber == lineNumber
                                && pn.prefix == prefix
                                && pn.areaCode == areaCode;
                            }
                            // Broken - no hashCode method!
                            ... // Remainder omitted
                        }

                        ---------------------------------------------------------------------

                        Map<PhoneNumber, String> m = new HashMap<PhoneNumber, String>();
                        m.put(new PhoneNumber(707, 867, 5309), "Jenny");

                        // [Problem] you might expect m.get(new PhoneNumber(707 , 867 , 5309)) to return "Jenny" , but it returns null .
                        // The failure to override hashCode causes the two equal instances to have unequal hash codes
                        // get method looks for the phone number in a different hash bucket (stored by the put method)
                        // Even if the two instances happen to hash to the same bucket， get method could return null, because an optimization of Hashmap
                        // Hashmap's optimization: caches the has code associated with each entry and doesn't bother checking for object equality if the hash codes don't match.

                        ----------------------------------------------------------------------
                        // The worst possible legal hash function - never use!
                        // hash tables degenerate to linked lists
                        // run in linear time instead run in quadratic time

                        @Override public int hashCode() { return 42; }

                        ----------------------------------------------------------------------
                        // A good hash function tends to produce unequal hash codes for unequal objects.
                        // This is exactly what is meant by the third provision of the hashCode contract.

---

- ### 2. A fair approximation(a simple recipe)
    - (1) Store some constant nonzero value say, (int result = 17).
    - (2) For each significant field f
        - a. Compute an int hash code c for the field f

        field|hash code
        ---|---
        boolean|(f ? 1 : 0)
        byte , char , short , or int | (int) f
        float| Float.floatToIntBits(f)
        double|Double.doubleToLongBits(f) && (int) (f ^ (f \>\>\> 32))
        object reference|ecursively invoking equals and hashCode
        array|treat items as separate fields

        - b. Combine the hash code c computed in step 2.a into result as follows:
            - result = 31 * result + c;
            - better performance: 31 * i == (i << 5) - i . Modern VMs do this sort of optimization automatically.
    - (3) Return result
    - (4) Ask yourself whether equal instances have equal hash codes.

                        //It is simple, reasonably fast, and does a reasonable job of dispersing unequal phone numbers into different hash buckets
                        @Override public int hashCode() {
                            int result = 17;
                            result = 31 * result + areaCode;
                            result = 31 * result + prefix;
                            result = 31 * result + lineNumber;
                            return result;
                        }

    - immutable class:
        - calculate the hash code when the instance is created: most objects of this type will be used as hash keys
        - choose to lazily initialize it the first time hashCode is invoked (Item 71)

                        // Lazily initialized, cached hashCode
                        private volatile int hashCode; //
                        @Override public int hashCode() {
                            int result = hashCode;
                            if (result == 0) {
                                result = 17;
                                result = 31 * result + areaCode;
                                result = 31 * result + prefix;
                                result = 31 * result + lineNumber;
                                hashCode = result;
                            }
                            return result;
                        }
---

- ### 3.Do & Do not
    - [Do] Exclude redundant fields
    - [Do not] be tempted to exclude significant parts of an object from the hash code computation to improve performance

---

- ### 4.Not Good
    - String , Integer , and Date , include in their specifications the exact value returned by their hashCode method as a function of the instance value
        - limits the ability to improve the hash function in future releases.

---
