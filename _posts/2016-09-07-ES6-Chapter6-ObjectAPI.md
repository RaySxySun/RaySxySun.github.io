---
layout: post
title: ES6 Chapter 6 Object API
date: 2016-09-07
weather: rainy
categories: JS
tags: [JS]
description: 
---

# ES6 Chapter 6 API Additions # Object API

> Traditionally, functions of Object have been seen as focused on the behaviors/capabilities of object values. However, starting with ES6, Object static functions will also be for general purpose global APIs of any sort that don’t already belong more naturally in some other location

- ### 2.Object
	- (1) [Static Function] Object.is(..)
		- [Feature] even more strict fashion than the === comparison
		- [Exceptions] two important exceptions: NaN or -0 value
	- (2) [Static Function] Object.getOwnPropertySymbols(..)
		- [Background] Symbols are likely going to be mostly used as special (meta) properties on objects.
		- [Purpose] retrieves only the symbol properties directly on an object
	- (3) [Static Function] Object.setPrototypeOf(..)
		- [Purpose] sets the [[Prototype]] of an object for the purposes of behavior delegation
	- (4) [Static Function] Object.assign(..)
		- [Simplified version] copying/mixing one object’s properties into another. like jQuery’s extend(..)
		- [Arguments] target, sources, returns the target object
		- [Note] For each source, its enumerable and own keys (not symbols) are copied (plain = assignment)
		- [Note] In other words, symbols, non-enumerable properties, and non-owned properties are all excluded from the assignment.
	

				================(1)two exceptions ======================
				var x = NaN, y = 0, z = -0;
				x === x;					// false
				y === z; 					// true
				Object.is( x, x );			// true
				Object.is( y, z ); 			// false
				================(2) retrieve symbol======================
				var o = {
					foo: 42,
					[ Symbol( "bar" ) ]: "hello world",
					baz: true
				};
				Object.getOwnPropertySymbols( o );		// [ Symbol(bar) ]
				================(3) Object.setPrototypeOf ================
				var o1 = { foo() { console.log( "foo" ); } };
				var o2 = { // .. o2's definition .. };
				Object.setPrototypeOf( o2, o1 ); 	// delegates to `o1.foo()`
				o2.foo();							// foo
				================(3) Alternatively=========================
				var o1 = { foo() { console.log( "foo" ); } };
				var o2 = Object.setPrototypeOf( 
							{ // .. o2's definition .. },
							 o1 
						 );			
				// delegates to `o1.foo()`
				o2.foo(); 			// foo
				================(4) Object.assign==========================
				var target = {},
				o1 = { a: 1 }, o2 = { b: 2 },
				o3 = { c: 3 }, o4 = { d: 4 };
				// setup read-only property
				Object.defineProperty( o3, "e", {
					value: 5,
					enumerable: true,
					writable: false,
					configurable: false
				} );
				// setup non-enumerable property
				Object.defineProperty( o3, "f", {
					value: 6,
					enumerable: false
				} );
				o3[ Symbol( "g" ) ] = 7;
				Object.setPrototypeOf( o3, o4 );
				
				// Only the properties a , b , c , and e will be copied to target 
				Object.assign( target, o1, o2, o3 );
				target.a;	// 1
				target.b;	// 2
				target.c;	// 3
				target.e;	// 5
				Object.getOwnPropertyDescriptor( target, "e" );
				// { value: 5, writable: true, enumerable: true, configurable: true }
				

