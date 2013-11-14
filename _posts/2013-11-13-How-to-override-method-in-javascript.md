---
layout: post
title: "How to override method in javascript"
description: ""
category: "javascript"
tags: [Tips]
---
{% include JB/setup %}


			function addmethod(object, methodName, fn) {
				//1
				var old = object[methodName];
				object[methodName] = function() {
					//2
					if(fn.length === arguments.length) {
						return fn.apply(this, arguments);
					}else if(typeof old === 'function') {
						return old.apply(this, arguments);
					}
				};
			}
	
			var ninjas = {
				values: ['aaa', 'bbb','ccc']
			};
	
			addmethod(ninjas, 'find', function() {
				return this.values;
			});
	
			addmethod(ninjas,'find', function(index) {
				return this.values[index];
			});
	
			console.log(ninjas.find());
			console.log(ninjas.find(1));


1. use old to point to the method if it is already exists in the object
2. check if the parameter length is same as the arguments length, if it is same then we assume it's the method we want to call, then use apply to call the method and use `this` as the context
otherwise, call old method.