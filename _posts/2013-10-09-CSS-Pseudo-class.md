---
layout: post
title: "Understand CSS Pseudo Class"
description: ""
category: "CSS"
tags: [CSS]
---
{% include JB/setup %}


### What is Pseudo Class?
We have used Pseduo-Class from older version CSS, you muse have seen `:hover` `:link` `:active`

`:before` and `:after` are used from CSS2.1, in the early version, it used single `:`
but in CSS3 it become `::` that is said `::before`, `::after`, but single `:` is still accepted.

### Let's look at an exemple

<iframe width="100%" height="300" src="http://jsfiddle.net/xujihui1985/WbHwQ/embedded/" frameborder="0"> </iframe>

we can also use image as the before element or after element

	blockquote:before {
	 content: " ";
	 font-size: 24pt;
	 text-align: center;
	 line-height: 42px;
	 color: #fff;
	 float: left;
	 position: relative;
	 top: 30px;
	 border-radius: 25px;
	 
	 background: url(images/quotationmark.png) -3px -3px #ddd;
	 
	 display: block;
	 height: 25px;
	 width: 25px;
	}

	blockquote:after {
	 content: " ";
	 font-size: 24pt;
	 text-align: center;
	 line-height: 42px;
	 color: #fff;
	 float: right;
	 position: relative;
	 bottom: 40px;
	 border-radius: 25px;
	 
	 background: url(images/quotationmark.png) -1px -32px #ddd;
	 
	 display: block;
	 height: 25px;
	 width: 25px;
	}

to use pseudo class with pseudo element together

	blockquote:hover:after, blockquote:hover:before {
	 background-color: #555;
	}

this will make a nice background change when hover on the before and after element

also we could apply transition on the pseudo-class

	transition: all 350ms;
	-o-transition: all 350ms;
	-moz-transition: all 350ms;
	-webkit-transition: all 350ms;

### example 
<iframe width="100%" height="300" src="http://jsfiddle.net/xujihui1985/WbHwQ/4/embedded/" frameborder="0"> </iframe>