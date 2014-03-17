---
layout: post
title: "Go Basic"
description: ""
category: "Go"
tags: [Go]
---
{% include JB/setup %}

## IDE

eclipse with Go plugin, IntelliJ with Go plugin

## Go Type

string, int, bool, float ....
1. Array
2. Slice
3. Struct
4. Pointer
5. Function
6. Interface
7. Map
8. Channel


# Basic Syntax

define a variable

	var message string
	message = "Hello Go World"

**go is special, if you declared a variable without use it. you will get an error.**

define multiple variable

	var a, b, c int
	fmt.Println(a,b,c)


define a variable and initialize value

	var msg string = "hello"
	var a,b,c int = 1,2,3

type infer, don't need to specify the type

	var msg = "hello"
	var a,b,c = 1,false,3


short cut for declare and initialize

	message := "hello"
	a,b,c := 1,2,3


# Pointer

in go, string pointer can only point to a string while an int pointer can only point to a int variable

parameter pass into function is pass by value by default, it is a copy of the variable, if a pointer pass into the function, the pointer it self is a copy but it still point to the original variable, this is like c#


define a pointer

	msg := "hello"
	var message *string = &msg

now message is a pointer to variable msg
print the message will print the address of the msg variable

in order to print the value of the msg, use `*` to dereference the variable

	fmt.Println(*message)

	*message = "hi"
	fmt.Print(msg)  //print hi

if assign a value to pointer, it will change the reference value of the variable


# No Class in Go

yes there is no Class


# User Defined Types

define a user defined types that behave like string

	type Salutation string

we just defined a type called `Salutation` that same as string

instead of use a string, let's create something useful

	type Person struct {
		firstName string
	    lastName  string
	}


use a type

	var s = Person{"sean", "xu"}
	var sean = Person{lastName:"xu", firstName:"Sean"}
	s.name = "Anna"

**in go, Capitalize means Public, in this example Person is a public type.
visiblity is base on the nameing conversion**

	
# Constants

define a constant

	const (
		PI = 3.14
		Language = "GO"
	)

	fmt.Print(PI)
	fmt.Print(Language)


# functions

function in Go can have multiple return values
can use like any other type
closure

## basic function declaration

	func SayHello(person Person) {
		fmt.Println(person.firstName)
		fmt.Println(person.lastName)
	}


## function with return value

	func CreateMessage(firstName, lastName string) string {
		return firstName + " "+lastName
	}

`firstName, lastName string` is a shortcut for define two string parameters, and this function has a return value of string. 


## multiple returns

	//multiple return value
	
	func CreateMessageWithMultipleReturn(firstName, lastName string) (string, string) {
		return firstName + " "+lastName, "Hey!" + firstName
	}


how to use

	hellomsg, alter := CreateMessageWithMultipleReturn(sean.firstName,sean.lastName)
	fmt.Println(hellomsg,alter)

if we just need `hellomsg` and don't care what `alter` return we can use `_` to ignore the unused value

	hellomsg, _ := CreateMessageWithMultipleReturn(sean.firstName,sean.lastName)
	fmt.Println(hellomsg,alter)


## named return value

	func CreateMessageWithNamedReturn(firstName, lastName string) (message string, alternate string){
		message = firstName + " "+lastName
		alternate ="Hey!" + firstName
		return
	}

sometimes it will be confused when there are multiple return values, then we can use named return value

## variadic functions

this is same as `params` in csharp

//Variadic functions

	func CreateMessageWithVariadic(name string, message ...string) string {
		len := len(message)
		return name + " " + strconv.Itoa(len)
	}

in go convert int to string need another package
`strconv`

need to import that package

	import (
		"fmt"
		"strconv"
	)

	strconv.Itoa(len)
	strconv.Atoi(str)  // convert str to int

## pass a function

in go function can be passed as a parameter, same as function pointer in c and delegate in csharp, or call back in javascript

	func SayHello(do func(string))

here `do` is the parameter name, and its' type is func and has one string parameter with no return value

	func GreetWithFunction(do func (string)) {
		do("hello")
	}
	
	func Print(message string) {
		fmt.Println(message)
	}
	
	func PrintLn(message string) {
		fmt.Println(message)
	}

	GreetWithFunction(Print)


## declaring a function type
	
	// this is a function take string as parameter return nothing
	type Printer func(string)
	// this is a function take string as parameter return two string 
	type Printer func (string) (string,string)

then we can replace the previous demo with
	func GreetWithFunction(do Printer) {
		do("hello")
	}

## Closures

we can also return an function from another function. and the inner function can still access to the variable of the outer function even the outer function has finished, this is called Closures
	
	func PrintCustom(custom string) func(string) (string,string) {
		return func(s string) (string ,string) {
			fmt.Println(s + custom)
			return "1","2"
		}
	
	}

	func GreetWithFunction(do Printer) {
		do("hello")
	}

	GreetWithFunction(PrintCustom("sean"))

	type Printer func(string) (string ,string)


we can also replace the `func` return type to `Printer`

	func PrintCustom(custom string) Printer {
		return func(s string) (string ,string) {
			fmt.Println(s + custom)
			return "1","2"
		}
	
	}

## flow control

if statement

syntax

if (optional statment); condition {
	block
}

	if isBool {
		// do sth
	}

	if isBool := true; isBool {
		// do sth
	}

	isFormal := true
	if prefix := "Mr "; isFormal {
		//do sth
	} 

in this example, variable `prefix` will only exists in the if block.


## switch case

in go **cases can be expressions**

switch basic

in go, switch by default will not fall through, that means, statement will break once the case expression matched.
if we really need fall through, we can use `fallthrough` keyword
	
	func GetPrefix(name string) (prefix string) {
		switch name {
		case "Bob":
			prefix = "Mr "
		case "Joe", "Amy":
			prefix = "Dr "
		case "Mary":
			prefix = "Mrs "
		default:
			prefix = "Dude "
	
		}
		return
	}

	// fall through demo
	func GetPrefix(name string) (prefix string) {
		switch name {
		case "Bob":
			prefix = "Mr "
			fallthrouth
		case "Joe":
			prefix = "Dr "
		case "Mary":
			prefix = "Mrs "
		default:
			prefix = "Dude "
	
		}
		return
	}


switch on nothing, it's really flexible

	func SwitchOnNothing(name string) (prefix string) {
		switch {
		case name == "Bob":
			prefix = "Mr "
		case len(name) == 10:
			prefix = "Dr "
		default:
			prefix = "Dude "
		}
		return
	}

switch on type

	func SwitchOnType(x interface {}) {
		switch t := x.(type) {
		case int:
			fmt.Println("int")
		case string:
			fmt.Println("string")
		default:
			fmt.Println("unknow")
		}
	}

## loop

there is only one loop that is `for`
basic for loop

	func ForLoop(times int) {
		for i := 0; i< times; i++ {
			fmt.Println(i)
		}
	}

	func WhileLoop(times int) {
		i := times
		for i < times {
			fmt.Println(i)
			i++
		}
	}

infinite loop

	for {
		fmt.Println("true")
	}

end the loop with `break`

	for {
 		if true {
			break;  // or continue
		}
		fmt.Println("true")
	}
	

## Range

Range can used on

Array or slice
String
Map
Channel

basic usage

	func RangeDemo2() {
		slice := []greeting.Person{
			{"sean", "xu"},
			{"anna", "shen"},
		}
		Greet2(slice)
	}
	
	func Greet2(person []greeting.Person) {
		for _, s := range person {
			fmt.Println(s.FirstName)
		}
	}


## Map

	prefixMap := make(map[string]string)

	var prefixMap map[string]string
	prefixMap = make(map[string]string)

the `string` in side `[]` is the type of key, and the string after `[]` is the type of value

	prefixMap["Bob"] = "Mr "
	prefixMap["Joe"] = "Dr "

this will insert the value into the map

shorthand to create a map, notice we need the last `,`, or it doesn't compile

	prefixMap := map[string]string {
		"Bob" : "Mr ",
		"Joe" : "Dr ",
	}

delete an element from map
	
	delete(prefixMap, "Bob")

check element exists in the map

	if value, exists := prefixMap[name]; exists {
		return value
	}

the value is the `value` of the key in the map, the exists is a bool value, indicate whether the key is in the map. `prefixMap[name]` actually return two value


## Array

an Array is a collection has a fix size

	var arr [3]int  //define an array

array is fixed size
no initialization
**not a pointer**

## Slice

	var s []int = make([]int,3,5)

the slice is basically a pointer to an array, it is a wrapper to array, the syntax to initilize slice is quite similar as array, besides you don't need to specify the size.

slice is a pointer or we can say it is a reference type 

a slice is fixed size
use make to initialize otherwise is nil
points to an array


		var s []int
		s = make([]int,3)
		s[0] = 1
		s[1] = 10
		s[2] = 500
		s[3] = 23  // will be error because the length and cap of slice is 3

		//short
		s := []int { 1,2,3 }
		
		slice := []greeting.Person{
			{"sean", "xu"},
			{"anna", "shen"},
		}

		slice = slice[0:1]
		slice = slice[:2]
		slice = slice[1:]

to slice a slice we use the syntax `slice[0:1]` mean that from 0 to 1, and include 0 but exclude 1 (>=0 <1)
that will return {"sean", "xu" } `slice[:2]` means take to index 2 but don't incluce 2, `slice[1:]` means take from index 1 and include 1

append a slice return a new slice

	slice = append(slice, greeting.Person{"jack","xu"}

and we can also append another slice to it

	slice = append(slice, slice...}

deleting from a slice

	slice = append(slice[:1],slice[2:]...)

this will delete the second item from the slice

		func SliceDemo() {
			slice := []int {1,2,3,4,5}
		
			slice = append(slice[:2],slice[3:]...)
		
			for _, v := range slice {
				fmt.Print(v)
			}
	
		}


## Method

there is no class in go, method in go is append to type

first, we need have a `named type`

eg: we are going to add a method IntruduceMySelf under a slice of Person struct,
first, we need to create a named type `People`, which is a slice of Person

then use the method syntax

	type People []Person
	
	func (people People) IntruduceMySelf() {
		for _, p := range people {
			fmt.Println("hi my name is "+p.FirstName)
		}
	}

usage

	people := greeting.People {
		{"Sean", "xu"},
		{"Anna", "shen"},
	}

	people.IntruduceMySelf()

## Method with a Pointer Receiver

parameter is passed by value, so if you want to change the value of the type your method append, you must use `Pointer Receiver`

	func (person *Person) Rename(newName string) {
		person.FirstName = newName
	}

usage

	people := greeting.People {
		{"Sean", "xu"},
		{"Anna", "shen"},
	}

	people[0].Rename("Jack")


## interface

in go, interface is just a contact, and as long as your object match the contact, that means your type implements this interface, you don't have to explicit implement the interface

create interface

	type Renamable interface {
		Rename(newName string)
		CanRename() bool
	}

	func (person *Person) Rename(newName string) {
		person.FirstName = newName
	}
	
	func (person *Person) CanRename() bool {
		return true
	}
	
we are creating an interface `Renamable` that has two methods, Rename and CanRename, then we add two methods on the type `Person`

usage:

	func RenameToSth(r greeting.Renamable){
		if canRename := r.CanRename(); canRename {
			r.Rename("Jack")
		}
	}

then we could create a function that the interface Renameable as parameter, and we could pass Person to this function, notice we must pass the pointer of Person, that is the address of Person,

	people := greeting.People {
		{"Sean", "xu"},
		{"Anna", "shen"},
	}

	RenameToSth(&people[0])

## Empty interface

	interface{}	

if we use an empty interface as parameter, that means you can pass any type of parameter to that function

	func GetType(x interface{}) {
		switch t := x.(type) {
		case int:
			fmt.Println("int")
		default:
			fmt.Println("invalid type",t)
		}
	}

	
usage:
	GetType(people[0])
	GetType(1)

example of use interface

	func (person *Person) Write(p []byte) (n int, err error) {
		s := string(p)
		person.Rename(s)
		n = len(s)
		err = nil
		return
	}

	fmt.Fprintf(&people[0], "the count is %d", 10)
	fmt.Println(people[0].FirstName)

this is not a good example, but you can understand from the example
here, the first argument of fmt.Fprintf is a Writer interface that has only one method Write.
and we implement that function under Person, so we can pass person to fmt.Fprintf,
and that means, as long as your object implment the `Write` method and the argument and return type is same as the interface, then you can pass your object to that function, that's the power of interface

## Concurrency

Goroutines

syntax
	
	go function

goroutine kickoff a new thread to run the function, like `async` in c#

## Channels

in go, there is no such concept of lock to sync the data between two thread, instead, go use channel to sync data

syntax
	
	done := make(chan,bool)

this create a channel called done that can only send bool in the channel

	go func() {
		doSth()
		done <- true
	}()

to pass the channel to the goroutine function, we need to change the signature of the method which we don't want to do that.
luckily, we can use anonymous function to pass the channel to the function.

	done <- true

this syntax means to put `true` into the channel

	<- done

this will make our main thread wait until the `done` channel fulfilled 

## buffered channel

by default, there can only be one item in the channel at one time, and you can only put the item into channel when there is no item in the channel

to define buffered channel

	done := make(chan bool, 2)

by this way we define a buffered channel done with 2 buffer, we can put two item into this channel at one time

	close(c)

close a channel and tell the consumer that it has put all the data into the channel

example

	
	func (people *People) ChannelPeople(c chan Person) {
		for _, p := range *people {
			c <- p
		}
		fmt.Println("channel done")
		close(c)
	}

	people := greeting.People {
		{"Sean", "xu"},
		{"Anna", "shen"},
	}

	peopleChannel := make(chan greeting.Person);
	go people.ChannelPeople(peopleChannel)
	for p := range peopleChannel {
		fmt.Println(p.FirstName)
	}

	result: Sean
			Anna
			channel done

	peopleChannel := make(chan greeting.Person, len(people));
	go people.ChannelPeople(peopleChannel)
	for p := range peopleChannel {
		fmt.Println(p.FirstName)
	}

	result: channel done
			Sean
			Anna

the keyword `range` will block the channel, when I use non-buffered channel, eachtime item was put into the channel, it will be read from another side.

when use buffered channel, `range` will wait the channel until it is full

## Select statement

	
 