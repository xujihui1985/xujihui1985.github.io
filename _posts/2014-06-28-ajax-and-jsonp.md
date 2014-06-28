---
layout: post
title: "new post"
description: ""
category: ""
tags: []
---
{% include JB/setup %}


`jsonp` is used to deal with cross origin request,

implementation:

```javascript
	function jsonpCallback(data) {

	}

	var script = document.createElement('script');
	script.src = "http://xxxxx/xxx/action?callback=jsonpCallback";
	document.head.appendChild(script);
```

ajax is used to deal with the request in the same domain

it is just a wrapper of `XMLHttpRequest`;

```javascript
	var xhr = new XMLHttpRequest();
	// the third parameter indicate it's sync or async
	xhr.open('GET', 'url', true);
	//the callback that will be fired.
	xhr.onload = function() {
		if(this.status === 200) {
			alert(this.responseText);			
		}
	}
	xhr.send();
```

```javascript
	var xhr = new XMLHttpRequest();
	xhr.open('POST', 'url', true);
	//the callback that will be fired.
	xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
	xhr.onload = function() {
		if(this.status === 200) {
			alert(this.responseText);			
		}
	}
	xhr.send("category=books&sort=asc");
```

can also pass formdata

```javascript
	var formData = new FormData();
	formData.append("category","books");
	formData.append("sort","asc");
	//can also pass form object to formData
	var form = document.getElementById("myform");
	formData = new FormData(form);

	var xhr = new XMLHttpRequest();
	xhr.open('POST', 'url', true);
	//the callback that will be fired.
	xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
	xhr.onload = function() {
		if(this.status === 200) {
			alert(this.responseText);			
		}
	}
	xhr.send(formData);
```