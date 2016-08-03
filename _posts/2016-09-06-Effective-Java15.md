---
layout: post
title: Effective Java - Item 15 Minimize mutability
date: 2016-09-06
weather: sunny
categories: Java
tags: [Java]
description:
---

> Chapeter 4 Classes and Interfaces

> Immutable classes are easier to design, implement, and use than mutable classes. They are less prone
to error and are more secure. no methods may modify the object and that all its fields must be final. However,some immutable classes have one or more nonfinal fields in which they cache the results of expensive computations the first time they are needed.

# Item 15: Minimize mutability

- ### 1.Five rules to make a class immutable:
	- (1) Don’t provide any methods that modify the object’s state.
	- (2) Ensure that the class can’t be extended.
		- 1) making the class final
		- 2) 
	- (3) Make all fields final.
	- (4) Make all fields private.
	- (5) Ensure exclusive access to any mutable components.

---

- ### 2.Benefits:
	- (1) exactly one state
		- Immutable objects are simple.
	- (2) thread-safe
		- Immutable objects are inherently thread-safe; they require no synchronization.
	- (3) immutable objects can be shared freely. Two ways:
		- 1) provide public static final constants
			- e.g. public static final Complex ZERO = new Complex(0, 0);
		- 2) can provide static factories (Item 1)
			- to share instances instead of creating new ones
				- [Benefit 1] reducing memory footprint 
				- [Benefit 2] reducing garbage collection costs
				- [Benefit 3] never have to make defensive copies
				- [Benefit 4] need not and should not provide a clone method or copy constructor
	- (4) share their internals
		- e.g. the BigInteger class uses a sign-magnitude representation internally. The negate method produces a new BigInteger of like magnitude and opposite sign. the newly created BigInteger ** points to the same internal array** as the original.
	- (5) Immutable objects make great building blocks for other objects
		- immutable objects make great map keys and set elements
		- don’t have to worry about their values changing once they’re in the map or set, which would destroy the map or set’s invariants.

---

- ### 3.Disadvantage:
	- [Disadantage] they require a separate object for each distinct value.
	- [Details] The operation requires time and space proportional to the size of the BigInteger
		- e.g [multistep operation] **generates a new object at every step, eventually discarding all objects except the final result**. Like BigInteger , BitSet represents an arbitrarily long sequence of bits, but unlike BigInteger , BitSet is mutable.
		- [workaround] for multistep operation, (1) a primitive OR (2)package-private mutable "companion class"
		- the implementors of BigInteger did the hard work for you. (package-private mutable "companion class")
	- [Best Solution] public mutable companion class (if you can accurately predict which complex multistage operations)
		- the String class has a mutable companion, StringBuilder
		- under certain circumstances, BitSet plays the role of mutable companion to BigInteger

				======================A Sample: Immutable Class========================
					public final class Complex {
						private final double re;
						private final double im;
						
						public Complex(double re, double im) {
							this.re = re;
							this.im = im;
						}
						// Accessors with no corresponding mutators
						
						public double realPart() { return re; }
						public double imaginaryPart() { return im; }
						
						public Complex add(Complex c) {
							return new Complex(re + c.re, im + c.im);
						}
						public Complex subtract(Complex c) {
							return new Complex(re - c.re, im - c.im);
						}
						public Complex multiply(Complex c) {
							return new Complex(re * c.re - im * c.im, re * c.im + im * c.re);
						}
						public Complex divide(Complex c) {
							double tmp = c.re * c.re + c.im * c.im;
							return new Complex((re * c.re + im * c.im) / tmp, (im * c.re - re * c.im) / tmp);
						}
						
						@Override public boolean equals(Object o) {
							if (o == this)
								return true;
							if (!(o instanceof Complex))
								return false;
							Complex c = (Complex) o;
							// See page 43 to find out why we use compare instead of ==
							return Double.compare(re, c.re) == 0 &&
							Double.compare(im, c.im) == 0;
						}
						@Override public int hashCode() {
							int result = 17 + hashDouble(re);
							result = 31 * result + hashDouble(im);
							return result;
						}
						private int hashDouble(double val) {
							long longBits = Double.doubleToLongBits(re);
							return (int) (longBits ^ (longBits >>> 32));
						}
						@Override public String toString() {
							return "(" + re + " + " + im + "i)";
						}
					}	

---

- ### 4.Alternatives

	- (1) [more flexible way] for the principle: a class must not permit itself to be subclassed
		- 1) Make all of its constructors private or package-private
		- 2) And add public static factories in place of the public constructors
		- [Pros] it possible to tune the performance of the class in subsequent releases by improving the object-caching capabilities of the static factories.
	- (2) [security]defensively copy
	- (3) [cache] saving the cost of recalculation (lazy initialization)
		- [too strong]no methods may modify the object and that all its fields must be final
		- [In truth] some immutable classes have one or more nonfinal fields in which they cache the results of expensive computations the first time they are needed. 

		==================(1) Private Constructor + Static Factory===================
		// Immutable class with static factories instead of constructors
		public class Complex {
			private final double re;
			private final double im;

			private Complex(double re, double im) {
				this.re = re;
				this.im = im;
			}

			public static Complex valueOf(double re, double im) {
				return new Complex(re, im);
			}
			... // Remainder unchanged
		}
		/*
		 *To its clients that reside outside its package, the immutable class is
		 *effectively final because it is impossible to extend a class that comes from another
		 *package and that lacks a public or protected constructor.
		 */
		==================(2) Defensively copy===================
		 public static BigInteger safeInstance(BigInteger val) {
			if (val.getClass() != BigInteger.class)
				return new BigInteger(val.toByteArray());
			return val;
		}
		==================(2) Defensively copy===================
		// For example, PhoneNumber ’s hashCode method (Item 9, page 49) 
		// computes the hash code the first time it’s invoked and caches it in case it’s invoked again.
		// Lazily initialized, cached hashCode
		
		private volatile int hashCode; // (See Item 71)
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

- ### 5.One caveat should be added concerning serializability
	- If immutable class implement Serializable & it contains 1+ fields(refer to mutable objects) you must:  
		- (1) provide an explicit readObject or readResolve method, OR
		- (2) use the ObjectOutputStream.writeUnshared and ObjectInputStream.readUnshared methods
		- [even if] the default serialized form is acceptable.
		- [Otherwise] an attacker could create a mutable instance of your not-quite-immutable class(Item 76).

---

- ### 6.To summarize, 
	- Classes should be immutable unless there’s a very good reason to make them mutable.
	- many advantages, but the only disadvantage is the potential for performance problems under certain circumstances.
	- several classes in the Java platform libraries should have been immutable but aren’t
		- java.util.Date and java.awt.Point
	- make small value objects immutable (such as PhoneNumber and Complex)
	- make larger value objects immutable (String and BigInteger)
	- provide a public mutable companion class for immutable class
	- If a class cannot be made immutable, limit its mutability as much as possible.
		- make every field final unless there is a compelling reason to make it nonfinal.