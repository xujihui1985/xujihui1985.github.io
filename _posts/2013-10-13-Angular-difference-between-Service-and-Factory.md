---
layout: post
title: "Difference Between Service And Factory"
description: ""
category: "Javascript"
tags: [Angular]
---
{% include JB/setup %}

# Set up Angular Application

### Service and Factory are both the function the provide inject-able object into Angular Module, the difference is subtle 

# Basicly the difference is 
1. for service, it return the instance of the function, the function it define is actually an object constructor
2. for factory, it return the result of the function, it return the object


### reference:
[http://slides.wesalvaro.com/20121113/#/](http://slides.wesalvaro.com/20121113/#/ "Difference between Service and factory")

[http://stackoverflow.com/questions/13762228/confused-about-service-vs-factory](http://stackoverflow.com/questions/13762228/confused-about-service-vs-factory "Answer from stackoverflow")