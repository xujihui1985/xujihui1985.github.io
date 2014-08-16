---
layout: post
title: "javascript debug tips"
description: ""
category: "javascrip"
tags: [debug]
---
{% include JB/setup %}


### debug javascript on mobile

tool: [weinre](http://people.apache.org/~pmuellr/weinre/docs/latest/)



### intecept native javascript function

something always happened on me is when the class of dom element changed, I don't know how it was changed. I want to listen on the event when some attribute on the element changed. but this code is not written by me.

```
var original = Element.prototype.setAttribute;

Element.prototype.setAttribute = function(attr, value) {
	if(attr === 'the attribute you want to listen') {
		debugger;
	}
	original.call(this, attr, value);
}

```

this is a common skill in javascript to do aop 

			
