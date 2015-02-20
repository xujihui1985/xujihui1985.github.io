---
layout: post
title: "python toolkit"
description: ""
category: "python"
tags: [essentialtools]
---
{% include JB/setup %}


### pip

install pip

on mac, we can use homebrew to install pip `brew intall pip`, python3 is installed with pip, so you don't need to install pip, for python2.x, you can install by download `get-pip.py`

```
sudo python get-pip.py
```

update pip

```
pip install -U pip
```

install package

```
pip install requests
pip install requests=1.0
pip install 'requests>=2.1'  note '' is required
```

removing a package

```
pip uninstall requests
```

#### pip list
#### pip show requests
#### pip search query

#### pip freeze

```
pip freeze > requirements.txt

```

save your project's dependencies in requirements.txt

```
pip install -r requirements.txt
```

install all packages listed in requirements.txt


### virtualenv

python install package systemwides, so we need virtualenv to encapslate the package into project scope, basically
we will create a virtualenv for every project

### installation

```
sudo pip install virtualenv
```

### usage

create a new virtualenv in directory `.virtualenvs`

```
mkdir ~/.virtualenvs
cd ~/.virtualenvs

virtualenv newproject

// activate virtualenv

source newproject/bin/activate  //activate is a shell script

//now install package, you don't need sudo anymore, all the package are installed in the virtualenv

pip install pylint 
```

#### install with interpreter

```
virtualenv py3project --python=python3.4
```

deactivate the virtualenv

`deactivate` directly

### best prectise

keep your virtual envrionments separate from your projects

prjects contain your source code under version control
virtialenv contain libraries, tools, python interpreter, keep them in a directory like ~/.virtualenvs

### virtualenvwrapper

workon: lists enviroments

workon projectname: Activate enviroment, switch to project

mkvirtualenv, rmvirtualenv: create, remove virtualenv

setvirtualenvproject: bind the virtual env to the project

**system-wide install**

```
sudo pip intall virtualenvwrapper
edit ~/.profile

source /usr/local/bin/virtualenvwrapper.sh
export PROJECT_HOME=$HOME/dev
```

default virtualenv location: `~/.virtualenvs`

usage

```
mkvirtualenv myenv    # create a new virtualenv

cd ~/work/projects/myproject  # go to my project

setvirtualenvproject  # this couple my project with virtualenv

workon myenv      # jump to myproject and activate the virtualenv
```

```
mkproject sample

this create new virtualenv
create project
bind the env to the project
and move to the project
```

### debug

```
import pdb
pdb.set_trace()
```

to enable debug in python, use pdb module

```
def main():
	local_variable= 'test'
	import pdb
	pdb.set_trace()
	for _ in range(10):
		print local_variable

if __name__ == '__main__':
	main()
```

### pdb command

`l` : list the current cursor
`n` : next step over
`s` : step into
`h` : help
`w` : current stacktrace
`c` : continue
`b` : set break point  `b my_module:58` this will set breakpoint on line 58 in file my_module


### sphinx  document generator

installation:

`pip install sphinx`

usage:

`sphinx-quickstart`
