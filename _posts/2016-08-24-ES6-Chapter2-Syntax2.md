---
layout: post
title: ES6 Chapter 2 Syntax 2
date: 2016-08-24
weather: cloudy
categories: JS
tags: [JS]
description: 
---

# ES6 Chapter 2 Syntax 2

- ### 5.Object Literal Extensions
	- [New Feature] Concise Properties
	
						--------old--------
						var x = 2, y = 3,
						o = {x: x, y: y};
						--------new--------
						var x = 2, y = 3,
						o = {x, y};

	- [New Feature] Concise Methods
		- [Advantage] Concise methods are short and sweet, and nice convenience

						--------old--------
						var o = {
							x: function() {..},
							y: function() {..}
						}
						--------new--------
						var o = {
							x() {..},
							y() {..}
						}

		- [Disadvantage] Concisely Unnamed
			- Concise Methods look closely. Do you see the problem?
				- The concise method definition implies some thing: function(x,y) .
				- Concise methods are short and sweet, and nice convenience
				- [Do Not] use them to do recursion event binding/unbinding)
			- [Solution] thing: function something(..)
							
							runSomething( {
								something(x,y) {
									if (x > y) {
										//  equals to [ something: function(x,y) ]
										return something( y, x ); // ERROR will not find a something identifier
									}
									return y - x;
								}
							} );

		- [Not New Feature] ES5 Getter/Setter

							var o = {
								__id: 10,
								get id() { return this.__id++; },
								set id(v) { this.__id = v; }
							}
							console.log(o.id) // 10
							console.log(o.id) // 11
							console.log(o.id) // 12

							console.log(o.__id) // 12 -- still!

	- [New Feature] Computed Property Names
		- allows you to specify an expression, whose result is a property name

								var prefix = "user_";
									var o = {
									baz: function(..) { .. },
									[ prefix + "foo" ]: function(..) { .. },
									[ prefix + "bar" ]: function(..) { .. }
									..
								};
			
		- a concise method or a concise generator:

								var o = {
									["f" + "oo"]() { .. } // computed concise method
									*["b" + "ar"]() { .. } // computed concise generator
								};

	- [New Feature] Setting [[Prototype]]
		- ES6 utility Static Function Object.setPrototypeOf(..)
		- __proto__ is controversial

								var o1 = {..};
								var o2 = {..};
								Object.setPrototypeOf( o2, o1 );

	- [New Feature]Object super
		- classless-objects-with-prototypes
		- super is only allowed in concise methods

						var o1 = {
							foo() {
								console.log( "o1:foo" );
							}
						};
						var o2 = {
							foo() {
								// The super reference
								// be Object.getPrototypeOf(o2) -- o1.foo()
								super.foo();
								console.log( "o2:foo" );
							}
						};
						
						Object.setPrototypeOf( o2, o1 );
						o2.foo(); // o1:foo o2:foo


---

- ### 6.Template Literals (Interpolated String Literals)
		- string interpolation expressions (`Hello ${name}!`)
		- like an IIFE, automatically evaluated inline.

						var name = "Kyle";
						var greeting = `Hello ${name}!`;
						console.log( greeting ); 		// "Hello Kyle!"
						console.log( typeof greeting ); // "string"

						// allowed to split across multiple lines
						var text =
						`the first line
						 the sencond line
						 the third line!`;

						console.log( text );
						// Now is the time for all good men
						// to come to the aid of their
						// country!

	- [New Feature]Interpolated Expressions:

						function upper(s) {
							return s.toUpperCase();
						}
						
						var who = "reader"
						
						var text =
						`A very ${upper( "warm" )} welcome
						to all of you ${upper( `${who}s` )}!`;
						
						console.log( text );
						-------------------Expression Scope---------------------
						// Neither the global name nor foo(..) ’s name matter.

						function foo(str) {
							var name = "foo";
							console.log( str );
						}

						function bar() {
							var name = "bar";
							foo( `Hello from ${name}!` );
						}

						var name = "global";
						bar(); // "Hello from bar!"

	- [New Feature]Interpolated Expressions:
		- It’s essentially a special kind of function call that doesn’t need the ( .. )
		- The tag is a function name
		- The tag can be any expression that results in a function

						function foo(strings, ...values) {
							console.log( strings );
							console.log( values );
						}

						var desc = "awesome";
						foo`Everything is ${desc}!`;
						// [ "Everything is ", "!"]
						// [ "awesome" ]
						-------any expression that results in a function-------
						// The tag can be any expression that results in a function

						function bar() {
							return function foo(strings, ...values) {
								console.log( strings );
								console.log( values );
							}
						}

						var desc = "awesome";
						bar()`Everything is ${desc}!`;
						// [ "Everything is ", "!"]
						// [ "awesome" ]

		- [Example] Practical uses

						function dollabillsyall(strings, ...values) {
							return strings.reduce( function(s,v,idx){
								if (idx > 0) {
									if (typeof values[idx-1] == "number") {
										// look, also using interpolated
										// string literals!
										s += `$${values[idx-1].toFixed( 2 )}`;
									}
									else {
										s += values[idx-1];
									}
								}

								return s + v;
							}, "" );
						}
						var amt1 = 11.99, amt2 = amt1 * 1.08, name = "Kyle";
						var text = dollabillsyall
						`Thanks for your purchase, ${name}! Your
						product cost was ${amt1}, which with tax
						comes out to ${amt2}.`

						console.log( text );
						// Thanks for your purchase, Kyle! Your
						// product cost was $11.99, which with tax
						// comes out to $12.95.

		- Raw Strings

						function showraw(strings, ...values) {
							console.log( strings );
							console.log( strings.raw );
						}
						showraw`Hello\nWorld`;
						// [ "Hello\nWorld" ]
						// [ "Hello\\nWorld" ]

---

> It’s important to understand the frustrations that this based programming with normal functions brings

- ### 7.Arrow Functions
	- [anonymous function expressions] no named reference for recursion or event binding/unbinding

						var f1 = () => 12;
						var f2 = x => x * 2; // like: a = a.map( v => v * 2 );
						var f3 = (x,y) => {
							var z = x * 2 + y;
							y++;
							x *= 3;
							return (x + y + z) / 2;
						};

	
	- short single statement utilities
		- arrow functions is an attractive and lightweight alternative to the more verbose function keyword and syntax.
		- [Inversely proportional]readability gains: The longer the function, the less => helps; the shorter the function, the more => can shine.

								var a = [1,2,3,4,5];
								a = a.map( v => v * 2 );
								console.log( a ); // [2,4,6,8,10]

		- [sensible and reasonable] adopt => to replace short inline function expressions

	
	- [New Feature] \=> is syntactic stand-in for "var self = this" or "bind(this)"

						var controller = {
							makeRequest: function() {
								var body = document.getElementById("body");
								body.addEventListener( "click", () => {this.helper();}, false );
							},
							helper: function(){
								alert('test');
							}
						};
						controller.makeRequest(); 

		- **[Not]** quite so simple: use => with a this -aware function

						var controller = {
							makeRequest: (..) => {
								this.helper(..);
							},
							helper: (..) => {
								
							}
						};
						// global scope: this points to the global object.
						controller.makeRequest(..);

	- [Conclusion] 
		- using Arrow function when
			- 1.short, single-statement inline function expression returns some computed value, doesn’t already make a this reference inside it, no self-reference(recursion, event binding/unbinding)
			- 2.inner function expression which relying on a "var self = this" or ".bind(this)", call on it in the enclosing function.
			- 3.make a lexical copy of arguments:like an inner function expression that’s relying on "var args = Array.prototype.slice.call(arguments)" 
		- Avoid Arrow function syntax.
			- For everything else — normal function declarations, longer multi-statment function expressions, functions which need a lexical name identifier self-reference(recursion, etc.), and any other function which doesn’t fit the previous characteristics — you should probably avoid \=> function syntax.

						// Array-like objects / collections 
						// The arguments inside a function is an example of an 'array-like object'.
						var a={length:2,0:'first',1:'second'};
						Array.prototype.slice.call(a);//  ["first", "second"]

						function list() {
						  return Array.prototype.slice.call(arguments);
						}
						var list1 = list(1, 2, 3); // [1, 2, 3]


---

- ### 8.for..of Loops
	- [New Feature] for..of Loops
		- 1.for..of VS. for..in
			- for..of loops over the values
			- for..in loops over the keys/indexes

						var a = ["a","b","c","d","e"];
						for (var idx in a) {
							console.log( idx );		// 0 1 2 3 4
						}
						for (var val of a) {
							console.log( val );		// "a" "b" "c" "d" "e"
						}
						---------manually iterating an iterator--------

						var a = ["a","b","c","d","e"];
						for (var val, ret, it = a[Symbol.iterator](); !(ret = it.next()) && !ret.done; ) 
						{
							val = ret.value;
							console.log( val ); // "a" "b" "c" "d" "e"
						}

		- 2.Standard built-in values include default iterables
			- arrays, strings, generators, collections / TypedArrays

						for (var c of "hello") {
							console.log( c ); 			// "h" "e" "l" "l" "o"
						}
						------------------------------------------------------
						var o = {};
						for (o.a of [1,2,3]) {
							console.log( o.a );			// 1 2 3
						}
						for ({x: o.a} of [ {x: 1}, {x: 2}, {x: 3} ]) {
							console.log( o.a );			// 1 2 3
						}

		- 3.for..of loops can be prematurely stopped
			- break , continue , return (if in a function), and thrown exceptions

---

- ### 9.Regular Expressions
	- 1.Unicode Flag (//u)
		- as of ES6, the u flag tells a regular expression to process a string with the intepretation of Unicode (UTF-16) characters
		- JavaScript strings are typically interpreted as sequences of 16-bit characters
		- correspond to Basic Multilingual Plane [BMP](http://en.wikipedia.org/wiki/Plane_%28Unicode%29)
				
					"UTF-16" doesn’t strictly mean 16 bits. Modern Unicode uses 21 bits,

		- [Example] the musical symbol G-clef(U+1D11E)
			- 𝄞 (G-clef)
			- two separate characters (0xD834 and 0xDD1E)

						/𝄞/.test( "𝄞-clef" ); 			// true
						/^.-clef/ .test( "𝄞-clef" ); 	// false: the match fails (2 charac‐ters)
						/^.-clef/u.test( "𝄞-clef" ); 	// true

	- 2.Sticky Flag (//y)
		- a virtual anchor

						var re1 = /foo/,
						str = "++foo++";
						re1.lastIndex;		// 0 
						re1.test( str ); 	// true
						re1.lastIndex; 		// 0 -- not updated

						re1.lastIndex = 4;
						re1.test( str ); 	// true -- ignored `lastIndex`
						re1.lastIndex; 		// 4 -- not updated

		- Three things: 
			- (1) test(..) ignore the lastIndex ’s value, performs its match from the beginning;
			- (2) does not have a ^ start-of-input anchor, free to move ahead to look for a match.
			- (3) lastIndex is not updated by test(..) 
	- 2.Regular Expression flags
		- Ironically, Prior to ES6, see what flags the regular expression had applied

						var re = /foo/ig;
						re.toString(); 		// "/foo/ig"
						var flags = re.toString().match( /\/([gim]*)$/ )[1];
						flags; 				// "ig"

		- As of ES6,

						var re = /foo/ig;
						re1.source;	// "foo"
						re.flags; 	// "gi"

---

- ### 10.Number Literal Extensions
	- Here are the new ES6 number literal forms:

						var dec = 42,
							oct = 0o52, 	// or '0O52'
							hex = 0x2a,		// or '0X2a'
							bin = 0b101010; // or '0B101010'

						var a = 42;
						a.toString();		// "42" -- also `a.toString( 10 )`
						a.toString( 8 );	// "52"
						a.toString( 16 );	// "2a"
						a.toString( 2 );	// "101010"

---

- ### 11.Unicode
	- Basic Multilingual Plane (BMP): The Unicode characters that range from 0x0000 to 0xFFFF
	- Extended Unicode characters: range up to 0x10FFFF
	- prior to ES6, using a surrogate pair

						//surrogate pair
						var gclef = "\uD834\uDD1E";
						console.log( gclef );  // " 𝄞 "

	- As of ES6, Unicode code point escaping

						var gclef = "\u{1D11E}";
						console.log( gclef ); 	// " 𝄞 "

	- Unicode-Aware String Operations

						var snowman = "☃";
						snowman.length; // 1
						var gclef = "𝄞";
						gclef.length; // 2

						var gclef = "𝄞";
						[...gclef].length;	// 1
						Array.from( gclef ).length; // 1
						gclef.normalize().length; // 1

	- Character Positioning

						// prior to ES6
						s1.charAt(2)

						// as of ES6
						[...s1.normalize()][2];
						s1.normalize().codePointAt( 2 ).toString( 16 );
						String.fromCodePoint( 0x107 ); // "ć"
						String.fromCodePoint( s1.normalize().codePointAt( 2 ) );

	- Unicode Identifier Names
		- rare and academic					

						var \u03A9 = 42; 		// same as: var Ω = 42;

---

- ### 11.Symbols (unique string value)
	- Don’t have a literal form		
	- create a symbol:
		- not use new with Symbol(..)
		- The parameter passed to Symbol(..) is optional
		- The typeof output is a new value ( "symbol" ) that is the primary way to identify a symbol.

						var sym = Symbol( "some optional description" );
						typeof sym; 	// "symbol"

		- construct a boxed wrapper object form of a symbol value
		
						sym instanceof Symbol;		// false
						var symObj = Object( sym );
						symObj instanceof Symbol; 	// true
						symObj.valueOf() === sym;	// true		

	- think of this symbol value as an automatically generated, unique (within your application) string value	
		- [Q] if the value is hidden and unobtainable, what’s the point of having a symbol at all?
			- (1)to create a string-like value that can’t collide with any other value		

							// EVT_LOGIN holds a value that cannot be duplicated
							const EVT_LOGIN = Symbol( "event.login" );			

			- (2)to create a special property that is hidden or meta in usage 
				- **A typical usage: Singleton Pattern**

						---------------------Singleton-----------------------
						const INSTANCE = Symbol( "instance" );
						function HappyFace() {
							
							if (HappyFace[INSTANCE]) return HappyFace[INSTANCE];
							
							function smile() { .. }
							
							return HappyFace[INSTANCE] = {
								smile: smile
							};
						}
						var me = HappyFace(),
							you = HappyFace();
						me === you;

	- Symbol Registry
		- create symbol values with the global symbol registry

						const EVT_LOGIN = Symbol.for( "event.login" );

						---------------------Singleton-----------------------

						function HappyFace() {
							const INSTANCE = Symbol.for( "instance" );
							if (HappyFace[INSTANCE]) return HappyFace[INSTANCE];
							// ..
							return HappyFace[INSTANCE] = { .. };
						}

	- Symbols as Object Properties
		- a property symbol is not actually hidden or inaccessible 
		- Object.getOwnPropertySymbols(..)
	
						var o = {
							foo: 42,
							[ Symbol( "bar" ) ]: "hello world",
							baz: true
						};		

						Object.getOwnPropertyNames( o );    	// [ "foo","baz" ]			
						Object.getOwnPropertySymbols( o );		// [ Symbol(bar) ]

						-------------------Built-in Symbols-------------------------
						var a = [1,2,3];
						a[Symbol.iterator]; 	// native function

