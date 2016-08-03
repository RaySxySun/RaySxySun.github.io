---
layout: post
title: ES6 Chapter 6 Array API
date: 2016-09-07
weather: rainy
categories: JS
tags: [JS]
description: 
---

# ES6 Chapter 6 API Additions # Array API

> ES6 adds many static properties and methods to various built-in natives and objects to help with common tasks.

- ### 1.Array
	- (1) [Static Function] Array.of(..)
		- [Purpose 1] to resolve the quirky "empty slots" behavior of Array(..) constructor
		- [Purpose 2] create and initialize elements in an instance of your subclass,
	- (2) [Static Function] Array.from(..) 
		- [Background] An “array-like object” in JavaScript is an object that has a length property on it
		- [Purpose] it’s been quite common to need to transform them into an actual array
		- [The 2nd argument] Mapping: The second argument, if provided, is a mapping callback
	- (3) Creating Arrays And Subtypes
		- [Note] @@species setting is only used for the prototype methods, like slice(..)
	- (4) [Prototype Method] Array#copyWithin(..)
		- [In Essense] it is a new mutator method
		- [Behavior] copies a portion of an array to another location in the same array
		- [Arguments] target, start, end
		- [Note] the copying algorithm reverses direction(see below)
	- (5) [Prototype Method] Array#fill(..)
		- [Arguments] element(s), start, end 
	- (6) [Prototype Method] Array#find(..)
		- [Background] no way to override the matching algorithm for indexOf(..). -1 is unfortunate/ungraceful
			- indexOf(..) comparison requires a strict === match. e.g. "2" fails to find a value of 2 , and vice versa.
		- [workaround] ES5: to have control over the matching logic has been the some(..) method
	- (7) [Prototype Method] findIndex(..)
	- (8) [Prototype Methods] entries() , values() , keys()

				================(1)"empty slots" =================
				var a = Array( 3 );
				a.length;					// 3
				a[0]; 						// undefined
				var b = Array.of( 3 );
				b.length;					// 1
				b[0]; 						// 3
				var c = Array.of( 1, 2, 3 );
				c.length;  					// 3
				c; 							// [1,2,3]
				================(1)"empty slots" =================
				class MyCoolArray extends Array {
					sum() {
						return this.reduce( function reducer(acc,curr){
							return acc + curr;
						}, 0 );
					}
				}
				var x = new MyCoolArray( 3 );
				x.length;					// 3 -- oops!
				x.sum(); 					// 0 -- oops!
				var y = [3];				// Array, not MyCoolArray
				y.length;					// 1
				y.sum(); 					// `sum` is not a function
				var z = MyCoolArray.of( 3 );
				z.length;					// 1
				z.sum(); 					// 3
				---------------(1) How does Reduce work---------------
				[0, 1, 2, 3, 4].reduce(function(previousValue, currentValue, index, array){
				  return previousValue + currentValue;
				});

				// the same as above
				[0, 1, 2, 3, 4].reduce( (prev, curr) => prev + curr );


previousValue | currentValue | index | array | return value
first call | 0 | 1 | 1 | [0,1,2,3,4] | 1
second call | 1 | 2 | 2 | [0,1,2,3,4] | 3
third call | 3 | 3 | 3 | [0,1,2,3,4] | 6
fourth call | 6 | 4 | 4 | [0,1,2,3,4] | 10


				================(2)"array-like object" =================
				// array-like object
				var arrLike = {
					length: 3,
					0: "foo",
					1: "bar"
				};
				// Before ES6
				var arr = Array.prototype.slice.call( arrLike );
				// More common: we use slice() to duplicate a real array
				var arr2 = arr.slice();

				// ES6 Array.from(..) can cover these 2 cases
				var arr = Array.from( arrLike );
				var arrCopy = Array.from( arr );
				================(2)undefined slots=========================
				var arrLike = {
					length: 4,
					2: "foo"
				};
				Array.from( arrLike ); 			// [ undefined, undefined, "foo", undefined ]
				
				var emptySlotsArr = [];
				emptySlotsArr.length = 4;
				emptySlotsArr[2] = "foo";
				Array.from( emptySlotsArr );	// [ undefined, undefined, "foo", undefined ]
				================(2) Empty slots============================
				// produce an array initialized to a certain length with actual undefined values in each slot
				// Prior to ES6
				var a = Array( 4 ); 							// four empty slots!
				var b = Array.apply( null, { length: 4 } ); 	// four `undefined` values
				// As of ES6
				var c = Array.from( { length: 4 } ); 			// four `undefined` values
				===============(2) [The 2nd argument]======================
				var arrLike = {
					length: 4,
					2: "foo"
				};
				Array.from( arrLike, function mapper(val,idx){
					if (typeof val == "string") {
						return val.toUpperCase();
					}
					else {
						return idx;
					}
				} );
				// [ 0, 1, "FOO", 3 ]
				===============(3) create Array & Subtypes==================
				class MyCoolArray extends Array { .. }
				Array.of( 1, 2 ) instanceof Array;						// true
				Array.from( [1, 2] ) instanceof Array; 					// true

				MyCoolArray.of( 1, 2 ) instanceof Array;				// false
				MyCoolArray.from( [1, 2] ) instanceof Array; 			// false
				
				MyCoolArray.of( 1, 2 ) instanceof MyCoolArray;			// true
				MyCoolArray.from( [1, 2] ) instanceof MyCoolArray; 		// true

				var x = new MyCoolArray( 1, 2, 3 );
				x.slice( 1 ) instanceof MyCoolArray;					// true

				//override Symbol.species
				class MyCoolArray extends Array {
					// force `species` to be parent constructor
					static get [Symbol.species]() { return Array; }
				}
				var x = new MyCoolArray( 1, 2, 3 );
				x.slice( 1 ) instanceof MyCoolArray;					// false
				x.slice( 1 ) instanceof Array;							// true

				// Note: @@species setting is only used for the prototype methods, like slice(..)
				MyCoolArray.from( x ) instanceof Array;					// false
				MyCoolArray.of( [2, 3] ) instanceof Array; 				// false
				===================(4) copyWithin(..)========================x
				[1,2,3,4,5].copyWithin( 3, 0 ); 				// [1,2,3,1,2]
				[1,2,3,4,5].copyWithin( 3, 0, 1 ); 				// [1,2,3,1,5]
				[1,2,3,4,5].copyWithin( 0, -2 ); 				// [4,5,3,4,5]
				[1,2,3,4,5].copyWithin( 0, -2, -1 ); 			// [4,2,3,4,5]
				// the copying algorithm reverses direction and copies 4 to overwrite 5 , then copies 3 to overwrite 4 , then copies 2 to overwrite 3 , and the final result is [1,2,2,3,4]
				[1,2,3,4,5].copyWithin( 2, 1 );					// [1,2,2,3,4] 
				=======================(5) fill(..)===========================
				var a = Array( 4 ).fill( undefined );
				a; 				// [undefined,undefined,undefined,undefined]		

				var a = [ null, null, null, null ].fill( 42, 1, 3 );
				a;				// [null,42,42,null]
				=========(5) ES5 workaround && ES6 find && ES6 findIndex======
				var a = [1,2,3,4,5];
				a.some( function matcher(v){
					return v == "2";			// true
				} ); 
				a.some( function matcher(v){
					return v == 7;				// false
				} ); 

				// find(..)
				var points = [
					{ x: 10, y: 20 },
					{ x: 20, y: 30 },
					{ x: 30, y: 40 },
					{ x: 40, y: 50 },
					{ x: 50, y: 60 }
				];

				points.find( function matcher(point) {
					return ( point.x % 3 == 0 && point.y % 4 == 0 );
				} );    		// { x: 30, y: 40 }

				points.findIndex( function matcher(point) {
					return (	point.x % 3 == 0 && point.y % 4 == 0 );
				} );
																// 2
				points.findIndex( function matcher(point) {
					return (
						point.x % 6 == 0 && point.y % 7 == 0
					);
				} );											// -1
				==========(8)entries() , values() , keys() ==============
				var a = [1,2,3];
				[...a.values()];			// [1,2,3]
				[...a.keys()];				// [0,1,2]
				[...a.entries()]; 			// [ [0,1], [1,2], [2,3] ]
				[...a[Symbol.iterator]()]; 	// [1,2,3]

				var a = []; a.length = 3; a[1] = 2;
				[...a.values()];			// [undefined,2,undefined]
				[...a.keys()]; 				// [0,1,2]
				[...a.entries()];			// [ [0,undefined], [1,2], [2,undefined] ]



