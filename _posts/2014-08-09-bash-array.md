---
layout: post
title: "Bash Array"
description: "Play with Bash Array"
category: "shell"
tags: [array]
---
{% include JB/setup %}


### Declare array

```
arr[0]='hello'
arr[1]='hello'

declare -a arr=(1 2 3)
declare -a arr=('hello' 'Red hat')

arr=(1 2 3)

```

### get element by index

```
echo ${arr[0]}
```

### Print the Whole Array

```
echo ${arr[@]}
```

### length of the Array

```
echo ${#arr[@]}
```

### get all array index

in bash, you can define array that doesn't follow the sequence

```
arr[0]=1
arr[3]=3
```

to print the actual index of the array

```
echo ${!arr[@]}
```

### extract array by offset and length

```
arr=(this is an array)

echo ${arr[@]:1:2}

// is an

```

### extract with offset and length, for a particular element of an array

```
arr=(this is an array)

echo ${arr[0]:0:3}

//thi
```

### Search and replace in an array elements

```
Unix=('Debian' 'Red hat' 'Ubuntu')
echo ${Unix[@]/Ubunti/SCO Unix}

Debian Red hat SCO Unix
```

### add an element to array

```
arr=(this is an array)
arr=("${arr[@]}" I am Sean)
echo ${arr[4]}
//I
```

### remove an element from array

```
arr=(a b c)
unset arr[2]
echo ${arr[@]}
```

```
$ cat arraymanip.sh
Unix=('Debian' 'Red hat' 'Ubuntu' 'Suse' 'Fedora' 'UTS' 'OpenLinux');
pos=3
Unix=(${Unix[@]:0:$pos} ${Unix[@]:$(($pos + 1))})
echo ${Unix[@]}

$./arraymanip.sh
Debian Red hat Ubuntu Fedora UTS OpenLinux
```

### remove array elements using pattern

```
$ cat arraymanip.sh
#!/bin/bash
declare -a Unix=('Debian' 'Red hat' 'Ubuntu' 'Suse' 'Fedora');
declare -a patter=( ${Unix[@]/Red*/} )
echo ${patter[@]}

$ ./arraymanip.sh
Debian Ubuntu Suse Fedora
```

### copy array

```
array1=(a b c)
array2=("${array1[@]}")
```

### concat array

```
array1=(a b c)
array2=(d e f)
array3=("${array1[@]}" "${array2[@]}")
```

### deleting entire array

```
array=(...)
unset array
```

### load content of file line by line into array

```
filecontent=($(cat logfile))

for row in "${filecontent[@]}"; do
	echo $row
done

```

### for .. in loop

```
for in in "${array[@]}"; do
	echo $i
end

for i in "my" "name" "is" "sean"; do
...
done


for i in {a,b,c,d}; do
	echo $i
done

for i in {1..10}; do
	echo $i
done

# print all the file in the current location
for i in $(ls); do
	echo $i
done

```

### hash
in bash4, we can define hash like array

```
declare -A animals
animals=(["key"]="value")

Then use them just like normal arrays.  "${animals[@]}" expands the values, "${!animals[@]}" (notice the !) expands the keys. Don't forget to quote them:

get by key

echo "${animals["moo"]}"

loop the hash
for sound in "${!animals[@]}"; do echo "$sound - ${animals["$sound"]}"; done

```

reference:

Bash Brace Expansion
[http://www.linuxjournal.com/content/bash-brace-expansion](http://www.linuxjournal.com/content/bash-brace-expansion)

>Bash brace expansion is used to generate stings at the command line or in a shell script. The syntax for brace expansion consists of either a sequence specification or a comma separated list of items inside curly braces "{}". A sequence consists of a starting and ending item separated by two periods "..".
>Some examples and what they expand to:
>{aa,bb,cc,dd}  => aa bb cc dd
  {0..12}        => 0 1 2 3 4 5 6 7 8 9 10 11 12
  {3..-2}        => 3 2 1 0 -1 -2
  {a..g}         => a b c d e f g
  {g..a}         => g f e d c b a