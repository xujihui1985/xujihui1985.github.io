---
layout: post
title: "Block"
description: "block syntax"
category: "objc"
tags: [objc]
---
{% include JB/setup %}

### Block Syntax

```
int (^squareBlock)(int) = ^(int)(int num) {
	return num * num;
};

```

in this example, the lhs declare the name and type of the variable, `int` is the return type, `squareBlock` is the variable name and `(int)` is parameter type

the rhs is the declaration of the method body, it says, the return value is int it takes one paramter num which is int and the method body is `return num * num`
`int` return type on the rhs can be ignored

we can use typedef to shorten the variable name

```
typedef int(^IntBlock)(int);

IntBlock squareBlock = ^(int)(int num) {
	return num * num
};

```


### variable scope

the block can ref to the variable outside the block

```
(void) changeVarInBlock {
	int constInBlock = 5;
	
	__block int modifyInBlock = 10;

    int(^addLexicalScopeVars)(void) = ^int(void){
        modifyInBlock = 25;
		return constInBlock + modifyInBlock;
	};

    int sum = addLexicalScopeVars();
}

```

in this example, if we want to change th moifyInBlock, we must defined `__block`, otherwise, the variable inside the block is readonly;

### inline blocks

```
-(void)runMyBlock:(void (^)(NSString *stringParam)) block;

[self runMyBlock: ^(NSString *str) {
	NSLog(@"%@", str);
}];

```

### using block in queue

#### serial queue

```
dispatch_queue_t queue;

queue = dispatch_queue_create("com.xxxxx.xxxxx", DISPATCH_QUEUE_SERIAL);
dispatch_async(queue, ^{
	// do work;
});

```

#### concurrent queue

```
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
dispatch_async(queue, ^{
	// do work;
});
```


#### NSOperationQueue

```
// init a task queue
NSOperationQueue *taskQueue = [[NSOperationQueue alloc] init];

// create an operation
NSOperation *opt = [NSBlockOperation blockOperationWithBlock: ^{
	//do some work;
    dispatch_async(dispatch_get_main_queue(), ^(void) {
		//update ui thread;
	});	
}];

// set a complete block for opt;
[opt setCompletionBlock: ^{
	//done after opt finished
}];

// create another operation
NSOperation *opt2 = [NSBlockOperation blockOperationWithBlock: ^{
	//do some work;
    dispatch_async(dispatch_get_main_queue(), ^(void) {
		//update ui thread;
	});	
}];

// set completion for opt2
[opt2 setCompletionBlock: ^{
	//done after opt2 finished;
}];

// set opt as dependency of opt2
[opt2 addDependency:opt];

//add opt and opt2 to taskQueue and kick off task execution
[taskQueue addOperation:opt];
[taskQUeue addOperation:opt2];

```