---
layout: post
title: ES6 Chapter 3 Organization 2
date: 2016-08-30
weather: sunny
categories: JS
tags: [JS]
description: 
---

# ES6 Chapter 3 Organization 2

> A imports B. B imports A. How does this actually work?

- ### 4.Classes
	- (1) mechanism
		- class Foo implies creating a (special) function of the name Foo
		- constructor(..) identifies the signature of that Foo(..) function
		- use the same “concise method” syntax available to object literals
		- Unlike object literals, NO commas separating members

						=============ES6 Class=============
						class Foo {
							constructor(a,b) {
								this.x = a;
								this.y = b;
							}
							gimmeXY() {
								return this.x * this.y;
							}
						}
						=========pre-ES6 equivalent========
						function Foo(a,b) {
							this.x = a;
							this.y = b;
						}
						Foo.prototype.gimmeXY = function() {
								return this.x * this.y;
						}
						===The class can be instantiated====
						var f = new Foo( 5, 15 );
						f.x; f.y; f.gimmeXY();

	- (2) Differences: Though class Foo is much like function Foo
		- The class Foo(..) call must be made with new
		- function Foo is "hoisted", BUT class Foo is not
		- the "extends" clause specifies an expression that cannot be "hoisted".
		- recommend: instanceof => Symbol.hasInstance 
		- An ES6 class isn’t really an entity itself, but a meta concept
		- can also be an expression
			- var x = class Y { .. }
	- (3) extends and super (syntax sugar)
		- note 1: super is equally effective with plain objects’ concise methods.

						// Bar extends Foo of course means to link the [[Prototype]] of Bar.prototype to Foo.prototype .
						=============ES6 Class=============
						class Bar extends Foo {
							constructor(a,b,c) {
								super( a, b );			// parent constructor
								this.z = c;
							}
							gimmeXYZ() {
								return super.gimmeXY() * this.z;
							}
						}
						var b = new Bar( 5, 15, 25 );
						b.x; b.y; b.z;
						b.gimmeXYZ();
						======note: plain object super=====
						var o1 = {
							foo() { console.log( "o1:foo" ); }
						};
						var o2 = {
							foo() {
								super.foo();
								console.log( "o2:foo" );
							}
						};
						
						Object.setPrototypeOf( o2, o1 );
						o2.foo();

		- note 2: super is statically bound to that specific class heirarchy, and cannot be overriden (at least in ES6).
			- What does that mean?
			- **Two Options**
				- **[1] class , extends , and super will be quite nice**
				- **[2] dropping all attempts to “fake” classes and instead embrace dynamic and flexible classless-objects and [[Prototype]] delegation**
			- [Conclusion] The choice boils down to narrowing your object design to these static hierarchies

			// It means that if you’re in the habit of taking a method from
			// one “class” and “borrowing” it for another class by overriding its this , say with
			// call(..) or apply(..) , that may very well create surprises if the method you’re 
			// borrowing has a super in it.
			=============static class heirarchy===============
			class ParentA {
				constructor() { this.id = "a"; }
				foo() { console.log( "ParentA:", this.id ); }
			}
			class ParentB {
				constructor() { this.id = "b"; }
				foo() { console.log( "ParentB:", this.id ); }
			}
			class ChildA extends ParentA {
				foo() {
					super.foo();
					console.log( "ChildA:", this.id );
				}
			}
			class ChildB extends ParentB {
				foo() {
					super.foo();
					console.log( "ChildB:", this.id );
				}
			}
			var a = new ChildA();
			a.foo();				// ParentA: a ChildA: a
			var b = new ChildB();
			b.foo();				// ParentB: b ChildB: b
			====================borrow======================
			// (1) borrow `b.foo()` to use in `a` context
			// (2) the this.id reference was dynamically rebound [a instead of b]
			// (3) b.foo() ’s super.foo() reference wasn’t dynamically rebound [ParentB instead of the expected ParentA]
			b.foo.call( a ); 		// ParentB: a  ChildB: a

		- note 3: Subclass Constructor
			- Constructors are not required for classes or subclasses
			- A default constructor is substituted in both cases if omitted

			// you could think of the default subclass constructor like this:
			constructor(...args) {
				super(...args);
			}

			- Not all class languages have the subclass constructor automatically call the parent constructor.
				- C++ does
				- Java does not
				- pre-ES6 classes DOES NOT
			- in a constructor of a subclass,**cannot access this until super(..) has been called**
				- it boils down to the fact that the parent constructor is actually the one creating/initializing your instance’s this .

							===============Pre-ES6:access this===================
							function Foo() { this.a = 1; }
							function Bar() {
								this.b = 2;
								Foo.call( this );
							}
							// `Bar` "extends" `Foo`
							Bar.prototype = Object.create( Foo.prototype );
							========[ERROR]ES6 equivalent is not allowed=========
							class Foo {
								constructor() { this.a = 1; }
							}
							
							class Bar extends Foo {
								constructor() {
									this.b = 2; 	// Not allowed before `super()`
									super();		// swap these two statements.
								}
							}

		- note 4: [Extend Benefits]the ability to subclass the built-in natives
			- using manual object creation and linking to Array.prototype only partially worked.
				- such as the automatically updating length property
			- ES6 subclasses should fully work with “inherited” and augmented behaviors as expected!
			
						=============Extends Array==============
						class MyCoolArray extends Array {
							first() { return this[0]; }
							last() { return this[this.length]; }
						}
						var a = new MyCoolArray( 1, 2, 3 );
						a.length;	 // 3
						a;			 // [1,2,3]
						a.first();	 // 1
						a.last(); 	 // 3
						============work well with ERROR========
						// Another common pre-ES6 “subclass” limitation is with the Error object, in creating custom error “subclasses”.
						// in pre-ES6 customized ERROR miss the special stack information
						// as of ES6, it functions well.
						class Oops extends Error {
							constructor(reason) {
								this.oops = reason;
							}
						}
						// later:
						var ouch = new Oops( "I messed up!" );
						throw ouch;
						
	- (4) new.target
		- ES6 introduces a new concept called a "Meta Property"
		- In any constructor, new.target always points at the constructor new directly invoked
		- If new.target is undefined , you know the function was not called with new
		- The "Meta Property" doesn’t have much purpose in class constructors, except accessing a **static property/method**

						============================new.target========================
						class Foo {
							constructor() { console.log( "Foo: ", new.target.name ); }
						}
						class Bar extends Foo {
							constructor() {
								super();
								console.log( "Bar: ", new.target.name );
							}
							baz() {
								console.log( "baz: ", new.target );
							}
						}

						var a = new Foo();		// Foo: Foo
						var b = new Bar();		// Foo: Bar		Bar: Bar
						b.baz();				// baz: undefined

	- (4) static
		- static methods & properties is designed for class; NOT for the function object’s prototype object

					class Foo {
						static answer = 42;
						static cool() { console.log( "cool" ); }
						// ..
					}
					class Bar extends Foo {
						constructor() {
							console.log( new.target.answer );
						}
					}

					Foo.answer;  Bar.answer; // 42  // 42
					var b = new Bar();
					b.cool();
					b.answer;

		- Symbol.species Constructor Getter
			- in setting, Symbol.species getter is useful for a derived (child) class.
			- a derived class can still vend instances of itself using new this.constructor(..) .

			=====================Sample 1=======================
			class MyCoolArray extends Array {
				// force `species` to be parent constructor
				static get [Symbol.species]() { return Array; }
			}
			var a = new MyCoolArray( 1, 2, 3 ),
				b = a.map( function(v){ return v * 2; } );
			b instanceof MyCoolArray; 			// false
			b instanceof Array;					// true
			=====================Sample 2=======================
			class Foo {
				// defer `species` to derived constructor
				static get [Symbol.species]() { return this; }
				spawn() {
					return new this.constructor[Symbol.species]();
				}
			}
			class Bar extends Foo {
				// force `species` to be parent constructor
				static get [Symbol.species]() { return Foo; }
			}
			
			var a = new Foo();
			var b = a.spawn();
			b instanceof Foo; 		// true

			var x = new Bar();
			var y = x.spawn();
			
			y instanceof Bar;		// false
			y instanceof Foo; 		// true