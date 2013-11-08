---
layout: post
title: "How jQuery Test Feature"
description: ""
category: "javascript"
tags: [Tips]
---
{% include JB/setup %}


# How jQuery Test Feature


	// Check if getElementsByTagName("*") returns only elements
	support.getElementsByTagName = assert(function( div ) {
		div.appendChild( doc.createComment("") );
		return !div.getElementsByTagName("*").length;
	});


	function assert( fn ) {
		var div = document.createElement("div");
	
		try {
			return !!fn( div );
		} catch (e) {
			return false;
		} finally {
			// Remove from its parent by default
			if ( div.parentNode ) {
				div.parentNode.removeChild( div );
			}
			// release memory in IE
			div = null;
		}
	}

jQuery use a ticky way to test if getElementsByTagName is supported in the current browser

create a new element, and if return false or execption thrown, then return false, finally, remove the element. and set variable to null to let gc collect this variable as quick as possible.