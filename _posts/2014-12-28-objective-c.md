---
layout: post
title: "Question when learning Objective c"
description: "some stupid questions when learning objc"
category: "objc"
tags: [objc]
---
{% include JB/setup %}


### Categories

```
@interface ClassToAddMethodsTo (category)
  ...methods go here
@end
```

**catetory is pretty much like partial class in c#**

[http://macdevelopertips.com/objective-c/objective-c-categories.html](http://macdevelopertips.com/objective-c/objective-c-categories.html)

As an alternative to subclassing, Objective-C categories provide a means to add methods to a class. What’s intriguing, is that any methods that you add through a category become part of the class definition, so to speak. In other words, if you add a method to the NSString class, any instance, or subclass, of NSString will have access to that method.

Defining a category is identical to defining the interface for a class, with one small exception: you add a category name inside a set of parenthesis after the interface declaration. The format is shown below:

```
@interface ClassToAddMethodsTo (category)
  ...methods go here
@end
```
```
@interface NSString (reverse)
-(NSString *) reverseString;
@end
```
```
@implementation NSString (reverse)
 
-(NSString *) reverseString
{
  NSMutableString *reversedStr;
  int len = [self length];
 
  // Auto released string
  reversedStr = [NSMutableString stringWithCapacity:len];     
 
  // Probably woefully inefficient...
  while (len > 0)
    [reversedStr appendString:
         [NSString stringWithFormat:@"%C", [self characterAtIndex:--len]]];   
 
  return reversedStr;
}
 
@end
```
```
#import <Foundation/Foundation.h>
#import "NSString+Reverse.h"
 
int main (int argc, const char * argv[])
{
  NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
  NSString *str  = [NSString stringWithString:@"Fubar"];
  NSString *rev;
 
  NSLog(@"String: %@", str);
  rev = [str reverseString];
  NSLog(@"Reversed: %@",rev); 
 
  [pool drain];
  return 0;
}
```


### the curly brace in the interface when define variable

[http://stackoverflow.com/questions/12632285/declaration-definition-of-variables-locations-in-objectivec](http://stackoverflow.com/questions/12632285/declaration-definition-of-variables-locations-in-objectivec)


### retain vs strong vs weak

[http://stackoverflow.com/questions/8927727/objective-c-arc-strong-vs-retain-and-weak-vs-assign](http://stackoverflow.com/questions/8927727/objective-c-arc-strong-vs-retain-and-weak-vs-assign)

1.strong (iOS4 = retain )

it says "keep this in the heap until I don't point to it anymore"
in other words " I'am the owner, you cannot dealloc this before aim fine with that same as retain"
You use strong only if you need to retain the object.
By default all instance variables and local variables are strong pointers.
We generally use strong for UIViewControllers (UI item's parents)
strong is used with ARC and it basically helps you , by not having to worry about the retain count of an object. ARC automatically releases it for you when you are done with it.Using the keyword strong means that you own the object.

```
@property (strong, nonatomic) ViewController *viewController;

@synthesize viewController;
```

2.weak -

it says "keep this as long as someone else points to it strongly"
the same thing as assign, no retain or release
A "weak" reference is a reference that you do not retain.
We generally use weak for IBOutlets (UIViewController's Childs).This works because the child object only needs to exist as long as the parent object does.
a weak reference is a reference that does not protect the referenced object from collection by a garbage collector.
Weak is essentially assign, a unretained property. Except the when the object is deallocated the weak pointer is automatically set to nil

```
@property (weak, nonatomic) IBOutlet UIButton *myButton;

@synthesize myButton;
```

Strong & Weak Explanation, Thanks to BJ Homer:

Imagine our object is a dog, and that the dog wants to run away (be deallocated).

Strong pointers are like a leash on the dog. As long as you have the leash attached to the dog, the dog will not run away. If five people attach their leash to one dog, (five strong pointers to one object), then the dog will not run away until all five leashes are detached.

Weak pointers, on the other hand, are like little kids pointing at the dog and saying "Look! A dog!" As long as the dog is still on the leash, the little kids can still see the dog, and they'll still point to it. As soon as all the leashes are detached, though, the dog runs away no matter how many little kids are pointing to it.

As soon as the last strong pointer (leash) no longer points to an object, the object will be deallocated, and all weak pointers will be zeroed out.

When we use weak?

The only time you would want to use weak, is if you wanted to avoid retain cycles (e.g. the parent retains the child and the child retains the parent so neither is ever released).

3.retain = strong

it is retained, old value is released and it is assigned retain specifies the new value should be sent
retain on assignment and the old value sent -release
retain is the same as strong.
apple says if you write retain it will auto converted/work like strong only.
methods like "alloc" include an implicit "retain"

```
@property (nonatomic, retain) NSString *name;

@synthesize name;
```

4.assign

assign is the default and simply performs a variable assignment
assign is a property attribute that tells the compiler how to synthesize the property's setter implementation
I would use assign for C primitive properties and weak for weak references to Objective-C objects.

```
@property (nonatomic, assign) NSString *address;

@synthesize address;
```