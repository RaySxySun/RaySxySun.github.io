---
layout: post
title: JS Design Pattern - Object Creation Patterns (Chapter 5)
date: 2016-08-03
weather: cloudy
categories: JS
tags: [JS]
description:
---

# Object Creation Patterns

> Creating objects in JavaScript is easy： (1) object literal OR (2) constructor functions

> [No syntax] The JavaScript language is simple and straightforward; No syntax for such as namespaces, modules, packages, private properties, and static members.

> This topic about (1) **Patterns: namespacing, dependency declaration, module pattern, and sandbox and (2) private and privileged members, static and private static members, object constants, chaining**

- ### 1.Namespace Pattern
    - reduce the number of globals
        - avoid naming collisions
        - excessive name prefixing

                //Warning: antipattern

                // BEFORE: 5 globals
                // constructors
                function Parent() {}
                function Child() {}
                // a variable
                var some_var = 1;
                // some objects
                var module1 = {};
                module1.data = {a: 1, b: 2};
                var module2 = {};

                ===================================

                //Simple Solution
                // AFTER: 1 global
                // global object
                var MYAPP = {};
                // constructors
                MYAPP.Parent = function () {};
                MYAPP.Child = function () {};
                // a variable
                MYAPP.some_var = 1;
                // an object container
                MYAPP.modules = {};
                // nested objects
                MYAPP.modules.module1 = {};
                MYAPP.modules.module1.data = {a: 1, b: 2};
                MYAPP.modules.module2 = {};

                ===================================

                //Improve
                // unsafe
                var MYAPP = {};
                // better
                if (typeof MYAPP === "undefined") {
                var MYAPP = {};
                }
                // or shorter
                var MYAPP = MYAPP || {};

                ===================================

                //create nondestructive function namespace()

                var MYAPP = MYAPP || {};

                MYAPP.namespace = function (ns_string) {
                    var parts = ns_string.split('.'),
                    parent = MYAPP, i;

                    // strip redundant leading global
                    if (parts[0] === "MYAPP") {
                        parts = parts.slice(1);
                    }

                    for (i = 0; i < parts.length; i += 1) {
                        // create a property if it doesn't exist
                        if (typeof parent[parts[i]] === "undefined") {
                            parent[parts[i]] = {};
                        }
                        parent = parent[parts[i]];
                    }
                    return parent;
                }

                ------------------------------------

                /*
                 * Call the function namespace()
                 */

                //it will make three checks
                MYAPP.namespace('MYAPP.modules.module2');

                // assign returned value to a local var
                var module2 = MYAPP.namespace('MYAPP.modules.module2');
                module2 === MYAPP.modules.module2; // true

                // skip initial `MYAPP`
                MYAPP.namespace('modules.module51');

                // long namespace
                MYAPP.namespace('once.upon.a.time.there.was.this.long.nested.property');

---

- ### 2.Declaring Dependencies (extremely simple pattern)
    - notice the users of your code; users need to make sure the specific files included
    - Upfront declaration makes it easy to find and resolve dependencies.
    - Working with a local variable (such as dom ) is always faster. resolution is performed only once in the function.
    - Advanced minification tools will rename local variables(Google Closure compiler can generate concise and succinct code)

                var myFunction = function () {
                    // dependencies
                    var event = YAHOO.util.Event,
                    dom = YAHOO.util.Dom;

                    // use event and dom variables
                    // for the rest of the function...
                };

                ===================================

                function test1() {
                    alert(MYAPP.modules.m1);
                    alert(MYAPP.modules.m2);
                    alert(MYAPP.modules.m51);
                }
                /*
                minified test1 body:
                alert(MYAPP.modules.m1);alert(MYAPP.modules.m2);alert(MYAPP.modules.m51)
                */

                -----------------------------------

                function test2() {
                    var modules = MYAPP.modules;
                    alert(modules.m1);
                    alert(modules.m2);
                    alert(modules.m51);
                }
                /*
                minified test2 body:

                var a=MYAPP.modules;alert(a.m1);alert(a.m2);alert(a.m51)
                */

---

- ### 3.Private Properties and Methods
    - (1) Public:
        - All object members are public
        - no special syntax to denote private, protected, or public properties and methods

                    //object literal
                    var myobj = {
                        myprop: 1,
                        getProp: function () {
                            return this.myprop;
                        }
                    };
                    console.log(myobj.myprop); // `myprop` is publicly accessible
                    console.log(myobj.getProp()); // getProp() is public too

                    ---------------------------------------------------------

                    //object constructor

                    function Gadget() {
                        this.name = 'iPod';
                        this.stretch = function () {
                            return 'iPad';
                        };
                    }
                    var toy = new Gadget();
                    console.log(toy.name); // `name` is public
                    console.log(toy.stretch()); // stretch() is public
    - (2) Private members
        - NO special syntax
        - using a closure

                    function Gadget() {
                        // private member
                        var name = 'iPod';

                        // public function
                        this.getName = function () {
                            return name;
                        };
                    }

                    var toy = new Gadget();

                    // `name` is undefined, it's private
                    console.log(toy.name); // undefined
                    // public method has access to `name`
                    console.log(toy.getName()); // "iPod"

    - (3) Privileged Methods
        - In the previous example, getName() is a privileged method because it has "special" access to the private property name .

    - (4) Privacy Failures
        - edge cases
            - [eval] Earlier versions of Firefox enable a 2nd parameter to be passed to eval() -> neak into the private scope of the function
            - [__parent__] Similarly, Mozilla Rhino includes __parent__ property -> access to private scope
            - [object or array] Returning a private variable from a privileged method (an object or array)

                            // it looks innocent
                            function Gadget() {
                                // private member
                                var specs = {
                                    screen_width: 320,
                                    screen_height: 480,
                                    color: "white"
                                };

                                // public function
                                this.getSpecs = function () {
                                    return specs;
                                };
                            }
                            -----------------------------------------------------
                            // getSpecs() returns a reference to the specs object
                            var toy = new Gadget(),
                                specs = toy.getSpecs();
                            specs.color = "black";
                            specs.price = "free";
                            console.dir(toy.getSpecs());

                            /*
                             *color: "black"
                             *price: "free"
                             *screen_height: 480
                             *screen_width: 320
                             */
                             // [return a new object] containing only some of the data that could be interesting to the consumer of the object. instead of giving out everything, to create getDimensions() containing only width and height
                             // [object-cloning function] shallow copy extend() VS. extendDeep()

    > Principle of Least Authority: never give more than needed

    - (5) Object Literals and Privacy
        - all we need is a function to wrap the private data. [the bare bones of module pattern]

                  var myobj; // this will be the object
                  (function () {
                      // private members
                      var name = "my, oh my";
                      // implement the public part
                      // note -- no `var`
                      myobj = {
                      // privileged method
                          getName: function () {
                          return name;
                          }
                      };
                  }());
                  myobj.getName(); // "my, oh my"

                  ---------------------------------------
                  // The same idea: module pattern

                  var myobj = (function () {
                      // private members
                      var name = "my, oh my";
                      // implement the public part
                      return {
                          getName: function () {
                          return name;
                          }
                      };
                  }());
                  myobj.getName(); // "my, oh my"

    - (6) Prototypes and Privacy
        - [Drawback]One drawback of the private members: they are recreated every time the constructor is invoked to create a new object.
        - [Solution]add common properties and methods to the **prototype property** of the constructor (share the hidden private members)

                  //create private member 'name' every time
                  function Gadget() {
                      // private member
                      var name = 'iPod';
                      // public function
                      this.getName = function () {
                          return name;
                      };
                  }

                  ------------------------------------------

                  //share the private member browser
                  Gadget.prototype = (function () {
                      // private member
                      var browser = "Mobile Webkit";
                      // public prototype members
                      return {
                          getBrowser: function () {
                              return browser;
                          }
                      };
                  }());

                  var toy = new Gadget();
                  console.log(toy.getName()); // privileged "own" method
                  console.log(toy.getBrowser()); // privileged prototype method

    - (7) Revealing Private Functions As Public Methods (Revelation Pattern)
        - When you expose methods publicly, you make them vulnerable
        - In ECMAScript 5 you have the option to freeze an object

                        // two private variables and two private functions

                        var myarray;
                        (function () {
                            var astr = "[object Array]",
                                toString = Object.prototype.toString;

                            function isArray(a) {
                                return toString.call(a) === astr;
                            }

                            function indexOf(haystack, needle) {
                                var i = 0,
                                max = haystack.length;
                                for (; i < max; i += 1) {
                                    if (haystack[i] === needle) {
                                        return i;
                                    }
                                }
                                return −1;
                            }

                            // Toward the end of the immediate function
                            // you decide is appropriate to make publicly available
                            myarray = {
                                isArray: isArray,
                                indexOf: indexOf,
                                inArray: indexOf
                            };
                        }());

                        myarray.isArray([1,2]); // true
                        myarray.isArray({0: 1}); // false
                        myarray.indexOf(["a", "b", "z"], "z"); // 2
                        myarray.inArray(["a", "b", "z"], "z"); // 2

                        //if something unexpected happens to the public indexOf()
                        //the private indexOf() is still safe

                        myarray.indexOf = null;
                        myarray.inArray(["a", "b", "z"], "z"); // 2

---

- ### 4.Module Pattern (highly recommended)
    - provides structure and helps organize your code
    - doesn’t have special syntax for packages
    - The module pattern is a combination of several patterns:
        - (1)Namespaces

                        MYAPP.namespace('MYAPP.utilities.array');

        - (2)Immediate functions

                        MYAPP.utilities.array = (function () {
                            return {
                                // todo...
                            };
                        }());

                        --------------------------------------
                        // public interface

                        MYAPP.utilities.array = (function () {
                            return {
                                inArray: function (needle, haystack) {
                                    // ...
                                },
                                isArray: function (a) {
                                    // ...
                                }
                            };
                        }());

        - (3)Private and privileged members
        - (4)Declaring dependencies

                MYAPP.namespace('MYAPP.utilities.array');
                MYAPP.utilities.array = (function () {
                    // dependencies
                    var uobj = MYAPP.utilities.object,
                        ulang = MYAPP.utilities.lang,

                        // private properties
                        array_string = "[object Array]",
                        ops = Object.prototype.toString;

                        // private methods
                        // ...

                        // end var

                    // optionally one-time init procedures
                    // ...
                    // public API
                    return {
                        inArray: function (needle, haystack) {
                            for (var i = 0, max = haystack.length; i < max; i += 1) {
                                if (haystack[i] === needle) {
                                    return true;
                                }
                            }
                        },

                        isArray: function (a) {
                            return ops.call(a) === array_string;
                        }

                        // ... more methods and properties
                    };

                }());

---

- ### 5.Revealing Module Pattern (highly recommended)
    - all the methods are kept private
    - only expose those that you decide at the end

MYAPP.utilities.array = (function () {
        // private properties
    var array_string = "[object Array]",
        ops = Object.prototype.toString,

        // private methods
        inArray = function (haystack, needle) {
            for (var i = 0, max = haystack.length; i < max; i += 1) {
                if (haystack[i] === needle) {
                    return i;
                }
            }
            return −1;
        },

        isArray = function (a) {
            return ops.call(a) === array_string;
        };
        // end var

    // revealing public API
    return {
        isArray: isArray,
        indexOf: inArray
    };
}());

---

- ### 6.Modules That Create Constructors
    - MYAPP.utilities.array
