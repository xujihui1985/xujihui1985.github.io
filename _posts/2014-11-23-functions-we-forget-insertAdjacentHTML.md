---
layout: post
title: "functions we forget -- insertAdjacentHTML"
description: "functions we forget"
category: "javascript"
tags: [dom,function]
---
{% include JB/setup %}


we usually like to append html like this

```javascript
element.innerHTML += newHTML

```

but the problem is by doing this, you will lost all the events binding on that element, so if you are only appending more html use `insertAdjacentHTML`

```
element.insertAdjacentHTML('beforeend', newHTML);

```

>'beforebegin'
>Before the element itself.
>'afterbegin'
>Just inside the element, before its first child.
>'beforeend'
>Just inside the element, after its last child.
>'afterend'
>After the element itself.

```
<!-- beforebegin -->
<p>
<!-- afterbegin -->
foo
<!-- beforeend -->
</p>
<!-- afterend -->
```

it is well supported in all browser, even in IE6