---
layout: post
title: "Typescript Fundamentals"
description: ""
category: "Javascript"
tags: [Typescript]
---
{% include JB/setup %}


# TypeScript

### TypeScript is a superset of Javascript

**superset** means typescript can lex the javascript syntax directly, the benefit of this feature is that it dramaticly reduce the cost of convert legcy javascript to typescript, the main script language of **Jerry project** is Typescript, but there are still several javascript library is written in javascript. 
 
### the keyworks and operators of typescript overview

1. class		
2. constructor	
3. exports		
4. extends 		
5. implements	
6. imports		
7. interface	
8. module
9. public/private
10. ()=>{}
11. <TypeName>
...


### Code hierarchy:

Module => Class, Interface => Fields, Contractor, Properties, Functions

Let's `hello world`:

learning a new language is exciting, I'm always happy when I saw my hello world sample run.


## Type Inference:

Dynamic VS Static

Javascript is dynamic language, that's said, you can do this

	var num = 2;
	num = 'two';

but in typescript, there it's illegal, there is type check in compile time

Let's see an example
	
	//function type
	var func: (firstPara: string, secondPara: number) => string = (firstPara, secondPara) => {
	    return firstPara + secondPara;
	};
	
	var result = func('hello', 2);
	
	var thefunction_that_take_func_as_paramater: (func: (firstPara: string, secondPara: number) => string) => void
	    = (func) => {
	    var
	        hello = 'hello',
	        world = 2;
	    console.log(func(hello,world));
	}
	
	thefunction_that_take_func_as_paramater((a, b) => {
	    return '';
	});

## Primitive Types

	var age: number = 20;
	var hasData: bool = true;
	var name: string = 'sean';
	var names: string[] = ['sean','york','jasper'];

## using external Library

when using external library like knockout or jQuery

there will be problem when use them directly. because, typescript don't recognize them.

	var name = ko.observable('Sean');
	/*
	
	Compile Error. 
	See error list for details
	 D:/work/Code/TypeScriptFundamental/TypeScriptFundamental/scripts/UsingExternalLibrary.ts(3,12): error TS2095: Could not find symbol 'ko'.
	
	
	*/

so one simple solution is to use **ambient declarations**

	declare var ko;

this line of code tells typescript that don't worry ko is type `any`
but we still don't have typecheck, or intellsence, (why we need typescript that don't have these feature ?)

so we need `*.d.ts` file, d.ts file is just the metadata of the library that tell ts what does this library have

just drag the `d.ts` to the top of the ts file.


## Objects
see example

## Function

strong Parameter types (required value or optional parameter)

lambda expression

2 ways to write function, they are both same

	var func = function(name: string, age: number) {
		return 'my name is'+name;
	};

	var func = (name:string, age: number) => 'my name is' + name;

params

	func(...strs: string[]){

	}	

	func('a','b','c');

## Interface

interface is the most attractive feature I like in typescript

interface will not produce javascript

in csharp or java(maybe), interface can only apply on Class, but in typescript, interface can also apply on Function

let's refact the the function we just created;

we have been using jQuery plugin for many years, one thing makes me not very happy with javascript, is sometime, it will have lots of options to initialize the object, and I always forget the name of the option, and interface can help us out.

	 $(function() {
	    $( document ).tooltip({
	      position: {
	        my: "center bottom-20",
	        at: "center top",
	        using: function( position, feedback ) {
	          $( this ).css( position );
	          $( "<div>" )
	            .addClass( "arrow" )
	            .addClass( feedback.vertical )
	            .addClass( feedback.horizontal )
	            .appendTo( this );
	        }
	      }
	    });
	  });


## Class

Class is just a container

`this` keyword is mandatory when call instance method

if you are working with ES5 or higher, you can also use properties;

the syntax is 
	private _instrument:　string;
	get instrument(): string {
		return this._instrument;
	}
	set instrument(value: string) {
		this._instrument = value;
	}


Casting Types

	var table: HTMLTableElement = document.createElement('table');

	or

	var table = <HTMLTableElement> document.createElement('table');

because HTMLTableElement is inherited from HTMLElement

static Method:

use `static` keyword, we can add static method on the class

Demo:

## magic this

`this` is magic in javascript, everytime I saw this in the javascript, the first thing I will do is check, what is `this`.
In typescript, it try to make `this` easy that every time I use `this` in the class, it always point to the object itself.

BUT......

let's see an example

in manipulatingDOM.ts, we are achieving a very easy task, attach an event on the button element and set the value of the input element
 

## Extending Types

type script support extend type and interface



## Module



### what's next?

separate modules with requireJS,

we have seen how to use module to separate large application, but the problem is it's hard to maintain the relationship between different js file, we will see how we solve this problem with requireJS

