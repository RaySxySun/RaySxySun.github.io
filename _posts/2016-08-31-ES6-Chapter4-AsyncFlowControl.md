---
layout: post
title: ES6 Chapter 4 Async Flow Control
date: 2016-08-31
weather: sunny
categories: JS
tags: [JS]
description:
---

# ES6 Chapter 4 Async Flow Control

> The primary mechanism for managing asynchrony has been the function callback. However, ES6 adds a new feature, Promises. In addition, we will see a pattern, Generators + Promises

- ### 1.Promises
	- (1) What is it?
		- Promises are a trustable system
		- Promises are not about replacing callbacks.
		- Promises provide a trustable intermediary (manage callbacks)
		- A promise is as an event listener (only ever fire once)
			- upon Promises, we can register to listen for an event that lets you know when a task has completed.
		- Promises can be chained together
			- to sequence a series of asychronously completing steps
			- higher-level abstractions: all(..), race(..)
		- A Promise is that it’s a future value
			- the async version of a sync function’s return value.
		- A Promise can only have one of two possible resolution outcomes
			- fulfilled or rejected
		- [Conclusion] Promises provide order, predictability, and trustability.

- ### 2.Making and Using Promises
	- (1) To construct a Promise instance.

				var p = new Promise( function(resolve,reject){ .. });

	- (2) two parameters
		- resolve(..)
			- [fulfilled] with no value, or any non-Promise value
			- [adopts other promise's state]pass another promise
		- reject(..): the promise is rejected

	- (3) use a Promise to refactor a callback-reliant function call
		- a shorthand for calling then(null,handleRejection) which is catch(handleRejection)

						===============callback-reliant function================
						function ajax(url,cb) {
						// make request, eventually call `cb(..)`
						}
						// ..
						ajax( "http://some.url.1", function handler(err,contents){
							if (err) {
							// handle ajax error
							}
							else {
							// handle `contents` success
							}
						} );
						=====================convert it to======================
						function ajax(url) {
							return new Promise( function pr(resolve,reject){
							// make request, eventually call
							// either `resolve(..)` or `reject(..)`
							} );
						}
						// ..
						ajax( "http://some.url.1" )
						.then(
							function fulfilled(contents){
							// handle `contents` success
							},
							function rejected(reason){
							// handle ajax error reason
							}
						);
						================receive the resolution=================
						ajax( "http://some.url.1" )
						.then(
							function fulfilled(contents){
								return contents.toUpperCase();
							},
							function rejected(reason){
								return "DEFAULT VALUE";
							}
						)
						.then( function fulfilled(data){
							// handle data from original promise's
							// handlers
						} );
						=================return a new promise==================
						// If you never observe it by calling
						// a then(..) or catch(..) , then rejection will go unhandled.
						ajax( "http://some.url.1" )
						.then(
						function fulfilled(contents){
							return ajax(
								"http://some.url.2?v=" + contents
							);
						},
						function rejected(reason){
							return ajax(
								"http://backup.url.3?err=" + reason
							);
						}
						)
						.then( function fulfilled(contents){
							// `contents` comes from the subsequent
							// `ajax(..)` call, whichever it was
						} );

- ### 3.Thenables
	-(1) What's it?
		- Any object (or function) with a then(..) function on it is assumed to be a thenable.
		- have a method on it called then(..) can be potentially confused as a thenable

						var th = {
							then: function thener( fulfilled ) {
								// call `fulfilled(..)` once every 100ms forever
								setInterval( fulfilled, 100 );
							}
						};

	- (2) trust concern:
		- if you’re receiving what purports to be a Promise or thenable back from some other system, you shouldn’t just trust it blindly.
		- a utility included with ES6 Promises that helps address this trust concern.

- ### 3.Promise API
	- (1) Promise.resolve(..)
		- creates a promise
	- (2) Promise.reject(..)
		- creates a rejected promise
	- (3) Promise.all([ .. ])
		- accepts an array of one or more values
		- returns a promise back
			- "fulfilled" if all the values fulfill
			- "rejected" immediately once the first of any of them rejects.
			- [in other words] it waits for all fulfillments or the first rejection
	- (4) Promise.race([ .. ])
		- waits only for either the first fulfillment or rejection.

							=================(1)resolve 1===================
							var p1 = Promise.resolve( 42 );
							var p2 = new Promise( function pr(resolve){
								resolve( 42 );
							} );
							=================(1)resolve 2====================
							var theP = ajax( .. );
							var p1 = Promise.resolve( theP );
							var p2 = new Promise( function pr(resolve){
								resolve( theP );
							} );
							=================(2)reject =======================
							var p1 = Promise.reject( "Oops" );
							var p2 = new Promise( function pr(resolve,reject){
								reject( "Oops" );
							} );
							====================(3)all =======================
							var p1 = Promise.resolve( 42 );
							var p2 = new Promise( function pr(resolve){
								setTimeout( function(){
									resolve( 43 );
								}, 100 );
							} );
							var v3 = 44;
							var p4 = new Promise( function pr(resolve,reject){
								setTimeout( function(){
									reject( "Oops" );
								}, 10 );
							} );

							----------------------fulfilled---------------------
							Promise.all( [p1,p2,v3] )
							.then( function fulfilled(vals){
								console.log( vals );				// [42,43,44]
							} );
							----------------------rejected----------------------
							Promise.all( [p1,p2,v3,p4] )
							.then(
								function fulfilled(vals){
									// never gets here
								},
								function rejected(reason){
									console.log( reason );			// Oops
								}
							);
							====================(3)race=======================
							Promise.race( [p2,p1,v3] )
							.then( function fulfilled(val){
								console.log( val );					// 42
							} );

							Promise.race( [p2,p1,v3,p4] )
							.then(
								function fulfilled(val){
									// never gets here
								},
								function rejected(reason){
									console.log( reason );			// Oops
								}
							);

- ### 4.Generators + Promises
	- (1) The Aim is to:
		- express a series of promises in a chain (the async flow control)
	- (2) generators to express the async flow control
		- be much more preferable in terms of coding style than long promise chains
		- synchronous-looking coding style

					=================(1)Long promise chains==================
					step1()
					.then(
						step2,
						step2Failed
					)
					.then(
						function(msg) {
							return Promise.all( [
								step3a( msg ),
								step3b( msg ),
								step3c( msg )
							] )
						}
					)
					.then(step4);
					======================(1)preferable=====================
					function *main() {
						var ret = yield step1();
						try {
							ret = yield step2( ret );
						}
						catch (err) {
							ret = yield step2Failed( err );
						}

						ret = yield Promise.all([ step3a( ret ), step3b( ret ), step3c( ret ) ]);
						yield step4( ret );
					}
					-------------------Need A Runner 1------------------------
					Q.spawn(..)
					-------------------Need A Runner 2------------------------
					function run(gen) {
						var args = [].slice.call( arguments, 1), it;
						it = gen.apply( this, args );

						return Promise.resolve()
							.then( function handleNext(value){
								var next = it.next( value );
								return (function handleResult(next){
									if (next.done) {
										return next.value;
									}
									else {
									return Promise.resolve( next.value )
									.then(
										handleNext,
										function handleErr(err) {
											return Promise.resolve(
												it.throw( err )
											)
											.then( handleResult );
										}
									);
								}
							})( next );
						} );
}
