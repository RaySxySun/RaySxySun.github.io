---
layout: post
title: ES6 Chapter 6 Other API
date: 2016-09-07
weather: rainy
categories: JS
tags: [JS]
description: 
---

# ES6 Chapter 6 API Additions # Other API

> Note: 5 primitive type (1) Undefined, (2)Null(Object), (3)Boolean (4)Number, (5)String

- ### 3.Math
	- (1) Trigonometry:
		- cosh(..) - hyperbolic cosine
		- acosh(..) - hyperbolic arccosine
		- sinh(..) - hyperbolic sine
		- asinh(..) - hyperbolic arcsine
		- tanh(..) - hyperbolic tangent
		- atanh(..) - hyperbolic arctangent
		- hypot(..) - the squareroot of the sum of the squares 
	- (2) Arithmetic:
		- cbrt(..) - cube root
		- clz32(..) - count leading zeros in 32-bit binary representation
		- expm1(..) - the same as exp(x) - 1
		- log2(..) - binary logarithm (log base 2)
		- log10(..) - log base 10
		- log1p(..) - the same as log(x + 1)
		- imul(..) - 32-bit integer multiplication of two numbers
	- (3) Meta:
		- sign(..) - returns the sign of the number
		- trunc(..) - returns only the integer part of a number
		- fround(..) - rounds to nearest 32-bit (single precision) floating point value

- ### 4.Number
	- (1) Static Properties
		- Number.EPSILON - the minimum value between any two numbers: 2^-52
		- Number.MAX_SAFE_INTEGER - The highest integer that can “safely” be represented unambiguously in a JS number value: 2^53 
		- Number.MIN_SAFE_INTEGER - The lowest integer that can “safely” be represented unambiguously in a JS number value: -(2^53 - 1) or (-2)^53 + 1 .
	- (2) [Static Function] Number.isNaN(..)
		- [Background] The standard global isNaN(..) has been broken, it coerces the argument to a number type.
		- [Purpose] ES6 adds a fixed utility Number.isNaN(..)
	- (3) [Static Function] Number.isFinite(..)
		- [Background] The standard global isFinite(..) coerces its argument
		- [Behavior] but Number.isFinite(..) omits the coercive behavior
		- [Coercion] use Number.isFinite(+x) , which explicitly coerces x to a number before passing it in
	- (4) [Static Functions] Integer-related

				==============(2) Number.isNaN============================
				var a = NaN, b = "NaN", c = 42;
				isNaN( a );// true
				isNaN( b );// true -- oops!
				isNaN( c );// false

				Number.isNaN( a );// true
				Number.isNaN( b );// false -- fixed!
				Number.isNaN( c );// false

				================(3) Number.isFinite(..)======================
				var a = NaN, b = Infinity, c = 42;
				Number.isFinite( a ); // false
				Number.isFinite( b ); // false
				Number.isFinite( c ); // true

				var a = "42";
				isFinite( a );			// true
				Number.isFinite( a );	// false

				================(4) Integer-related ================
				Number.isInteger( 4 ); 			// true
				Number.isInteger( 4.2 ); 		// false
				Number.isInteger( NaN );		// false
				Number.isInteger( Infinity ); 	// false
				
				function isFloat(x) {
					return Number.isFinite( x ) && !Number.isInteger( x );
				}
				isFloat( 4.2 );				// true
				isFloat( 4 );				// false
				isFloat( NaN );				// false
				isFloat( Infinity );		// false

				var x = Math.pow( 2, 53 ),
				y = Math.pow( -2, 53 );
				Number.isSafeInteger( x - 1 );	// true
				Number.isSafeInteger( y + 1 ); 	// true
				Number.isSafeInteger( x );		// false
				Number.isSafeInteger( y ); 		// false

---

- ### 5.String
	- (1) Unicode Functions
		- String.fromCodePoint(..) 
		- String#codePointAt(..) 
		- String#normalize(..)
	- (2) repeat(..) Prototype Function
		- repeat(..)
			- "foo".repeat( 3 );
	- (3) String Inspection Functions
		- startsWith(..) 
		- endsWidth(..) 
		- includes(..)


