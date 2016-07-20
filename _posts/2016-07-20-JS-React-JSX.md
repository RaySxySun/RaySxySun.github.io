---
layout: post
title: ReactJS JSX
date: 2016-07-20
weather: rainy
categories: JS
tags: [JS]
description:
---

# JSX Specification

> [facebook reference](https://facebook.github.io/jsx/)

> [ECMAScript 6th Edition: ECMA-262](http://people.mozilla.org/~jorendorff/es6-draft.html)

- What is JSX
	- JSX is a XML-like syntax extension to ECMAScript without any defined semantics.
	- It's NOT a proposal to incorporate JSX into the ECMAScript spec itself.
	
		var dropdown =
		  <Dropdown>
			A dropdown list
			<Menu>
			  <MenuItem>Do Something</MenuItem>
			  <MenuItem>Do Something Fun!</MenuItem>
			  <MenuItem>Do Something Else</MenuItem>
			</Menu>
		  </Dropdown>;

		render(dropdown);

---
		
- Rationale 
	- The purpose of this specification is to define a **concise** and **familiar** syntax for defining **tree structures** with attributes. 
	- This specification does not attempt to comply with any XML or HTML specification. 
	- JSX is designed as an ECMAScript feature

---
	
- [Syntax] (http://people.mozilla.org/~jorendorff/es6-draft.html)

	- PrimaryExpression :
		- JSXElement
	- Elements
		- JSXElement
			- JSXSelfClosingElement
			- JSXOpeningElement JSXChildren JSXClosingElement
				- (names of JSXOpeningElement and JSXClosingElement should match)
		- JSXSelfClosingElement 
			- < JSXElementName JSXAttributes / >
		- JSXOpeningElement 
			- < JSXElementName JSXAttributes >
		- JSXClosingElement 
			- < / JSXElementName >
		- JSXElementName 
			- JSXIdentifier
				- IdentifierStart
				- JSXIdentifier IdentifierPart
			- JSXNamedspacedName
				- JSXIdentifier : JSXIdentifier
			- JSXMemberExpression
				- JSXIdentifier . JSXIdentifier
				- JSXMemberExpression . JSXIdentifier
	- Attributes
		- JSXAttributes 
			- JSXSpreadAttribute JSXAttributes
			- JSXAttribute JSXAttributes
		- JSXSpreadAttribute 
			- { ... AssignmentExpression }
		- JSXAttribute
			- JSXAttributeName = JSXAttributeValue
				- JSXAttributeName
					- JSXIdentifier
					- JSXNamespacedName
				- JSXAttributeValue 
					- " JSXDoubleStringCharacters "
					- ' JSXSingleStringCharacters '
					- { AssignmentExpression }
					- JSXElement
	- Children
		- JSXChildren 
			- JSXChild JSXChildren
		- JSXChild 
			- JSXText
			- JSXElement
			- { AssignmentExpression }
		- JSXText 
			- JSXTextCharacter JSXText
		- JSXTextCharacter 
			- SourceCharacter but not one of {, <, > or }

---

- Parser Implementations 
	- [acorn-jsx](https://github.com/RReverser/acorn-jsx): A fork of acorn.
	- [esprima-fb](https://github.com/facebook/esprima): A fork of esprima.
	- [jsx-reader](https://github.com/jlongster/jsx-reader): A sweet.js macro

---

- Transpilers 
	- A source-to-source compiler, transcompiler or transpiler is a type of compiler that takes the source code of a program written in one programming language as its input and produces the equivalent source code in another programming language.
	- [1] JSXDOM(https://github.com/vjeux/jsxdom): Create DOM elements using JSX.
	- [2] Mercury JSX(https://github.com/Raynos/mercury-jsx): Create virtual-dom VNodes or VText using JSX.
	- [3] React JSX(http://facebook.github.io/react/docs/jsx-in-depth.html): Create ReactElements using JSX.

---
		
- Why not Template Literals?
	- Why not just use that instead of inventing a syntax that's not part of ECMAScript?
	- ECMAScript 6th Edition (ECMA-262) introduces template literals for embedding DSL
	- Unfortunately the syntax noise is substantial when you exit in and out of embedded arbitrary ECMAScript expressions with identifiers in scope.

---

- Why not JXON?
	- Another alternative would be to use object initializers (similar to [](https://developer.mozilla.org/en-US/docs/JXON))
	- JXON：lossless JavaScript XML Object Notation
	- Unfortunately, the balanced braces do not give great syntactic hints for where an element starts and ends in large trees. 