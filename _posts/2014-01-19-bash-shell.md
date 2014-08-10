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

`crontab -e` to edit the schedule

format

min hour dayofmonth month dayofweek command

`* arbitory number`
`*/min  every x min`
`, ` eg: 10,20,30 1 * * * /bin/echo "hello"
echo hello everyday at 1 aclock, at 10,20 and 30 mins 


## cut

```
cut -d':' -f1,2 /etc/passwd
seprate the /etc/passwd file by : and get the first and second column 
```
cut must supply file as the second argument, but if I want to cut the stdout from another command like `env` what I can do is use named pipeline [http://en.wikipedia.org/wiki/Named_pipe](http://en.wikipedia.org/wiki/Named_pipe "namedpipe")

```
mkfifo my_pipe
env > my_pipe &
cut -d'=' -f1 my_pipe
```

because command `env > my_pipe` will block the frontend, so must put it on the backend process or I need to startup another terminal

## tar

```
tar -czvf archive.gz /directory
tar -cjvf archive.tar.bz2 /directory   /smaller but slower
```

```
tar -tzf archive.gz  //just list the file in the tar
tar -xzvf archive.gz  //unzip the gz file
tar -xjvf archive.bz2  //unzip the bzip file
```

sometime I want to copy a large amount of files to another directory or to remote without create a archive file I can use `-`, for `-czf` `-` means, instead of create an archive file, write the tarred up filed onto `stdout`, for `-xzf` means, extracting a tarfile from `stdin`

```
tar -czf - /directory | tar -xzf -
```

### save file with sudo in vim

```
:w !sudo tee %
```
### vim a remote file
```
vim scp://username@host//path/to/somefile
scp is remote file copy program
```

### scp

```
Copy the file "foobar.txt" from a remote host to the local host
$ scp your_username@remotehost.edu:foobar.txt /some/local/directory

Copy the file "foobar.txt" from the local host to a remote host
$ scp foobar.txt your_username@remotehost.edu:/some/remote/directory

Copy the directory "foo" from the local host to a remote host's directory "bar"
$ scp -r foo your_username@remotehost.edu:/some/remote/directory/bar

Copy the file "foobar.txt" from remote host "rh1.edu" to remote host "rh2.edu"
$ scp your_username@rh1.edu:/some/remote/directory/foobar.txt \
your_username@rh2.edu:/some/remote/directory/

Copying the files "foo.txt" and "bar.txt" from the local host to your home directory on the remote host
$ scp foo.txt bar.txt your_username@remotehost.edu:~

Copy the file "foobar.txt" from the local host to a remote host using port 2264
$ scp -P 2264 foobar.txt your_username@remotehost.edu:/some/remote/directory

Copy multiple files from the remote host to your current directory on the local host
$ scp your_username@remotehost.edu:/some/remote/directory/\{a,b,c\} .
$ scp your_username@remotehost.edu:~/\{foo.txt,bar.txt\} .

```

### date convert datespan to date

```
date -d@12343345
```

### check ascii

```
man ascii
```

### ssh compage a local file with remote file

```
ssh user@host cat /path/to/remotefile | diff /path/to/localfile -
```

### du

```
du -sh /etc

-s for summary
h for human readable
```


### rsync

```
rsync -av /home/backup
rsync -ave ssh

mkdir /backup
rsync -av /*archive mode*/  /home/ /backup/   /*if there is trailing /, that means backup the content of the directory*/

rsync -av --delete /home/ /backup/  /*this also sync the deleted files*/
```