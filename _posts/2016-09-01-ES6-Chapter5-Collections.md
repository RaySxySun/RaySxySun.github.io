---
layout: post
title: ES6 Chapter 5 Collections
date: 2016-09-01
weather: rainy
categories: JS
tags: [JS]
description: 
---

# ES6 Chapter 5 Collections

> As of ES6, some of the most useful data structure abstractions have been added as native components of the language.

- ### 1.Typed Arrays
	- (1) what's it:
		- access to binary date
		- create a typed arrays based on a bit-bucket
		- an array of 8-bit signed integers, 16-bit signed integers, etc.
	- (2) Endianness
		- Endian means if the low-order byte is on the right or the left of the number’s bytes
		- [Example] base-10 number 3085
			- 0000110000001101 / 0c0d (big endian)
			- 0000110100001100 / 0d0c (little endian)
	- (3) Multiple Views
		- A single buffer can have multiple views attached to it
	- (4) extra parameters: byteOffset and length.
		- (buffer,[offset, [length]])
	- (5) Typed Array Constructors
		- Typed array constructors also support these forms(In addition to the (buffer,[offset, [length]]))
			- \[constructor\](length)
			- \[constructor\](typedArr)
			- \[constructor\](obj)
		- typed array constructors are available as of ES6:
			- Int8Array & Uint8Array
			- Int16Array & Uint16Array
			- Int32Array & Uint32Array
			- Float32Array
			- Float64Array


				================(1)construct a bit-bucket================
				var buf = new ArrayBuffer( 32 );
				buf.byteLength; 				// 32-bytes long (256-bits) all 0’s
				================(1)construct a typed array===============
				var arr = new Uint16Array( buf );
				arr.length;						// 16-bit unsigned integers & 16 elements
				================(2)test the endianness===================
				// for most browsers, it should return true.
				var littleEndian = (function() {
					var buffer = new ArrayBuffer( 2 );
					new DataView( buffer ).setInt16( 0, 256, true );
					return new Int16Array( buffer )[0] === 256;
				})();	
				================(3)test the endianness===================
				var buf = new ArrayBuffer( 2 );
				var view8 = new Uint8Array( buf );
				var view16 = new Uint16Array( buf );
				view16[0] = 3085;
				view8[0];					// 13
				view8[1]; 					// 12
				view8[0].toString( 16 );	// "d"
				view8[1].toString( 16 ); 	// "c"
				// swap (as if endian!)
				var tmp = view8[0];
				view8[0] = view8[1];
				view8[1] = tmp;
				view16[0];					// 3340
				================(4)byteOffset and length=================
				// the buf includes 
				// (1) ONE 2-byte number
				// (2) TWO 1-byte numbers  
				// (3) ONE 32-bit floating point number
				var first = new Uint16Array( buf, 0, 2 )[0],
					second = new Uint8Array( buf, 2, 1 )[0],
					third = new Uint8Array( buf, 3, 1 )[0],
					fourth = new Float32Array( buf, 4, 4 )[0];
				================(5)convert them to a real array==========
				// ES5
				Array.prototype.slice( arr );
				// ES6
				Array.from( arr );

---

- ### 2.Map

	- (1) the major drawback of objects-as-maps 
		- it is the inability to use a non-string value as the key
		- stringify to "[object Object]"
	- (2) use Map(..)
		- set(..), delete(..), size, clear(), entries()
		- The Map(..) constructor can also receive an iterable 
	- (3) Map Values
		- values(..), includes(..), entries()
	- (4) Map Keys
		- keys()

					==================(1)the major drawback==================
					// The two objects x and y both stringify to "[object Object]"
					var m = {};
					var x = { id: 1 },
					y = { id: 2 };
					m[x] = "foo";
					m[y] = "bar";
					m[x]; 		// "bar"
					m[y];		// "bar"
					======(1)fake maps by maintaining a parallel array=======
					// O(1) time-complexity, but instead is O(n).
					var keys = [], vals = [];
					var x = { id: 1 },
					y = { id: 2 };
					keys.push( x );
					vals.push( "foo" );
					keys.push( y );
					vals.push( "bar" );
					keys[0] === x;			// true
					vals[0]; 				// "foo"
					keys[1] === y;			// true
					vals[1]; 				// "bar"
					===============(2) use Map================================
					var m = new Map();
					var x = { id: 1 },
					y = { id: 2 };
					m.set( x, "foo" );
					m.set( y, "bar" );
					m.get( x );				// "foo"
					m.get( y );				// "bar"
					m.delete( y );
					m.size;					// 1
					m.clear()				// clear
					===============(2) constructor=============================
					var m2 = new Map( m.entries() );
					// same as:
					var m2 = new Map( m );
					===============(3) Map Values==============================
					var m = new Map();
					var x = { id: 1 },
					y = { id: 2 };
					m.set( x, "foo" );
					m.set( y, "bar" );
					var vals = [ ...m.values() ];
					vals; // ["foo","bar"]
					m.includes( "foo" );			// true
					m.includes( "baz" );			// false
					var vals = [ ...m.entries() ];
					vals[0][0] === x;				// true
					vals[0][1]; 					// "foo"
					vals[1][0] === y; 				// true
					vals[1][1]; 					// "bar"
					===============(3) Map Keys=================================
					var m = new Map();
					var x = { id: 1 },
					y = { id: 2 };
					m.set( x, "foo" );
					m.set( y, "bar" );
					var keys = [ ...m.keys() ];
					keys[0] === x;					// true
					keys[0] === y;					// true
					m.has( x );						// true
					m.has( y );						// false

---

- ### 3.WeakMaps
	- (1) the same external behavior but differs underneath in how the memory allocation (specifically its GC) works.
	- (2) WeakMaps take (only) objects as keys


				==============(2)================
				var m = new WeakMap();
				var x = { id: 1 },
				y = { id: 2 };
				m.set( x, "foo" );
				m.has( x );				// true
				m.has( y );				// false

---

- ### 4.Sets
	- (1) [Definition]A set is an a collection of unique values
	- (2) [Methods] add(..), has(..) 
	- (3) Set Iterators
	- (4) WeakSets

				==============(2)================
				var s = new Set();
				var x = { id: 1 },
				y = { id: 2 };
				s.add( x ).add( y ).add( x );
				s.has( x );			// true
				s.size; 			// 2
				s.delete( y );
				s.size; 			// 1
				s.clear();
				s.size; 			// 0		
				==============(3)================
				var s = new Set( [1,2,3,4,"1",2,4,"5"] ),
				uniques = [ ...s ];
				uniques;			// [1,2,3,4,"1","5"]
				==============(4)================
				var s = new WeakSet();
				var x = { id: 1 },
				y = { id: 2 };
				s.add( x ).add( y );
				x = null;				// `x` is GC-able
				y = null;				// `y` is GC-able

