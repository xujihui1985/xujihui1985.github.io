---
layout: post
title: "Go Test"
description: ""
category: "Go"
tags: [GoTest]
---

unittest is import

in go the conversion is *_test.go

in the package, any file surfixed with _test.go is treated as unitttest file.

and in unittest file, you need to import testing package

	import (
		"testing"
	)
	
	func TestFirstTest(t *testing.T) {
		if testing.Short(){
			t.Error("get data error")
		}
	
	}

the test method is prefixed with TestXXXX


run unittest is simple, go to the package folder run

	go test

there are a lot of option arg for go test

	go test -test.run=TestFirstTest

this will exclusivly run the specified test

	go test -test.short=true

this will set the short flag to true
then in your test you can test if current test is in Short mode by 
	
	if testing.Short(){
		
	} 
