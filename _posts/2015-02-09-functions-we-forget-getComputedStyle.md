---
layout: post
title: "functions we forget -- getComputedStyle"
description: "getComputedStyle"
category: "javascript"
tags: [functions-we-forget]
---
{% include JB/setup %}


今天被问到一个问题，如何把一个`p`tag中的字体大小翻倍，乍一听觉得很容易，就写下了第一个版本

```
$.fn.doubleFontSize = function() {
	var fontSize = this.style.fontSize;
	this.style.fontSize = parseFloat(fontSize) + 'px';
}
```

这里有两个问题， 第一个， `fontSize` 可能为空，但`fontSize`为空的情况下，应该是找parent element的`fontSize`, 第二个问题，有可能element的unit是`em`, 这个时候加入强制转为`px`就错了。

其实，浏览器原生的window对象下，已经有个方法，`window.getComputedStyle`, 这个方法可以获取到一个元素真正的值，举个例子，假如一个元素的fontSize为空，那么这个方法将显示它父元素的fontsize, 假如它的单位是`em`, 那么它会返回 `(父元素的fontsize * em)+'px'`;

```html
<style>
 #elem-container{
   position: absolute;
   left:     100px;
   top:      200px;
   height:   100px;
 }
</style>

<div id="elem-container">dummy</div>
<div id="output"></div>  

<script>
  function getTheStyle(){
    var elem = document.getElementById("elem-container");
    var theCSSprop = window.getComputedStyle(elem,null).getPropertyValue("height");
    document.getElementById("output").innerHTML = theCSSprop;
   }
  getTheStyle();
</script>
``` 

所以正确的应该是

```
$.fn.doubleFontSize = function() {
	var fontSize = window.getComputedStyle(this,null).getPropertyValue('fontSize');
	this.style.fontSize = (parseFloat(fontSize) * 2) + 'px';
}

或者

$.fn.doubleFontSize = function() {
	var fontSize = $(this).css('fontSize');
	this.style.fontSize = (parseFloat(fontSize) * 2) + 'px';
}

这里，jquery的css方法返回的也是computedStyle
```