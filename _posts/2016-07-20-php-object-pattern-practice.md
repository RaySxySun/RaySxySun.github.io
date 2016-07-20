---
layout: post
title: PHP object, pattern, practice
date: 2016-07-20
weather: rainy
categories: php
tags: [php]
description:
---

# Chapter 4: Object Advanced Features

- Static Methods and Properties
- Constant Properties 
- Abstract Classes(extends)
	- [Error]PHP Fatal error: Cannot instantiate abstract class ShopProductWriter in...
	- [Error]PHP Fatal error: Class ErroredWriter contains 1 abstract method and
- Interfaces (implements)
- Traits (use)
	- Using More than One Trait
	- Managing Method Name Conflicts with insteadof
		- [Error]Fatal error: Trait method calculateTax has not been applied
		- use PriceUtilities, TaxTools {TaxTools::calculateTax insteadof PriceUtilities;}
	- Defining Abstract Methods in Traits
		- it takes on the commitment to implement any abstract methods it declares
	- Changing Access Rights to Trait Methods (p53)
		-  as operator can be used to alias a method  name
		-  use an access modifier on the right-hand side of this operator, it will change the method’s access level
			- PriceUtilities::calculateTax as private;
- Late Static Bindings: The static Keyword (p55)
	- (1)static. static is similar to self, except that it refers to the invoked rather than the containing class.
		- return new static();
	- (2) The **static** keyword can be used for more than just instantiation Like **self** and **parent**
		$this->group = static::getGroup(); (p56)

- Handling Errors
	- Throwing an Exception, try/catch Clauses, with finally
		- throw new Exception( "file '$file' does not exist" );
- Final Classes and Methods
	- final puts a stop to inheritance. A final class cannot be subclassed. Less drastically, a final method cannot be overridden.
	- [Error] PHP Fatal error: Class IllegalCheckout may not inherit from final class (Checkout) in
	- [Error] Fatal error: Cannot override final method Checkout::totalize() in ...

- Working with Interceptors

Method| Description
---|---
__get( $property ) | Invoked when an undefined property is accessed
__set( $property, $value ) |Invoked when a value is assigned to an undefined property
__isset( $property ) |Invoked when isset() is called on an undefined property
__unset( $property ) |Invoked when unset() is called on an undefined property
__call( $method, $arg_array ) |Invoked when an undefined non-static method is called
__callStatic( $method, $arg_array ) |Invoked when an undefined static method is called

- Defining Destructor Methods
	- function \_\_destruct() 
- Copying Objects with \_\_clone()
	- (1)clone operates on an object instance, producing a by-value copy
		- $second = clone $first;
		- \_\_clone() is run on the copied object and not the original; overwrites the default copy
- Defining String Values for Your Objects
	- [Error] PHP Catchable fatal error: Object of class StringThing could not be converted to string in 
	- By implementing a \_\_toString() method
- Callbacks, Anonymous Functions and Closures
	-  is_callable(), call_user_func(), array_walk()
	- [ugly]$logger = create_function( '$product', 'print " logging ({$product->name})\n";' );
	- [PHP 5.3] $logger2 = function( $product ) { print " logging ({$product->name})\n"; }
	- [return anonymous fun] return function( $product ) use ( $amt, &$count ){...}   (p79)
	

# Chapter 5: Object Tools

- PHP and Packages
	- avoid naming collisions [p82]
		- [Error]PHP Fatal error: Cannot redeclare class Outputter in ...Outputter1.php on line 2
		- [problem] as projects got more involved in class name, class names grew longer and longer. 
	- Namespaces to the Rescue [PHP 5.3]
		- namespace com\getinstance\util
			- [Error] PHP Fatal error: Class 'main\com\getinstance\util\Debug' not found in...
			- [Solution]Search Gloable: That leading backslash tells PHP to begin its search at the root
			- \com\getinstance\util\Debug::helloWorld();
		- implicitly aliased
			- use com\getinstance\util;
			- util\Debug::helloWorld();
			- [Notice] that I didn’t begin with a leading backslash character
		- name collisions [alias explicit]		
			- [Error] PHP Fatal error: Cannot declare class main\Debug because the name is already in use in...
			- [Solution]alias explicit: use com\getinstance\util\Debug as uDebug;
		- \_\_NAMESPACE\_\_
		- [Alternative Format]namespace com\getinstance\util {}
- The Class and Object Functions
	- Looking for Classes
		- class_exists()
			- $path = "tasks/{$classname}.php"
			- if ( ! file_exists( $path ) ) {...}
	- Learning About an Object or Class
		- get_class()
		- if ( get_class( $product ) === 'CdProduct' ) {...}	
	- Getting a Fully Qualified String Reference to a Class 
		- ClassName::class
		- a scope resolution operator and the class keyword to get the fully qualified class name
	- Learning About Methods
		- get_class_methods()
			- [Useful Case]if ( in_array( $method, get_class_methods( $product ) ) )
			- then invoke $product->$method()
		- is_callable()
			- if ( is_callable( array( $product, $method) ) ) 
		- method_exists()
			- if ( method_exists( $product, $method ) )
			- [Caution] remember that the fact that a method exists does not mean that it will be callable
			- [Caution] returns true for private and protected methods as well as for public ones.
	- Learning About Properties
		- get_class_vars( 'CdProduct' )
			- print_r( get_class_vars( 'CdProduct' ) );
	- Learning About Inheritance
		- get_parent_class()
			- if ( is_subclass_of( $product, 'ShopProduct' ) )
		- is_subclass_of() 
			- if ( in_array( 'someInterface', class_implements( $product )) )
	- Method Invocation
		- [1]$product->$method();
		- [2]call_user_func()
			- $returnVal = call_user_func( array( $myObj, "methodName") )
			- call_user_func( array( $product, 'setDiscount' ), 20 );
				- $product->setDiscount( 20 );
			- call_user_func_array()
- The Reflection API
	
Class | Description
---|---
Reflection | Provides a static export() method for summarizing class information
ReflectionClass | Class information and tools
ReflectionMethod | Class method information and tools
ReflectionParameter | Method argument information
ReflectionProperty | Class property information
ReflectionFunction |Function information and tools
ReflectionExtension |PHP extension information
ReflectionException |An error class
ReflectionZendExtension |PHP Zend extension information

  - Time to Roll Up Your Sleeves(p99)
	- Reflection::export();
		- $prod_class = new ReflectionClass( 'CdProduct' );
		- Reflection::export( $prod_class );
		- [Notice] var_dump():  it provides nothing like the detail made available by Reflection::export()
		- [Notice] print_r(): var_dump's cousin
	- ReflectionClass::getName()
		- $class->getName()
	- ReflectionClass::isUserDefined()
		- $class->isUserDefined()
	- ReflectionClass::isInternal()
		- $class->isInternal()
	- ReflectionClass::isAbstract()
		- $class->isAbstract()
	- ReflectionClass::isInterface()
		- $class->isInterface()
	- ReflectionClass::isInstantiable()
		- $class->isInstantiable()
	- ReflectionClass::getFileName()
		- $lines = @file( $class->getFileName() );
	- $class->getStartLine();	
  - Examining Methods (p103)
	-  ReflectionClass::getMethods()
		- $prod_class = new ReflectionClass( 'CdProduct' );
		- $methods = $prod_class->getMethods();
	- ReflectionClass::
		- getName(), isInternal(), isAbstract(), isPublic(), isProtected(), isPrivate(), isStatic9(), isFinal(), isConstructor(), returnsReference()
	- **ReflectionMethod::invoke()** (p108)
  - Examining Method Arguments (p104)
	- ReflectionMethod::getParameters() 
		- $prod_class = new ReflectionClass( 'CdProduct' );
		- $method = $prod_class->getMethod( "__construct" );
		- $params = $method->getParameters()
	- $arg->getPosition()
	- $arg->isPassedByReference()
	- $arg->isDefaultValueAvailable()
  - Using the Reflection API (p105)
	- plug-ins written
		- To achieve this, you might define an execute() method in the Module interface or abstract base class, forcing all child classes to define an implementation.
			
			