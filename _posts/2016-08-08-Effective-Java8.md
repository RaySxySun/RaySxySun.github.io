---
layout: post
title: Effective Java - Item 8  Overriding Equals
date: 2016-08-08
weather: sunny
categories: Java
tags: [Java]
description:
---


> Methods Common to All Objects Item 8 ~ 12

# Item 8: Obey the general contract when overriding equals

> we should obey the equals and hashCode contracts (Item 8, Item 9)

>Overriding the equals method seems simple, but there are many ways to get it wrong, and consequences can be dire.

- Not to override the equals method in some circumstances
    - Each instance of the class is inherently unique. e.g, Thread
    - don’t care whether the class provides a “logical equality” test. e.g, java.util.Random
    - A superclass has already overridden equals , and the superclass behavior is appropriate for this class.
        - Set implementations from AbstractSet
        - List implementations from AbstractList
        - Map implementations from AbstractMap
    - The class is private or package-private, and you are certain that its equals method will never be invoked.

                @Override public boolean equals(Object o) {
                    throw new AssertionError(); // Method is never called
                }

---

- when is it appropriate to override Object.equals ? (value classes)
    - has a notion of logical equality
    - a superclass has not already overridden equals to implement the desired behavior.

---

- General contract for overriding equals method
    - Implements an **equivalence relations** (the equals contract)
        - 1.**Reflexive**: For any non-null reference value x , **x.equals(x) must return true** .
        - 2.**Symmetric**: For any non-null reference values x and y , x.equals(y) must return true if and only if y.equals(x) returns true .
        - 3.**Transitive**: For any non-null reference values x , y , z , if x.equals(y) returns true and y.equals(z) returns true , then x.equals(z) must return true .
        - 4.**Consistent**: For any non-null reference values x and y , **multiple invocations** of x.equals(y) consistently return true or consistently return false , provided no information used in equals comparisons on the objects is modified.
        - 5.**Non-nullity**: For any non-null reference value x , x.equals(null) must return false .

---

- violation:
    - Reflexivity
    - Symmetry

                // Broken - violates symmetry!
                public final class CaseInsensitiveString {
                    private final String s;
                    public CaseInsensitiveString(String s) {
                        if (s == null)
                            throw new NullPointerException();
                        this.s = s;
                    }

                    // Broken - violates symmetry!
                    @Override public boolean equals(Object o) {
                    if (o instanceof CaseInsensitiveString)
                        return s.equalsIgnoreCase(((CaseInsensitiveString) o).s);

                    if (o instanceof String) // One-way interoperability!
                        return s.equalsIgnoreCase((String) o);
                        return false;
                    }
                    ... // Remainder omitted
                }

                CaseInsensitiveString cis = new CaseInsensitiveString("Polish");
                String s = "polish";
                cis.equals(s) // true
                s.equals(cis) // false

                List<CaseInsensitiveString> list = new ArrayList<CaseInsensitiveString>();
                list.add(cis);
                // Once you’ve violated the equals contract, you simply don’t know how other objects will behave when confronted with your object.
                list.contains(s) //uncertain

                // ill-conceived attempt
                // remove the ill-conceived attempt to interoperate with String from the equals method.
                @Override public boolean equals(Object o) {
                    return o instanceof CaseInsensitiveString && ((CaseInsensitiveString) o).s.equalsIgnoreCase(s);
                }

    - **Transitivity: There is no way to extend an instantiable class and add a value component while preserving the equals contract**

    > unless you are willing to forgo the benefits of object-oriented abstraction

                    public class Point {
                        private final int x;
                        private final int y;
                        public Point(int x, int y) {
                            this.x = x;
                            this.y = y;
                        }
                        @Override public boolean equals(Object o) {
                            if (!(o instanceof Point))
                                return false;
                            Point p = (Point)o;
                            return p.x == x && p.y == y;
                        }
                        ...
                        // Remainder omitted
                    }

                    // extend Class Point
                    public class ColorPoint extends Point {
                        private final Color color;
                        public ColorPoint(int x, int y, Color color) {
                            super(x, y);
                            this.color = color;
                        }
                        ...
                        // Remainder omitted

                        // Broken - violates symmetry!
                        @Override public boolean equals(Object o) {
                            if (!(o instanceof ColorPoint))
                                return false;
                            return super.equals(o) && ((ColorPoint) o).color == color;
                        }

                    }

                    Point p = new Point(1, 2);
                    ColorPoint cp = new ColorPoint(1, 2, Color.RED);
                    p.equals(cp); //returns true
                    cp.equals(p); //returns false

                    --------------------------------------------------

                    // mixed comparisons
                    // try to fix the symmetry

                    // add to ColorPoint
                    // Broken - violates transitivity!
                    @Override public boolean equals(Object o) {
                        if (!(o instanceof Point))
                            return false;

                        // If o is a normal Point, do a color-blind comparison
                        if (!(o instanceof ColorPoint))
                            return o.equals(this);

                        // o is a ColorPoint; do a full comparison
                        return super.equals(o) && ((ColorPoint)o).color == color;
                    }

                    ColorPoint p1 = new ColorPoint(1, 2, Color.RED);
                    Point p2 = new Point(1, 2);
                    ColorPoint p3 = new ColorPoint(1, 2, Color.BLUE);
                    p1.equals(p2); // return true
                    p2.equals(p3); // return true
                    p1.equals(p3); // return false

                    ---------------------------------------------------
                    // By using a getClass, extend an instantiable class and add a value component while preserving the equals contract
                    // Broken - violates Liskov substitution principle
                    // Using getClass test in place of the instanceof test
                    @Override public boolean equals(Object o) {
                        if (o == null || o.getClass() != getClass())
                            return false;
                        Point p = (Point) o;
                        return p.x == x && p.y == y;
                    }

    - Consistency:
        - For example, java.net.URL ’s equals method relies on comparison of the IP addresses of the hosts associated with the URLs.
        - network access isn’t guaranteed to yield the same results over time.
        - [Consequence] violate the equals contract and caused problems in practice.
        - [Unfortunately] this behavior cannot be changed due to compatibility requirements.
        - [Also] very few exceptions, equals methods should perform deterministic computations on **memory-resident** objects

    - Non-nullity:

                    // explicit test for null(no need to do this)
                    @Override public boolean equals(Object o) {
                        if (o == null)
                            return false;
                        ...
                    }
                    ---------------------------------------------
                    // don’t need a separate null check.
                    //the type check will return false if null is passed in
                    @Override public boolean equals(Object o) {
                        if (!(o instanceof MyType))
                            return false;
                        MyType mt = (MyType) o;
                        ...
                    }



---



- The Liskov substitution principle
    - any important property of a type should also hold for its subtypes, so that any method written for the type should work equally well on its subtypes.

            // Let’s suppose we want to write a method to tell whether an integer point is on
            // the unit circle. Here is one way we could do it:
            // Initialize UnitCircle to contain all Points on the unit circle

            private static final Set<Point> unitCircle;
                static {
                    unitCircle = new HashSet<Point>();
                    unitCircle.add(new Point( 1, 0));
                    unitCircle.add(new Point( 0, 1));
                    unitCircle.add(new Point(-1, 0));
                    unitCircle.add(new Point( 0, -1));
                }
                public static boolean onUnitCircle(Point p) {
                    return unitCircle.contains(p);
            }
            ---------------------------------------------

            public class CounterPoint extends Point {
                private static final AtomicInteger counter = new AtomicInteger();
                public CounterPoint(int x, int y) {
                    super(x, y);
                    counter.incrementAndGet();
                }
                public int numberCreated() { return counter.get(); }
            }

            // But suppose we pass a CounterPoint instance to the onUnitCircle method.
            // If the Point class uses a getClass-based equals method, the onUnitCircle method will return false
            // regardless of the CounterPoint instance’s x and y values.
            // the HashSet used by the onUnitCircle method, use the equals method to test for containment,
            // and no CounterPoint instance is equal to any Point .

            ---------------------------------------------
            // There is a fine workaround: Item 16 "Favor composition over inheritance."

            // Adds a value component without violating the equals contract
            public class ColorPoint {
                private final Point point;
                private final Color color;
                public ColorPoint(int x, int y, Color color) {
                    if (color == null)
                        throw new NullPointerException();
                    point = new Point(x, y);
                    this.color = color;
                }

                /**
                * Returns the point-view of this color point.
                */
                public Point asPoint() {
                    return point;
                }

                @Override public boolean equals(Object o) {
                if (!(o instanceof ColorPoint))
                    return false;
                ColorPoint cp = (ColorPoint) o;
                return cp.point.equals(point) && cp.color.equals(color);
                }
                ...
            }

- keep them separate
    - There are some classes in the Java platform libraries that do extend an instantiable class and add a value component.
    - java.sql.Timestamp **extends** java.util.Date AND **adds** a nanoseconds field
        - The equals implementation for Timestamp does violate symmetry
        - The Timestamp class has a disclaimer cautioning programmers against mixing dates and timestamps. e.g, used in the same collection

---

- Item 20, "Prefer class hierarchies to tagged classes."
  - Note that you can add a value component to a subclass of an abstract class without violating the equals contract.

                    e.g
                        1. abstract class Shape with no value components
                        2. a subclass Circle that adds a radius field
                        3. a subclass Rectangle that adds length and width fields

> Problems of the sort shown above won’t occur so long as it is impossible to create a superclass instance directly.

---

- **A high-quality equals method should**:
    - 1.Use the == operator to check if the argument is a reference to this object.
    - 2.Use the instanceof operator to check if the argument has the correct type.
        - (1)the correct type is the class in which the method occurs.
        - (2)the class implements an interface that refines the equals contract to permit comparisons across classes that implement the interface.
            - [e.g]Collection interfaces such as Set , List , Map , and Map.Entry have this property
    - 3.Cast the argument to the correct type.
    - 4.For each “significant” field in the class, check if that field of the argument matches the corresponding field of this object.
        - For interface -> must access the argument’s fields via interface methods
        - For primitive fields(NOT float or double), using \=\=
        - For object reference fields -> invoke the equals method recursively
        - For float & double -> Float.compare & Double.compare (because the existence of Float.NaN , -0.0f)
        - For array fields -> check each element OR Arrays.equals
        - For object reference fields that may legitimately **contain null** -> to avoid the possibility of a **NullPointerException**
            - use this idiom to compare such fields:

                    (field == null ? o.field == null : field.equals(o.field))
                    // faster alternative
                    (field == o.field || (field != null && field.equals(o.field)))
        - For some classes, field comparisons are more complex than simple equality tests. (CaseInsensitiveString)
            - store a canonical form of the field
            - equals method can do cheap exact comparisons
        -[Performance] affected by the order, For best performance
            - compare fields that are more likely to differ, less expensive to compare, or, ideally, both.
            - compare redundant fields that is summary description of the entire object (like the area of a Polygon)
            - must NOT compare fields
                - not part of an object’s logical state (Lock fields used to synchronize operations)
                - redundant fields (be calculated from "significant fields,")
    - 5.When you are finished writing your equals method, ask yourself three questions: Is it symmetric? Is it transitive? Is it consistent?

---

- Final caveats
    - Always override hashCode when you override equals (Item 9).
    - Don’t try to be too clever.(overly aggressive in searching for equivalence)
        - e.g, the File class shouldn’t attempt to equate symbolic links referring to the same file.
    - Don’t substitute another type for Object in the equals declaration.(overloads)

                            // The problem is that this method does not override Object.equals
                            public boolean equals(MyClass o) {
                                ...
                            }
                            ------------------------------------------------------------------
                            // @Override annotation
                            @Override public boolean equals(MyClass o) {
                                ...
                            }

