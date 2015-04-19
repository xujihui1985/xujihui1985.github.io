---
layout: post
title: "pure functions"
description: ""
category: "javascript"
tags: [function]
---
{% include JB/setup %}

### pure function

Functions that don't change anything are called `pure`

example:

**impure**

this function become hard to test because you can't easily mock window.location object 

```
function getQueryVariable(variable) {
	var query = window.location.search.substring(1);
	var vars = query.split('&');
    for(var i = 0; i < vars.length; i++) {
		var pair = vars[i].split('=');
		if(decodeURIComponent(pair[0]) === variable) {
			return decodeURIComponent(pair[1]);
		}
	}
}

```


### map

we all know `[array].map(doSth)` will iterator the array and execute doSth on it, actually `map` can also apply on container, which means, **open the container, execute the callback on the value in the container then put it back to the container**

```
Container([1,2,3]).map(reverse).map(first)  // return Container(3)
```

```

var map = _.curry(function(f, object) {
	return object.map(f);
});

Container(3).map(add(1))  //Container(4)
map(add(1), Container(3)) //Container(4)

map(match(/cat/g), Container('catsup'));
map(compose(first, reverse), Container('dog'));
```

### Functor

> An object or data structure you can map over


### Maybe

```
var _Maybe.prototype.map = function(f) {
	return this.val? Maybe(f(this.val)):Maybe(null);
}

map(capitalize, Maybe(null));  //=> Maybe(null)
```

```
var firstMatch = compose(first, match(/cat/g));

firstMatch("dogsup");


var firstMatch = compose(map(first), Maybe, match(/cat/g));
```

### Functor Laws

```
_Identity.prototype.map = function(f) {
    return Identity(f(this.val));
}

map = _.curry(function(fn, obj) {
	return obj.map(fn);
})
```


```
map(id) == id

compose(map(f), map(g)) == map(compose(f,g))

compose(map(compose(toUpper,reverse)), toArray)("bingo")
==
compose(map(toUpper), map(reverse), toArray)("bingo")

```


