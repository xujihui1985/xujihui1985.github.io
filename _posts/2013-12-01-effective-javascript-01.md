---
layout: post
title: "Effective Javascript"
description: ""
category: "javascript"
tags: [effectivejs]
---
{% include JB/setup %}


## Check undefined

use typeof to check "undefined" instead of check undefined directly.

typeof obj === "undefined"

because in unstrict mode, undefined can be set value.


## Check NaN

a easy to check NaN 

the native isNaN is not very reliable, because coercible

	isNaN('1')  // false
	isNaN(1) // false

Since NaN is the only javascript value that is treated as unequal to itself, you can always test it fi a value 
is NaN by checking it for equality to itself:

	var a = NaN;
	a !== NaN;   //true

	var b = "foo";
	b !== b;    // false

	function isReallyNaN(x) {
		return x !== x;
	}


javascript allows comma-separated expressions, which evaluate from left to right and return the value of their last subexpression.

	var b = {a:'a', b:'b', c:'c'};

in this case `b['a','b','c']` will return `c` as the result


## simicolon

you can follow a rule of always prefixing statements beginning with `(, [,+,-, or /` with an extra semicolon.

	(function(){

	})()

	;(function(){

	}())

when you write your module, it's more save to prefixing an extra semicolon, because you have no idea if other library author have add `;` to the end of their module 

When contactenating scripts insert semicolons explicitly between scripts


## why with is not good

`with` looks pretty good to save some typing
for example

	function foo(info) {
		var obj = new SomeObject();
		with(obj) {
			setBackground('blue');
			setText(info);
		}
	}

yes, it looks good at first glance, but when I look at `info`, I don't know if it's a property on `SomeObject`
or it's the parameter of function `foo`


## Variable Hoisting

Javascript does not support block scoping, variable definitions are not scoped to their nearest enclosing statement or block, but rather to their containing function.
but there is one exception that is `exception`
try...catch binds a caught exception to a variable that is scoped just to the catch block.

	function test (){
		var x = 'var';
		try{
			throw new Error('test: error');
		}catch(x) {
			console.log(x.message);
			x='error';
			console.log(x);
		}
		console.log(x);
	}



## Prototype

When you instantiate an object (by invoking the function using the new operator), it inherits all the properties in the prototype of that function. Keep in mind, though, **that those instances will not have direct access to the prototype object but only to its properties.**
	
	// Extending the Person prototype from our earlier example to
	// also include a 'getFullName' method:
	Person.prototype.getFullName = function() {
	  return this.firstName + ' ' + this.lastName;
	}
	
	// Referencing the p1 object from our earlier example
	console.log(p1.getFullName());            // prints 'John Doe'
	// but p1 can’t directly access the 'prototype' object...
	console.log(p1.prototype);                // prints 'undefined'
	console.log(p1.prototype.getFullName());  // generates an error