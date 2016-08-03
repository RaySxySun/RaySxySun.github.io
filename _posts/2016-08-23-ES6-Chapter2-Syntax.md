---
layout: post
title: ES6 Chapter 2 Syntax 1
date: 2016-08-23
weather: cloudy
categories: JS
tags: [JS]
description:
---

> [ES6 Testing](http://www.es6fiddle.net/)

# ES6 Chapter 2 Syntax 1

- ### 1.Block-Scoped Declarations:
	- [Not New Feature] the fundamental unit of variable scoping in JavaScript has always been the function .
		- IIFE (immediately invoked function expression)

					var a = 2;

					(function IIFE(){
						var a = 3;
						console.log( a ); // 3
					})();

					console.log( a ); // 2

	- [New Feature] 1.let Declarations
		- Unlike traditional var -declared variables, let declarations attach to the block scope but are not initialized until they appear in the block.

					var a = 2;
					{
						let a = 3;
						console.log( a ); // 3
					}
					console.log( a ); // 2

					--------------------------
					//the BEST
					// recommend using just one let at the very top
					{
						let a = 2, b, c;
						// ..
					}

					--------------------------

					let (a = 2, b, c) {
						// ..
					}

		- block-scoped variables

					// the if statement contains b and c block-scoped variables
					// the for loop contains i and j block-scoped variables
					let a = 2;
					if (a > 1) {
						let b = a * 3;
						console.log( b ); // 6

						for (let i = a; i <= b; i++) {
							let j = i + 10
							console.log( j );  // 12 13 14 15 16
						}

						let c = a + b;
						console.log( c ); // 8
					}

		- ReferenceError!

					{
						console.log( a );	// undefined
						console.log( b 		// ReferenceError!
						var a;
						let b;
					}
		- let + for (!= var + for)

					// var + for = 5
					// let + for = 3
					var funcs = [];
					for (let i = 0; i < 5; i++) {
						funcs.push( function(){
							console.log( i );
						} );
					}
					funcs[3](); // 3

	- [New Feature] 2.const Declarations (block-scoped declaration)
		- Constants are not a restriction on the value itself, but on the variable assignment of that value.


				{
					const a = [1,2,3];
					a.push( 4 );
					console.log( a ); // [1,2,3,4]
					a = 42; // TypeError!
				}

- ### 2.Spread / Rest
	- [New Feature] 1.Spread:

				--------------function call------------
				// replace foo.apply( null, [1,2,3] );
				// 1 2 3
				function foo(x,y,z) {
					console.log( x, y, z );
				}

				foo( ...[1,2,3] ); // 1 2 3

				--------------array concat(..)------------
				// [1].concat( a, [5] )
				var a = [2,3,4];
				var b = [ 1, ...a, 5 ];
				console.log( b ); // [1,2,3,4,5]
-
	- [New Feature] 2.Rest: ... gathers a set of values

				//gather the rest of the arguments (if any) into an array called z .
				function foo(x, y, ...z) {
					console.log( x, y, z );
				}
				foo( 1, 2, 3, 4, 5 ); // 1 2 [3,4,5]

				-----------------------------------------------

				function foo(...args) {
					console.log( args );
				}
				foo( 1, 2, 3, 4, 5);
				// [1,2,3,4,5]

				--------NEW feature VS old-school way----------

				// doing things the new ES6 way
				function foo(...args) {
					// `args` is already a real array
					// discard first element in `args`
					args.shift();
					// pass along all of `args` as arguments
					// to `console.log(..)`
					console.log( ...args );
				}

				// doing things the old-school pre-ES6 way
				function bar() {
					// turn `arguments` into a real array
					var args = Array.prototype.slice.call( arguments );

					// add some elements on the end
					args.push( 4, 5 );

					// filter out odd numbers
					args = args.filter( function(v){
						return v % 2 == 0;
					} );

					// pass along all of `args` as arguments
					// to `foo(..)`
					foo.apply( null, args );
				}

				bar( 0, 1, 2, 3 ); // 2 4

- ### 3.Default Parameter Values
	- [Old-school]

					function foo(x,y) {
						x = x || 11;
						y = y || 31;
						console.log( x + y );
					}

					foo( 0, 42 ); // 53 <-- Oops, not 42

					-----------write the check more verbosely---------

					function foo(x,y) {
						x = (x !== undefined) ? x : 11;
						y = (y !== undefined) ? y : 31;
						console.log( x + y );
					}

	- [New Feature] 1.Default values

		- [can] only omit arguments at the end(righthand side)
		- [cannot] omit arguments in the middle or at the beginning of the arguments list.

					// `undefined` means missing.
					function foo(x = 11, y = 31) {
						console.log( x + y );
					}

					foo( 5 ); // 36
					foo( 5, undefined ); // 36 <-- `undefined` is missing
					foo( 5, null );  // 5 <-- null coerces to `0`

	- [New Feature] 2.Default Value Expressions
		- the default value expressions are lazily evaluated
		- not-yet-initialized-at-that-moment parameter variable

					function bar(val) {
						console.log( "bar called!" );
						return y + val;
					}

					function foo(x = y + 3, z = bar( x )) {
						console.log( x, z );
					}

					var y = 5;
					foo();	// 8 13
					foo( undefined, 10 ); // 9 10 lazily evaluated

		- TDZ: temporal dead zone (as let Declarations)
			- prevents a variable from being accessed in its uninitialized state.

					var w = 1, z = 2;
						function foo( x = w + 1, y = x + 1, z = z + 1 ) {
						console.log( x, y, z );
					}

					foo(); // ReferenceError

		- There will very rarely be any cases where an IIFE (or any other executed inline function expression) will be appropriate for default value expressions.

					function foo( x = (function(v){ return v + 11; })( 31 ) ) {
						console.log( x );
					}

					foo(); // 42

- ### 4.Destructuring
	- [New Feature] 1.destructuring assignment: array destructuring and object destructuring
		- destructuring assignment VS. structured assignment(Manually assigning indexed values)
		- which eliminates the need for the tmp variable, making them much cleaner

				function foo() {
					return [1,2,3];
				}
				var tmp = foo(), a = tmp[0], b = tmp[1], c = tmp[2];
				console.log( a, b, c );
				// 1 2 3

				function bar() {
					return { x: 4, y: 5, z: 6 };
				}
				var tmp = bar(), x = tmp.x, y = tmp.y, z = tmp.z;
				console.log( x, y, z );
				// 4 5 6
				------------------Destructuring-----------------------
				var [ a, b, c ] = foo();
				var { x: x, y: y, z: z } = bar();
				console.log( a, b, c );  // 1 2 3
				console.log( x, y, z );  // 4 5 6

	- [New Feature] Object Property Assignment Pattern (shorten the syntax)
		- normal object literals [target: source]
		- destructuring assignment [source: target]

				var { x, y, z } = bar(); console.log( x, y, z ); // 4 5 6
				var { x: bam, y: baz, z: bap } = bar(); console.log( bam, baz, bap ); // 4 5 6

	- Not Just Declarations
		- destructuring is a general assignment operation, not just a declaration
		- should use var, let and const declarations
		- leaving off a var/let/const declarator
			- had to surround the whole assignment expression in ( )

					[o.a, o.b, o.c] = foo();
					( { x: o.x, y: o.y, z: o.z } = bar() );

					//computed expression
					var which = "x", o = {};
					( { [which]: o[which] } = bar() );
					console.log( o.x ); //4
		- swap two variables

					[ y, x ] = [ x, y ];

	- [New Feature] Too Many, Too Few, Just Enough
		- do not have to assign all the values that are present.
		- `undefined` is missing

					var [,b] = foo(); var { x, z } = bar();
					console.log( b, x, z ); // 2 4 6

					var [,,c,d] = foo(); var { w, z } = bar();
					console.log( c, z ); // 3 6
					console.log( d, w ); // undefined undefined

		- ... operator [For Array]

					// spreading a out
					var a = [2,3,4]; var [b, ...c] = a;
					console.log( b, c ); // 2 [3,4]

					// gather
					var a = [2,3,4];
					var [b, ...c] = a;
					console.log( b, c ); // 2 [3,4]

	- [New Feature] Default Value Assignment
		- combine the default value assignment with the alternate assignment expression syntax

					var [ a = 3, b = 6, c = 9, d = 12 ] = foo();
					var { x = 5, y = 10, z = 15, w = 20 } = bar();
					console.log( a, b, c, d ); // 1 2 3 12
					console.log( x, y, z, w ); // 4 5 6 20
					----------------------------------------------
					var { x, y, z, w: WW = 20 } = bar();
					console.log( x, y, z, WW ); // 4 5 6 20

	- [New Feature] Nested Destructuring
		- a simple way to flatten out object namespaces

						var App = {
							model: {
								User: function(){ .. }
							}
						};
						// var User = App.model.User;
						var { model: { User } } = App;

	- [New Feature] Destructuring Parameters
		- This technique is an approximation of named arguments

						function f1([ x=2, y=3, z ]) { .. }
						function f2([ x, y, ...z], w) { .. }
						function f3([ x, y, ...z], ...w) { .. }
						function f4({ x: X, y }) { .. }
						function f5({ x: X = 10, y = 20 }) { .. }
						//Destructuring Defaults + Parameter Defaults
						function f6({ x = 10 } = {}, { y } = { y: 10 }) { .. }

						f3( [] ); // undefined undefined [] []
						f3( [1,2,3,4], 5, 6 ); // 1 2 [3,4] [5,6]

		- [New Feature] Destructuring Defaults + Parameter Defaults
			- still a bit fuzzy

						function f6({ x = 10 } = {}, { y } = { y: 10 }) {
							console.log( x, y );
						}
						f6(); 						// 10 10
						f6( {}, undefined );		// 10 10
						f6( undefined, undefined );	// 10 10

						f6( {}, {} ); 				// 10 undefined
						f6( undefined, {} );		// 10 undefined
						f6( { x: 2 }, { y: 3 } );	// 2 3

		- Nested Defaults: Destructured and Restructured
			- temporary variables hanging around would pollute scope. So, let’s use block scoping

						// merge `defaults` into `config`
						{
							// destructure (with default value assignments)
							let {
								options: {
									remove = defaults.options.remove,
									enable = defaults.options.enable,
									instance = defaults.options.instance
								} = {},
								log: {
									warn = defaults.log.warn,
									error = defaults.log.error
								} = {}
							} = config;

						// restructure
						config = {
							options: { remove, enable, instance },
							log: { warn, error }
							};
						}
