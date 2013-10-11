---
layout: post
title: "Javascript Break the Rule"
description: "misunderstand of some javascript usage"
category: "javascript"
tags: [javascript]
---
{% include JB/setup %}


# with语句

### 为什么不去使用它?

1. 意外的运行结果,可能隐式创建全局变量

2. 闭包作用域解析过多消耗

3. 后期编译

有人说,ES5的严格模式可以防止隐式创建全局变量(不用var),这样能减少with的一个问题.但是…

严格模式也不能使用with啊.

为什么说它是有用的?

1.构建浏览器开发者工具

//Chrome Developer Tools

	IS._evaluateOn =
	  function(evalFunction, obj, expression) {
	    IS._ensureCommandLineAPIInstalled();
	    expression =
	      "with (window._inspectorCommandLineAPI) {\
	        with (window) { " + expression + " } }";
	    return evalFunction.call(obj, expression);
	}

2.模拟块级作用域

	var addHandlers = function(nodes) {
	  for (var i = 0; i < nodes.length; i++) {
	    nodes[i].onclick =
	      function(e) {alert(i);}
	  }
	};

//你可以通过在外面包一个函数来解决

	var addHandlers = function(nodes) {
	  for (var i = 0; i < nodes.length; i++) {
	    nodes[i].onclick = function(i) {
	      return function(e) {alert(i);};
	    }(i);
	  }
	};

//或者使用'with'来模拟块级作用域

	var addHandlers = function(nodes) {
	  for (var i = 0; i < nodes.length; i++) {
	    with ({i:i}) {
	      nodes[i].onclick =
	        function(e) {alert(i);}
	    }
	  }
	};

# eval语句

### 为什么不去使用它?

1. 代码注入

2. 无法进行闭包优化

3. 后期编译

### 为什么说它是有用的?

1. JSON.parse不可用的时候

有人在Stack Overflow上说:

*“JavaScript的eval是不安全的,使用json.org上的JSON解析器来解析JSON”*

还有人说:

*“不要使用eval来解析JSON!用道格拉斯写的json2.js!”*

可是:

// From JSON2.js
 
    if (/^[\],:{}\s]*$/
      .test(text.replace(/*regEx*/, '@')
      .replace(/*regEx*/, ']')
      .replace(/*regEx*/, ''))) {
      j = eval('(' + text + ')');
    }

2.浏览器的JavaScript控制台都是用eval实现的

在Webkit控制台或JSBin中执行下面的代码

    (function () {
    console.log(String(arguments.callee.caller))
    })()
     
    function eval() {
    [native code]
    }

John Resig说过:
*
“eval和with是被轻视的,被误用的,被大部分JavaScript程序员公然谴责的,但如果能正确使用的话,可以用它们写出一些奇妙的,无法用其他功能实现的代码”*

# Function构造函数

### 为什么说它是有用的?

1. 代码运行在可预见的作用域内

2. 只能动态创建全局变量

3. 不存在闭包优化的问题

### 用在了什么地方?

1. jQuery的parseJSON

// jQuery parseJSON 
 
// Logic borrowed from http://json.org/json2.js

	if (rvalidchars.test(data.replace(rvalidescape,"@")
	  .replace( rvalidtokens,"]")
	  .replace( rvalidbraces,""))) {
	 
	  return ( new Function( "return " + data ) )();
	}

2.Underscore.js的字符串内插

//from _.template
 
// If a variable is not specified,
// place data values in local scope.

	if (!settings.variable) 
	  source = 'with(obj||{}){\n' + source + '}\n';
 
 
	var render = new Function(
	  settings.variable || 'obj', '_', source);

# ==运算符

### 为什么不去使用它?

1. 强制将两边的操作数转换为相同类型

### 为什么说它是有用的?

1. 强制将两边的操作数转换为相同类型

2.undefined == null

//这样写是不是很麻烦

if ((x === null) || (x === undefined))
 
//完全可以下面这样写

if (x == null)

3.当两边的操作数类型明显相同时使用

	typeof thing == "function";   //typeof运算符肯定返回字符串
	myArray.length == 2;          //length属性肯定返回数字
	myString.indexOf('x') == 0;   //indeOf方法肯定返回数字
真值不一定==true,假值不一定==false

	
	if ("potato") {
	  "potato" == true;  //false
	}

# Array构造函数

### 为什么不去使用它?

1. new Array()也是evil的?JSLint也推荐使用[].

### 为什么说它是有用的?

//From prototype.js extension of 
//String.prototype

	function times(count) {
	  return count < 1 ?
	    '' : new Array(count + 1).join(this);
	}
	'me'.times(10); //"memememememememememe"