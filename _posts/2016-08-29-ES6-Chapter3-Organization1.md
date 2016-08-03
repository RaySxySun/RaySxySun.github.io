---
layout: post
title: ES6 Chapter 3 Organization 1
date: 2016-08-29
weather: sunny
categories: JS
tags: [JS]
description: 
---

# ES6 Chapter 3 Organization 1

> This chapter will explore Iterators, Generators, Modules, and Classes.

- ### 1.Iterators
	- Definition: a structured pattern for pulling information from a source in one-at-a-time fashion.
	- Interfaces: 
		- Iterator:
			- [required]: next() {method}: retrieves next IteratorResult
			- [optional]:
				- return() {method}: stops iterator and returns IteratorResult
				- throw() {method}: signals error and returns IteratorResult
		- IteratorResult: { value: .. , done: true / false }
			- value {property}: current iteration value or final return value
			- done {property}: boolean, indicates completion status
		- Iterable interface
			- @@iterator() {method}: produces an Iterator
				- @@iterator is the special built-in symbol representing the method that can produce iterator(s) for the object.
	- next()

						// Each time the method located at Symbol.iterator
						var arr = [1,2,3];
						var it = arr[Symbol.iterator]();
						it.next();		// { value: 1, done: false }
						it.next();		// { value: 2, done: false }
						it.next(); 		// { value: 3, done: false }
						it.next(); 		// { value: undefined, done: true }
						-----------------------------------------------------
						var greeting = "hello world";
						var it = greeting[Symbol.iterator]();
						it.next(); 		// { value: "h", done: false }
						it.next(); 		// { value: "e", done: false }
						..
						-----------collections generate an iterator----------
						var m = new Map();
						m.set( "foo", 42 );
						m.set( { cool: true }, "hello world" );
						var it1 = m[Symbol.iterator]();
						var it2 = m.entries();
						it1.next(); 	// { value: [ "foo", 42 ], done: false }
						it2.next(); 	// { value: [ "foo", 42 ], done: false }
						..
						
	- Optional: return(..) and throw(..)
		- not implemented on most of the built-in iterators
		- return(..): sending a signal to an iterator: cleanup (releasing/closing network, database, or file handle resources, etc.) 
			- the consuming code is complete and will not be pulling any more values from it.
		- throw(..): signal an exception/error to an iterator
			- can be caught with a try..catch

	- Iterator Loop:

						for (var v, res; !(res = it.next()) && !res.done; ) {
							v = res.value;
							console.log( v );	
						}
						-------------------Alternative------------------------
						var it = {
							// make the `it` iterator an iterable
							[Symbol.iterator]() { return this; },
							next() { .. },
						};
						it[Symbol.iterator]() === it; 	// true

						for (var v of it) {
							console.log( v );
						}
						------------------Custom Iterators---------------------
						var Fib = {
							[Symbol.iterator]() {
								var n1 = 1, n2 = 1;
								return {
									// make the iterator an iterable
									[Symbol.iterator]() { return this; },
									
									next() {
										var current = n2;
										n2 = n1;
										n1 = n1 + current;
										return { value: current, done: false };
									},

									return(v) {
										console.log("Fibonacci sequence abandoned.");
										return { value: v, done: true };
									}
								};
							}
						};

						for (var v of Fib) {
							console.log( v );
							if (v > 50) break;
						}
						// 1 1 2 3 5 8 13 21 34 55
						// Fibonacci sequence abandoned.

	- Iterator Consumption:
		- other ES6 structures which can consume iterators

						var a = [1,2,3,4,5];
						---------------The ... spread operator--------------
						function foo(x,y,z,w,p) {
							console.log( x + y + z + w + p );
						}
						foo( ...a ); // 15
						-----------spread an iterator inside an array-------
						var b = [ 0, ...a, 6 ];
						b; 		// [0,1,2,3,4,5,6]
						---------------Array destructuring------------------
						var it = a[Symbol.iterator]();
						var [x,y] = it;
						var [z, ...w] = it;
						it.next(); // { value: undefined, done: true }
						x;		// 1
						y;		// 1
						z;		// 3
						w;		// [4, 5]

---

> Consistency eases understanding and learning.

- ### 2.Generator:

	- Syntax: \*

						function *foo() {..}
						*foo() { .. } 		//concise methods

	- yield:  expression precedence
		- (1) a pause point
		- (2) an expression that sends out a value
		- (3) receives (e.g., is replaced by) the resumption value message
		- (4) appear anywhere a normal expression can
		----------------------------(1)--------------------------------
		// (1): If and when resumed, the last line of *foo() would run.
		function *foo() {
			var x = 10;
			var y = 20;
			yield;
			var z = x + y;
		}
		----------------------------(2)--------------------------------
		function *foo() {
			while (true) {
				yield Math.random();
			}
		}
		----------------------------(3)--------------------------------
		function *foo() {
			var x = yield 10;
			console.log( x );
		}
		----------------------------(4)--------------------------------
		function *foo() {
			var arr = [ yield 1, yield 2, yield 3 ];
			console.log( arr, yield 4 );
		}
		--------------------expression precedence----------------------
		var a, b;
		a = 3;  // valid
		b = 2 + a = 3;  // invalid
		b = 2 + (a = 3); // valid

		yield 3; // valid
		a = 2 + yield 3;// invalid
		a = 2 + (yield 3); // valid
		--------------------expression precedence----------------------
		yield 2 + 3; // same as `yield (2 + 3)`
		(yield 2) + 3; // `yield 2` first, then `+ 3`

	- yield \*: yield delegation

						--------------------yield *----------------------
						function *foo() {
							yield *[1,2,3];
						}
						var it = foo();
						console.log(it.next().value);
						console.log(it.next().value);
						console.log(it.next().value);
						---------------------Same 1----------------------
						function *foo() { yield *[1,2,3]; }
						for (var v of foo()) { console.log( v ); }
						---------------------Same 2----------------------
						function *foo() {
							yield 1;
							yield 2;
							yield 3;
						}
						function *bar() {
							yield *foo();
						}

	- Iterator Control: consume an iterator
		- (1) recursive *foo(..)
		- (2) for..of loop

						----------------------------(1)--------------------------------
						function *foo(x) {
							if (x < 3) {
								x = yield *foo( x + 1 );
							}
							return x * 2;
						}
						var it = foo( 1 );
						it.next(); 			// { value: 24, done: true }
						----------------------------(2)--------------------------------
						function *foo() { yield *[1,2,3]; }
						for (var v of foo()) { console.log( v ); }
						----------------------------(3)--------------------------------
						function *foo() {
							var x = yield 1;
							var y = yield 2;
							var z = yield 3;
							console.log( x, y, z );
						}

						var it = foo();
						// start up the generator
						it.next(); 			// { value: 1, done: false }
						// answer first question
						it.next( "foo" );	// { value: 2, done: false }
						// answer second question
						it.next( "bar" );	// { value: 3, done: false }
						// answer third question
						it.next( "baz" );	// "foo" "bar" "baz" // { value: undefined, done: true }

> think of a generator as (1)a generator as a producer of values, (2)controlled, progressive code execution

	- Early Completion
		- The purpose: if the controlling code is no longer going to iterate over it anymore, so that it can perhaps do any cleanup tasks (freeing up resources, resetting status, etc)
		- [Do not] put a yield statement inside the finally clause!
		- [Can] use multiple iterators attached to the same generator **concurrently**
		
					-------------------------cleanup:return()-------------------------
					function *foo() {
						try {
							yield 1;
							yield 2;
							yield 3;
						}
						finally {
							console.log( "cleanup!" );
						}
					}
					
					for (var v of foo()) { console.log( v ); }
					// 1 2 3  // cleanup!

					var it = foo();
					it.next();			// { value: 1, done: false }
					it.return( 42 );	// cleanup!  // { value: 42, done: true }
					-------------------------cleanup:throw()-------------------------
					function *foo() { yield 1; yield 2; yield 3; }
					var it = foo(); it.next(); 			// { value: 1, done: false }
					try { it.throw( "Oops!" ); } catch (err) { console.log( err ); } // Exception: Oops!	
					it.next(); // { value: undefined, done: true }

	- Error Handling
		- works in both inbound and outbound directions

					-------------------------2 Directions Error-------------------------
					function *foo() {
						try {yield 1;} catch (err) { console.log( err ); }
						yield 2;
						throw "Hello!";
					}
					var it = foo();
					it.next();	// { value: 1, done: false } 
					try {
						it.throw( "Hi!" ); // Hi!  // { value: 2, done: false }
						it.next();
						console.log( "never gets here" );
					}
					catch (err) {
						console.log( err ); // Hello!
					}
					---------------------propagate in both directions---------------------
					function *foo() {
						try {yield 1;} catch (err) {console.log( err );}
						yield 2;
						throw "foo: e2";
					}
					function *bar() {
						try {
							yield *foo();
							console.log( "never gets here" );
						}
						catch (err) {
							console.log( err );
						}
					}

					var it = bar();
					try {
						it.next();				// { value: 1, done: false }
						it.throw( "e1" ); 		// e1	// { value: 2, done: false }
						it.next(); 				// foo: e2	// { value: undefined, done: true }
					}
					catch (err) {
						console.log( "never gets here" );
					}
					it.next();					// { value: undefined, done: true }


---

> the module pattern drives the vast majority of code.

- ### 3.Modules:

	- The Old Way: An outer function with (**enclosing function and closure**)
		- (1) inner variables
		- (2) inner functions
		- (3) a returned “public API” with methods (closure over the inner data and capabilities)

				---------------------The Old Way---------------------
				function Hello(name) {
					function greeting() {
						console.log( "Hello " + name + "!" );
					}
					// public API
					return {
						greeting: greeting
					};
				}
				var me = Hello( "Ya" );
				me.greeting(); 			// Hello Ya!
				----------------singleton module: IIFE----------------
				var me = (function Hello(name){
					function greeting() {
						console.log( "Hello " + name + "!" );
					}
					// public API
					return {
						greeting: greeting
					};
				})( "Kyle" );
				me.greeting(); 				// Hello Kyle!

	- Moving Forward Conceptual Differences
		-As of ES6 (ES6 static APIs)
			- no longer rely on the enclosing function and closure
			- (1) ES6 modules are file-based -- one module per file
			- (2) no standardized way of combining multiple modules into a single file
			- (3) the advent of HTTP/2 will significantly mitigate any such performance concerns
			- cont. P100 ~ P101

	- The New Way
		- Two Keywords: import & export
			- [Must]Top-level scope: appear outside of all blocks and functions.
		- `export`ing API Members
			- syntx:
				- (1) in front of a declaration
				- (2) as an operator with a special list of bindings to export.

						---------------named exports---------------
						export function foo() {..}		// (1)
						export var awesome = 42;		// (1)
						var bar = [1,2,3];
						export { bar };					// (2)
						-----------Another named exports-----------
						function foo() {..}
						var awesome = 42;
						var bar = [1,2,3];
						export { foo, awesome, bar }; 	// (2)
						--------------------rename-----------------
						function foo() { .. }
						export { foo as bar };

			- exporting **a binding** (like a pointer, reference) to that thing (variable, etc).

						----------------when imported, it is 100 not 42---------------
						var awesome = 42;
						export { awesome };
						// later
						awesome = 100;

			- **Default export**: ES6 prefers a single export
				- the import syntax is more concise if the module has a default export.
				- update module later
					- never plan to update: **export default ..** is fine.
					- plan to update the value: **export { .. as default }**
				- hoisting: 
					- (1) export default function foo(..) {}
					- (2) export default class Foo(..) {}
					- (3) **[Inconsistency] cannot** do export default var foo = ..
						- export var foo = .. 

							// exporting a binding to the function expression value
							---------------default export 1---------------
							function foo(..) {..}
							export default foo;
							---------------default export 2---------------
							// snippet 1 == snippet 2
							// the foo name is bound in the module’s top-level scope (often called “hoisting”).
							export default function foo(..) {}
							---------------default export 3---------------
							function foo(..) {..}
							export { foo as default };

				- **Tempting Structure**
					- [NO]it has some downsides and is officially discouraged.
					- [NO]it cannot do some optimizations for static import performance
					- [YES] having each member individually and explicitly exported 
						- JS engine CAN do the static analysis and optimization.
						
							---------------(discouraged)---------------
							export default {
								foo() { .. },
								bar() { .. },
								..
							};
							---------------(discouraged)---------------
							export default function foo() { .. }
							foo.bar = function() { .. };
							foo.baz = function() { .. };
							------------------(can do)-----------------
							// the ES6 module mechanism  discourage modules with lots of exports
							export default function foo() { .. }
							export function bar() { .. }
							export function baz() { .. }
							------------------(not best)-----------------
							// not mix default export with named exports
							function foo() { .. }
							function bar() { .. }
							function baz() { .. }
							export { foo as default, bar, baz, .. };
							-----(re-export:pass-through untouched)------
							// not mix default export with named exports
							export { foo, bar } from "baz";
							export { foo as FOO, bar as BAR } from "baz";
							export * from "baz";

		- `import`ing API Members
			- These bindings are NOT allowed to be 2-way.
				- (1) import a foo from a module
				- (2) change the value of your imported foo variable
				- (3) an error will be thrown TypeError

					// { .. } like an object literal or object destructuring syntax
					// However, special just for modules
					// foo: (1) goal is statically analyzable syntax
					//		(2) cannot be a variable holding the string value
					import { foo, bar, baz } from "foo";

					--------------------------(rename)-----------------------
					import { foo as theFooFunc } from "foo";
					theFooFunc();

					---------------------(default export)--------------------
					import foo from "foo";
					// or:
					import { default as foo } from "foo";
					
					----(a default export along with other named exports)----
					export default function foo() { .. }
					export function bar() { .. }
					export function baz() { .. }
					// ....
					import FOOFN, { bar, baz as BAZ } from "foo";
					FOOFN(); bar(); BAZ();
					
					-----------------------(namespace import)-----------------
					export function bar() { .. }
					export var x = 42;
					export function baz() { .. }
					// ....
					// [CANNOT] do something like import { bar, x } as foo
					import * as foo from "foo";
					foo.bar();
					foo.x;			// 42
					foo.baz();

					--------(mix default import and namespace import)---------
					// You can additionaly name the default import outside of the namespace binding, as a top-level identifier.
					// syntax is valid, it can be rather confusing
					export default function foo() { .. }
					export function bar() { .. }
					export function baz() { .. }
					//....
					import foofn, * as hello from "world";
					foofn();
					hello.default(); hello.bar(); hello.baz();

					-----------------------(TypeError:immutable/read-only)-----------------
					//Protections:
					//cannot change them from the imported bindings
					import foofn, * as hello from "world";
					foofn = 42; 		// (runtime) TypeError!
					hello.default = 42; // (runtime) TypeError!
					hello.bar = 42;		// (runtime) TypeError!
					hello.baz = 42;		// (runtime) TypeError!

	- Circular Module Dependency
		- A imports B. B imports A. How does this actually work?
		- try to avoid
	- Module Loading
		- Module Loader
			- a separate mechanism
			- provided by the hosting environment (browser, Node.js, etc.)
			- to resolve the module specifier string into some instruction
			- for finding and loading the desired module
			- [Browser] interpret a module specifier as a URL
			- [Node.js] interpret a module specifier as a local file system path
		- Loading Modules Outside Of Modules
			- avoid dynamic loading whenever possible
			- "Reflect.Loader.import(..)" utility == "import * as foo .."

						Reflect.Loader.import( "foo" ) // returns a promise for `"foo"`
						.then( function(foo){
							foo.bar();
						} );
						---------------support a second argument----------------
						Reflect.Loader.import( "foo", { address: "/path/to/foo.js" } )
						.then( function(foo){
							// ..
						} )