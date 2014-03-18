---
layout: post
title: "Bash Shell"
description: ""
category: "shell"
tags: [linux]
---
{% include JB/setup %}

If you are working on a system that does not automatically mount removable devices,you can use the following technique to determine how the removable device is named

This tells us the device name is `/dev/sdb` for the entire device and `/dev/sdb1` for the first partition on the device. As we have seen, working with Linux is full of interesting detective work!

Tip:Using the `tail -f /var/log/syslog` technique is a great way to watch what the system is doing in near real-time.

find:

	find dir -type f[f:file, d:directory ...] -name filename[wildcard] -exec command '{}' ';'

eg 
	
	find playground/ -type f -name 'file-A' -exec touch '{}' ';'
	find playground/ -type f -newer playground/timestamp | wc -l


tar:
	
	create tar
	tar cf playground.tar playground

	list the content of the archive
	tar tf playground.tar

	for a more detailed listing
	tar tvf playground.tar

	extract tar
	cd playground
	tar xf playground.tar
	
	tar xf archive.tar pathname
	

use tar and find
	
	find playground -name 'file-A' -exec tar rf playground.tar '{}' '+'

	find playground -name 'file-A' | tar czf playground.tgz -T -

	find playground -name 'file-A' | tar cjf playground.tbz -T -


There is a third way called a here documentor here script. A here document is an
additional form of I/O redirection in which we embed a body of text into our script and
feed it into the standard input of a command. It works like this:
**command << token
text
token**
where commandis the name of command that accepts standard input and  tokenis a string
used to indicate the end of the embedded text. We’ll modify our script to use a here document:

	#!/bin/bash 
	# Program to output a system information page 
	TITLE="System Information Report For $HOSTNAME" 
	CURRENT_TIME=$(date +"%x %r %Z") 
	TIMESTAMP="Generated $CURRENT_TIME, by $USER"
	cat << _EOF_ 
		<HTML> 
			<HEAD> 
				<TITLE>$TITLE</TITLE> 
			</HEAD> 
			<BODY> 
				<H1>$TITLE</H1> 
				<P>$TIMESTAMP</P>
			</BODY> 
		</HTML> 
	_EOF_


it's good habit: always use `-r` for `read`

stream redirection

	command 2> /dev/null

this will discards all errors

everything in linux are files, stdin stdout stderr null are three files in /dev  , they are special files

stream redirect

	>&2 == 1>&2
	cmd > logfile 2>&1  this will send stderr to stdout and since stdout is sent to logfile, botu stderr and stdout will be sent to logfile


case WORD in
	pattern1)
		code for pattern 1;;
	pattern2)
		code for pattern 2;;
	patternn)
		code for pattern n;;
esac


## function

redirection is allowed immediately after function definition

redirection will be executed when function is run

	fun(){}>&2

A command is a pipeline runs in a subshell

	ls | while read -r; do ((++cout)); done

cat <<END
	text to use as input goes here
END


${#var}: length of $var


${var#pattern} removes shortest match from begin of string
${var##pattern} removes longest match from begin of string
${var%pattern} removes shortest match from end of string
${var%%pattern} removes longest match from end of string

example with i="\users\sean\demo.txt"

${i#*/} ==> "users\sean\demo.txt"
${i##*/} ==> "demo.txt"
${i%.*} ==> remove the extension of the file "demo"
${i%/*} ==> return the directory of the file


Search and replace

${var/pattern/string}  substitute first match with string

${var//parttern/string} substitute all match with string

${var/%parttern/string} substitute the end with string


i="mytxt.txt"

echo ${i/%txt/jpg} ==> mytxt.jpg
echo ${i/txt/jpg} ==> myjpg.txt
echo ${i//txt/} ==> my.  //remove all the matches

## Default value

${var:-value}
will evaluate to "value" if var is empty or unset
${var-value}

assign a default value
${var:=value}

**declare notesdir=${NOTESDIR:-$HOME}**  
//this will check if NOTESDIR has value, if it has value it will return $HOME


## Conditional Expression Patterns

== is the same operator as =

	[[ $filename == *.txt ]]  // pattern match
	[[ $var == "[0-9]" ]] // string match

=~   does regular expresson matching

uses POSIX extended regular expressions

## end of options

is denoted by --

arguments after this will not be interpreted as options

for example

rm -- -l.txt   this will remote the file named -l.txt

	for i in *.txt; do touch -- $1; done

Good habit:

when you use a variable as an argument for a command
and the contents of the variable are not under your control use the end of option

## run script

	**nohup myscript & **  this will keep your script running when you exit the terminal session

nohub nice myscript &


suppose there is a long running script, and I want to execute the script and log out from the system, and check the result tomorrow.

first I need to redirect the result to logfile

nohup longrunningscript > log \&


## schedule the task

At 
will execute your script at a specific time
at -f myscript noon tomorrow

Cron
will execute your script according to a schedule
