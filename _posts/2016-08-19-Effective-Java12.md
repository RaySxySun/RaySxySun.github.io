---
layout: post
title: Effective Java - Item 12 Consider implementing Comparable
date: 2016-08-19
weather: cloudy
categories: Java
tags: [Java]
description:
---

# Item 12 Consider implementing Comparable

> Unlike others, the compareTo method is not declared in Object. It is the sole method in the Comparable interface (similar to Object's equals method). It is generic.

> natural ordering: like Arrays.sort(a);

- ### 1.A case in point
	- the program relies on the fact that String implements Comparable ,

						public class WordList {
							public static void main(String[] args) {
								Set<String> s = new TreeSet<String>();
								Collections.addAll(s, args);
								System.out.println(s);
							}
						}

---

- ### 2.Ordering Types:
	- 1 alphabetical order 
	- 2 numerical order 
	- 3 chronological order,

//strongly consider implementing the interface

			public interface Comparable<T> {
				int compareTo(T t);
			}

---

- ### 3.Returning Values
	- 1.A negative integer(less than), zero(equal to), or a positive integer(greater than)
	- 2.Throws ClassCastException

---

- ### 4.Comparison Contract [Less Important]

> a compareTo method must obey the same restrictions imposed by the equals contract: reflexivity, symmetry, and transitivity.
	
> sgn (expression) designates the mathematical signum function

	- sgn(x.compareTo(y)) == -sgn(y.compareTo(x)) for all x and y 
	- transitive: (x.compareTo(y) > 0 && y.compareTo(z) > 0) implies x.compareTo(z) > 0 .
	- [consistent] x.compareTo(y) == 0 implies that sgn(x.compareTo(z)) == sgn(y.compareTo(z)) , for all z .
	- [strongly recommended, but not strictly required] (x.compareTo(y)== 0) == (x.equals(y))

> interclass comparisons: as of release 1.6, NO classes in the Java platform libraries that support them

---

- ### 5.Classes that depend on comparison (searching and sorting)
	- the sorted collections TreeSet and TreeMap
	- the utility classes Collections and Arrays

---

- ### 6.The same caveat applies:
	- no way to extend an instantiable class with a new value component while preserving the compareTo contract
		- unless forgo the benefits of object-oriented abstraction
		- don’t extend it; write an unrelated class containing an instance of the first class.
	- compareTo method imposes an order that is inconsistent with equals will still work
		- The interfaces (Collection , Set , or Map) may not obey the general contract, because their general contracts are defined in terms of the equals method.
		- BUT sorted collections (like, TreeSet and TreeMap) use the equality test imposed by compareTo in place of equals .
	- [Example] BigDecimal("1.0") and new BigDecimal("1.00")
		- A HashSet: Both of them are added to the set when compared using the equals method (unequal)
		- A TreeSet: Only one element because they are equal when compared using the compareTo method.

---

- ### 6.Differences from writing a equals method:
	- statically typed: don’t need to type check or cast its argument
		- wrong type: the invocation won’t even compile
		- null: throw a NullPointerException
	- Compare object reference fields by invoking the compareTo method recursively.
	- integral primitive fields using the relational operators < and >
	- Double.compare or Float.compare in place of the relational operators
	- multiple significant fields: compare next-most-significant fields.
	- Comparable<T>

				public final class CaseInsensitiveString implements Comparable<CaseInsensitiveString> {
					public int compareTo(CaseInsensitiveString cis) {
						return String.CASE_INSENSITIVE_ORDER.compare(s, cis.s);
					}
					... // Remainder omitted
				}

---

- ### 7.Improvement
	- compareTo does not specify the magnitude of the return value.
	- [extreme caution] non-negative; The differences <= Integer.MAX_VALUE (2^31 -1);
				public int compareTo(PhoneNumber pn) {
					// Compare area codes
					int areaCodeDiff = areaCode - pn.areaCode;
					
					if (areaCodeDiff != 0)
						return areaCodeDiff;
					
					// Area codes are equal, compare prefixes
					int prefixDiff = prefix - pn.prefix;
					if (prefixDiff != 0)
						return prefixDiff;
					
					// Area codes and prefixes are equal, compare line numbers
					return lineNumber - pn.lineNumber;
				}	



