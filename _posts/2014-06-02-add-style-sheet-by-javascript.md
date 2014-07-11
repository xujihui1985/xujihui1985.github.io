---
layout: post
title: "add style sheet by javascript"
description: ""
category: "javascript"
tags: [css]
---
{% include JB/setup %}


```javascript
	
	// create style element
	var style = document.createElement('style');

	// Add a media (and/or media query) here if you'd like! 
	// style.setAttribute("media", "screen") 
	// style.setAttribute("media", "@media only screen and (max-width : 1024px)")

	// webkit hack
	style.appendChild(document.createTextNode(''));

	// this is important, the stylesheet will have effect only when it was added into dom tree
	document.head.appendChild(style);

	var sheet = style.sheet;

	//add rule to sheet
	sheet.addrule('#mylist li', 'float: left; background: red !important;');

	
```

### Inserting Rules for Media Queries

Media query-specific rules can be added in one of two ways. The first way is through the standard insertRule method:


```javascript

	sheet.insertRule("@media only screen and (max-width : 1140px) { header { display: none; } }");

```

the other method is creating a STYLE element with the proper media attribute, then adding styles to that new 