---
layout: post
title: "ES6"
description: "javascript ES6"
category: "javascript"
tags: [es6,javascript]
---
{% include JB/setup %}


#### Destructuring

```
 	let [x, y, z] = function() {
						return [1,2,3];
					}

    x = 1
    y = 2
    z = 3

	let [, x, y] = [1,2,3];
    x = 2
    y = 3

    let { firstName: first, twitter: twitter} = function() {
                                                 return {
                                                    first: 'sean',
                                                    twitter: 'sean@twitter'
                                                 }

                                               }


```

if the new propertyname happened to be as same as the the returned object, then we can use shorthand assign

```
let { firstName, lastName } = function() {
	return {
		firstName: 'sean',
		lastName: 'xu'
	}
}
```

#### Default Paramenter

```
var doWork = function(name='sean') {
    return name;
};

var myName = doWork();

```

#### Rest Paramenters

```
let sum = function(...numbers) {
    console.log(Array.isArray(numbers)); // true
    let result = numbers.reduce(function(prev, curr) {
        return prev + curr;
    },0);
    return result;
}

sum(1,2,3); // ==> 6

```

#### Spread

```
let spreadTest = function(x, y, z) {
    return x + y + z;
};

var result = spreadTest(...[1,2,3]);

var a = [1,2,3];
var b = [4,5,6,...a, 7,8,9];

b.forEach(print);

function print(element) {
    console.log(element);
}
```


#### template literals

```
let category = 'music';
let id = 123;

let url = `http://apiserver/${category}/${id}`;
```

this is stolen from ruby

```
let upper = function (strings, ...values) {
    let result = '';

    for(let i = 0; i < strings.length; i++) {
        result += strings[i];

        if (i < values.length) {
            result += values[i];
        }
    }
    return result;
};

var x = 1;
var y = 3;
var result = upper `${x} + ${y} is ${x + y}`;
```

here, upper is a `tag`, used to parse template


#### class

```

class Employee {

   // constructor function to initialize object
   constructor(name) {
      this._name = name;
   }

   doWork() {
      return  'completed!';
   }

   getName() {
      return this._name;
   }
}

let e = new Employee('sean');
e.doWork();
e.getName();
```

#### get set

```
class Person {
   constructor(name) {
      this._name = name;
   }

   get name() {
      return this._name;
   }

   set name(value) {
      if(!value) {
         throw new Error('name can not be empty');
      }
      this._name = value;
   }
}

var p = new Person();
console.log(p.name);
p.name = 'sean';
```

#### super
```
class Worker extends Person{
   constructor(name, job) {
      super(name);
      this._job = job;
   }
   doWork() {

      return `${this._name} is working`;
   }
}

class Worker extends Person{
   constructor(name, job) {
      super(name);
      this._job = job;
   }
   doWork() {
      var message = super.doWork();
      return `${this._name} ${message}`;
   }
}
```


#### arrow function

```
let add = (x, y) => x + y;

add(1,2);

let multiple = (x, y) => {
    let temp = 123;
    return x * y + temp;
};

[1, 2, 3, 4, 5].forEach(item=> {
    console.log(item);
});

[1,2,3,4,5].map(item=> item * item);
```

#### arrow function and async

arrow function always capture the this value of the context they are inside

```
...
setTimeout(()=>{
 console.log(this);
});
...

```
this will always be the enclosed context of setTimeout;


#### iterator

```

class Company {
    constructor() {
        this.employees = [];
    }

    addEmployees(...names) {
        this.employees = this.employees.concat(names);
    }

    // special Symbol to indicate this is a iterator
    [Symbol.iterator]() {
        return new ArrayIterator(this.employees);
    }
    
    *[Symbol.iterator]() {
       for(let e of this.employees) {
          yield e;
       }
    }
}

class ArrayIterator {
    constructor(array) {
        this.array = array;
        this.index = 0;
    }

    next() {
        var result = {value: undefined, done: true};
        if(this.index < this.array.length) {
            result.value = this.array[this.index];
            result.done = false;
            this.index ++;
        }
        return result;
    }
}

let count = 0;
let company = new Company();
company.addEmployees('sean', 'anna');
for(let employ of company) {
    console.log(employ);
}
```

#### Generators

basic

```
let numbers = function *() {
  for(let i = 0; i < 10; i++) {
    console.log(i);
    yield i;
  }
}

let sum = 0;
let iterator = numbers();
console.log('next');
let next = iterator.next();
while(!next.done) {
  sum += next.value;
  console.log('next')
  next = iterator.next();
}
```

use generator to lazy filter a collection 

```
let take = function *(items, number) {
    let count = 0;
    if(number < 1) return;  //this set the done flag to true immediately
    for(let item of items) {
        yield item;
        count++;
        if(count >= number) {
            return;
        }
    }
};

let filter = function *(items, predicate) {
  for(let item of items) {
      if(predicate(item)) {
          yield item;
      }
  }
};

for(let employ of take(filter(company, e=>e[0] === 'T'), 3)) {
    console.log(employ);
}
```

#### different between yield * and yield

when I yield an array from a generater, the value of the yield item is an `array`

```
let filter = function *(items, predicate) {
  yield [for (item of items) if(predicate(item)) item];
}

for(item of filter([1,2,3], item => item === 3)) {
  console.log(item);
}

in this case item will be [3];

```

if I want to yield individual item, then use `yield *`



#### comprehensions

```
var numbers = [for (n of [1,2,3]) n * n];

```


#### number

```
var octal = 0o71
var bin = 0b1101  ==> 13
```

#### array

```
var arrayLike = document.querySelectorAll('div');
var fromArray = Array.from(arrayLike);
fromArray.forEach(item=> console.log(item));
```

#### set

```
var set = new Set();
set.add('somevalue');
var key = {};
set.add(key);
set.has(key) === true

var set = new Set([1,2,3]);

set.clear();

```

#### map

```
var map = new Map();
map.set('age', 11);
map.get('age') === 11;

the key of map can be object

var sean = {'name': 'sean'};
map.set(sean, 29);

map.get(sean) === 29

```


#### weakmap & weakset

when I use set to hold a list of dom element, when I remove the dom element from the dom tree, it can not be gced, because it is still pointed by set, I need to remove the element from the set manually

with weakset, the element will be gced once there is no other element reference this element

```
var set = new WeakSet();

set.add
set.delete
set.has


```


#### promise

```
function asyncFunction() {

    var promise = new Promise(function (resolve, reject) {
        if (somecondition) {
            resolve(result);
        } else {
            reject(new Error('sdfadas'));
        }
    });

    return promise;
}

promise
    .then(function () {

    })
    .catch(function (error) {

    });
```

#### generator

```

(function () {
    var sequence;
    var run = function(generator) {
        sequence = generator();
        sequence.next();
    };

    var resume = function() {
        sequence.next();
    };

    var fail = function (reason) {
        sequence.throw(reason);
    };

    window.async = {
        run: run,
        resume: resume,
        fail: fail
    };
}());

function pause(delay) {
    setTimeout(function() {
        console.log('pause for ...');
        async.resume();
    }, delay);
}

```


#### use generator to simplify async function

```
function getStockPrice() {
    $.get('/price', function (prices) {
        try {
            throw Error('error');
            async.resume(price);
        } catch (e) {
            async.fail(e);
        }
    });
}

function executeTrade() {
    setTimeout(function () {
        async.resume();
    }, 1000);
}

function *main() {
    try {

        var price = yield getStockPrice();
        if (price > 15) {
            yield executeTrade();
        } else {
            console.log('trace not made');
        }
    } catch (e) {
        console.log(e);
    }
}
```

#### advance usage

```
(function(){
    var run = function(generator) {
        var sequence;
        var process = function(result) {
            result.value.then(function(value) {
                if(!result.done) {
                    process(sequence.next(value))
                }
            }, function(error) {
                if(!result.done) {
                    process(sequence.throw(error));
                }
            })
        };

        sequence = generator();
        var next = sequence.next();
        process(next);
    };

    window.async = {
        run: run
    };
}());
```

```
function getStockValue() {
	return new Promise(function(resolve, reject){
      setTimeout(function() {
        resolve(value);
      })
    });
}

function *main() {
	var value1 = yield getStockValue();
}

async.run(main);
```


#### Object static function
```
Object.is(1,2) // similar with ===

Object.is(NaN, NaN)  //true

NaN === NaN  //false
```

#### Object.assign
```
Object.assign(obj1, obj2);

obj2 will be mix into obj1

var newobject = Object.assign({}, obj1, obj2, ...);

```

#### shorthand

```
var object = {
	model: mode,
	year: year
}

is same as 

var object = {
	model,
	year
}

var server = {
	getPort() {
	}
}

is same as 

var server = {
	getPort: function getPort() {
	}
}

var obj = {
	['member_'+first.name]: first
}

the [] tells compiler that this is an property expression

```

#### proxy

```
var proxyUnicorn = new Proxy(unicorn, {
    get: function(target, property) {
        if(property === 'color') {
            return 'awesome' + target[property];
        } else {
            return target[property];
        }
    },
    set: function(target, property, value) {

    }
});

var originFunction = function() {
  console.log('origin Function');
};

var newFunction = new Proxy(originFunction, {
    // target is the origin function
    //context is the this context
    // args is the argument of the function
   apply: function(target, context, args){
       if(context !== object) {
           return ''
       } else {
           target.apply(context, args);
       }
   };
});


```

### module import export

```
export class Employee {
	constructor(name) {
		this.name = name
	}

	get name() {
		return this.name;
    }

	doWork() {
       return `${this.name} is working`;
    }
}


import {Employee} from "./Employee";

var e = new Employee('sean');
e.doWork();

```

#### multiple exports

```

export let log = function(value) {
	console.log(value);
}

export let defaultRaise = 0.03;

export let modelEmployee = new Employee('sean');


```

#### module and default

if there is only one thing you want to export from the module, we can use export default syntax to export the 
module, and we can import directly from the module without {} syntax

```
export default class Employee {
	constructor(name) {
		this.name = name;
	}
}

import factory from "./Employee";

```

and we can use `module` syntax to import

```
module employees from './Employee';

var e = employees.Employee('sean');
``` 


#### run es6 in node

```
node --harmony --use-strict index.js
```