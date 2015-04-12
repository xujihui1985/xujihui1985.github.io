---
layout: post
title: "javascript prototype"
description: ""
category: "javascript"
tags: [prototype]
---
{% include JB/setup %}

1. the new keywords can be add to any Functions

```
new Function();
```

2. when the new keywords was added, the function call becomes a constructor call

3. when a constructor call happened, there are 4 things happened


```
function Foo() {
    1. a brand new object was created
    2. the object linked to the prototype of the constructor
    3. the object was set to thisargs of the constructor
    this.name = 'sean';
    4. the object was returned from the constructor
}

var obj = new Foo();
```

understard prototype

in javascript `Object` is a function, it's prototype was linked to and object
when create a `class` Foo, the prototype of Foo was set to and object, and this object linked to the prototype of Object

**prototype is the property of a constructor while constructor is a property of prototype**

`__proto__` is a getter function of the Object.prototype

when calling foo.__proto__, it first look for __proto__ of it's own, if not found, it look for its' [[Prototype]], until it found it
tunrs out `__proto__` is a getter function of the top of the linkage Object.prototype


```
foo.__proto__ === Object.getPrototypeOf(foo)
foo.constructor === Foo
foo.__proto__ === foo.constructor.prototype
```

### OLOO

*Object Link to Other Object*

this is another way to think object inherits in javascript

first let's look at how it was achieve in triditional oop

```
// create a constructor Foo
function Foo(who) {
    this.me = who;
}

Foo.prototype.identify = function() {
    ...
}

function Bar() {
}

// inherits from Foo.prototype, set the constructor to Bar
Bar.prototype = Object.create(Foo.prototype, {
    constructor: Bar
});

var b1 = new Bar();
b1.speak();

```

now let's see how to achieve in OLOO

```
var Foo = {
    init: function(who) {
        this.me = who; 
    },
    identify: function() {
        return 'I am ' + this.me;
    }
};

var Bar = Object.create(Foo);

Bar.speak = function() {
    alert('Hello' + this.identify());
};

var b1 = Object.create(Bar);
b1.init('b1');
b1.speak();
```
