---
layout: post
title: "How to create partial method in javascript"
description: ""
category: "javascript"
tags: [Tips]
---
{% include JB/setup %}

		Function.prototype.partial = function() {
			var fn = this,
				args = Array.prototype.slice.call(arguments);

			function preparForArgs(newArgs) {
					var arg = 0,
						i,
						l;
					for(i=0,l=args.length; i<l; i++) {
						if(args[i] == undefined) {
							args[i] = newArgs[arg++];
						}
					}
			}
			return function() {

				preparForArgs(arguments);
				return fn.apply(this, args);
			}
		};

first we create partial method on the Function's prototype, that make all the function able to partial.

var fn = this;

use fn to save the origial function,
args to save the original parameters.

preparForArgs, is to create new arguments for the partial function,  use `undefined` as a placeholder, if we see undefined, then get the arguemnt from newly passed arguments.

		var delay = setTimeout.partial(undefined, 1000);

		delay(function() {
			alert('delayed');
		});

we can create a new delay function base on setTimeout function. and use undefined as the placeholder.
