---
layout: post
title: "Javascript Break the Rule"
description: "misunderstand of some javascript usage"
category: "javascript"
tags: [javascript]
---
{% include JB/setup %}


# with


//Chrome Developer Tools

	IS._evaluateOn =
	  function(evalFunction, obj, expression) {
	    IS._ensureCommandLineAPIInstalled();
	    expression =
	      "with (window._inspectorCommandLineAPI) {\
	        with (window) { " + expression + " } }";
	    return evalFunction.call(obj, expression);
	}


	var addHandlers = function(nodes) {
	  for (var i = 0; i < nodes.length; i++) {
	    nodes[i].onclick =
	      function(e) {alert(i);}
	  }
	};


	var addHandlers = function(nodes) {
	  for (var i = 0; i < nodes.length; i++) {
	    nodes[i].onclick = function(i) {
	      return function(e) {alert(i);};
	    }(i);
	  }
	};

	var addHandlers = function(nodes) {
	  for (var i = 0; i < nodes.length; i++) {
	    with ({i:i}) {
	      nodes[i].onclick =
	        function(e) {alert(i);}
	    }
	  }
	};

# eval

// From JSON2.js
 
    if (/^[\],:{}\s]*$/
      .test(text.replace(/*regEx*/, '@')
      .replace(/*regEx*/, ']')
      .replace(/*regEx*/, ''))) {
      j = eval('(' + text + ')');
    }

    (function () {
    console.log(String(arguments.callee.caller))
    })()
     
    function eval() {
    [native code]
    }


# Function


### jQuery

// jQuery parseJSON 
 
// Logic borrowed from http://json.org/json2.js

	if (rvalidchars.test(data.replace(rvalidescape,"@")
	  .replace( rvalidtokens,"]")
	  .replace( rvalidbraces,""))) {
	 
	  return ( new Function( "return " + data ) )();
	}

2.Underscore.js
 
// If a variable is not specified,
// place data values in local scope.

	if (!settings.variable) 
	  source = 'with(obj||{}){\n' + source + '}\n';
 
 
	var render = new Function(
	  settings.variable || 'obj', '_', source);

# ==

undefined == null

if ((x === null) || (x === undefined))

is same as

if (x == null)


	typeof thing == "function";   
	myArray.length == 2;         
	myString.indexOf('x') == 0;   

	
	if ("potato") {
	  "potato" == true;  //false
	}

# Array

//From prototype.js extension of 
//String.prototype

	function times(count) {
	  return count < 1 ?
	    '' : new Array(count + 1).join(this);
	}
	'me'.times(10); //"memememememememememe"


### test if array contain element

```
!!~array.indexOf(element)

```
