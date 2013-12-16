---
layout: post
title: "unit test in javascript"
description: ""
category: "javascript"
tags: [unittest]
---
{% include JB/setup %}

# unit test in javascript


> As we all know, javascript now plays a very important role in today's web development, we have adopted writing unit test in serverside, however writing unittest in javascript is also very important, and it's always a good habbit to writing unittest for your code.


## use bower to install QUnit

	bower install QUnit --save-dev

if you want to change the default path of the script installation, look at .bowerrc file

	{
	    "directory": "app/bower_components"
	}


## writing unit test with QUnit

QUnit is a member of xUnit family. and the syntax of it is quite similar as our old friend NUnit/JUnit.
	
	module('utility test');
	
	test('pass test', function(){
	   ok(true);
	});


## module
the function `module` is to define a test fixture, a group of test.

	module: function( name, testEnvironment ) {
		config.currentModule = name;
		config.currentModuleTestEnvironment = testEnvironment;
		config.modules[name] = true;
	},

when create a module in the test, QUnit will create a module with the name you passed in it's private config object.

## setup and teardown
the second parameter of `module` is an object with two functions `setup`, and `teardown`, the function `setup` will run on every time the test run within the module. same as `teardown`


## the purposes of `setup` and `teardown`

1. prepare some fake data and clear it after test
2. Setting up the DOM

test dom element is tricky, every little changes in UI will break the test easily.
eg: the message of the error, the innerHtml of the element, etc...

but lots of times, the message changed should not lead to the break of the unit test. or the message changed is due to another issue. how can we avoid these noise?

see demo of utility.dom 

create fake dom container in the setup
remove container in the teardown in order not to pollute the dom


## run all the tests in one test runner

testing multipal modules that locate in different folder becomes cumbersome, when there are more and more scripts added into the project.

we can use Qunit composite plunin to include all the test into one test runner.

but this is not the best approach, we will see the best approach later.


## async test.

there are lots of async action in javascript, ajax call, test these function is interesting.
let's look at a failure sample.


## mocha

install mocha

npm install -g mocha

setting up mocha for the browser

	mocha init ./   // this will create mocha test file in the current directory 

why mocha?

plain English,
 
	describe('my test suite', function(){
	
	    it('should pass', function(){
	        expect(1).to.equal(1);
	
	    });
	}
 
hierarchical test structure. which means you can create sandbox inside each suite

can run exclusive test with the `only` method. and only this test will run.

can skip the test with `skip` method.
 
`only` and `skip` can apply on both `describe` scope and `it` scope

it can also warning you if the test is running too long.

easy to write async test

config the timeout, default timeout is 3 seconds.

	mocha.setup({timeout:5000});

or config the time out inside the test

	describe('async test', function () {
	       
	        it('should pass the test', function (done) {
			    this.timeout(5000);
	            setTimeout(function () {
	                expect(1).to.equal(1);
	                done();
	            }, 3000);
	        });
	
	    });

## mock

mock with Sinon 

sinon.spy() return a stub function, which can be passed into other function as parameter, to test if the method is called.

eg: when I want to test the event handle is called when some event fired. I can use spy() without truly implement it.

spy.getcall(n)

Returns the nth call. Accessing individual calls helps with more detailed behavior verification when the spy is called more than once. Example:

spyCall.calledWith

Returns true if spy was called at least once with the provided arguments. Can be used for partial matching, Sinon only checks the provided arguments against actual arguments, so a call that received the provided arguments (in the same spots) and possibly others as well will return true.

etc...


### stub

there are three main purpose of using stub.

1. Control behavior

2. Suppress existing behavior when the true function is not ready,(you don't have to ask your colleague when you can finish you part)

3. Behavior verification


 creating stubs


		var stub = sinon.stub(object,'method');
		var stub = sinon.stub(object,'method',function(){});
		var stub = sinon.stub(object);  //this will stub all the method on that object really handy when the object is very large.


## fake timer

what we can do with fake timer?

1. we can test the method that depend on specific time  eg: current time
2. we can test animate, we don't have to wait until animate finished, instead we can fast play.
3. ..... 

## fake XHR


## test with grunt

demo on how to setup grunt to run mocha test in the command line

you can config the task by create multipal target, and run the target separately

##  continous integration

demo with travis-ci

## set up server quickly with nodejs

	npm install http-server -g

run http-server under the folder

or create a test env with expressjs which I preferred