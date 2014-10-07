---
layout: post
title: "DateTime in javascript"
description: ""
category: "javascript"
tags: [datetime]
---
{% include JB/setup %}

```

var ts = Date.now();  // return the timestamp
var dt = new Date(ts);

var dt = new Date(); //return the current local time
var ts = new Date().getTime();
var dt = new Date(year, month. day, hour, minutes, second, millisecond);

var ts = Date.UTC(year, month, day, hour .....);
```

```
new Date(2014, 1, 1).toDateString();   // be careful, javascript month is start from 0
==> 2014 02 01

```

timezone issue
```
new Date(2013, 9, 20, 12).toDateString()   // always set the hour to noon which is 12, even if you don't care about the timezoon, it can save lots of trouble
```

Datetime parse

UTC and Local depands on the input string
```

UTC: new Date("2013-09-30")  //note use - and 2 digital 09 and 30

Local: new Date("2013/09/30")  //note use /

```