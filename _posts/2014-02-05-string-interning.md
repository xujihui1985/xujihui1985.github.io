---
layout: post
title: "String interning"
description: ""
category: "csharp"
tags: [csharp]
---
{% include JB/setup %}

		[Test]
        public void StringReferenceNotSameTest()
        {
            var he = "he";
            var hello = "hello";
            var hello2 = he + "llo";
            Assert.False(ReferenceEquals(hello,hello2));
            Assert.AreNotSame(hello,hello2);
        }

        [Test]
        public void StringSameReferenceTest()
        {
            var hello = "hello";
            var hello2 = "he" + "llo";
            Assert.True(ReferenceEquals(hello, hello2));
            Assert.AreSame(hello, hello2);
        }

consider these two tests, the first test will create three object in the heap,
because `he` is not const, so clr didn't treat it as same as "hello", there will be three object, "he", and two same "hello"

the second test, will create only one object as `he` here is const

so when concat string, don't write such code
	
	var str = "abc";
	var str2 = str + "bcd";

instead
	
	var str2 = "abc" + "bcd";

so when there is also "abcbcd" in the heap, clr will not create a new object