---
layout: post
title: "create grunt plugin"
description: ""
category: "javascript"
tags: [grunt]
---
{% include JB/setup %}


## install grunt-init globally

```javascript
	npm install -g grunt-init
```

## cloning the package into ~/.grunt-init/gruntfile(*nux) or %userprofile%/.grunt-init/gruntfile(windows)

```javascript
	git clone [url for init.git] ~/.grunt-init/gruntfile
```

###using the scaffolding

```javascript
	grunt-init gruntfile
```


###create inline task

```javascript
var fs = require('fs');
grunt.initConfig({
	'checkFileSize':{
		'options': {
			folderToScan: "./files"
		}
	}
});	
grunt.registerTask('checkFileSize', 'Task to check file Size', function(debug){ //the debug parameter is passed into callback in the commandline
	

// Merge task-specific and/or target-specific options with these defaults.
	var options = this.options({
		folderToScan: ''
	});
	
	if(this.args.length !==0 && debug !== undefined){
		grung.log.writeflags(options, 'Options');
	}

	grunt.file.recurse(options.folderToScan, function(abspath, rootdir, subdir, filename){
		if(grunt.file.isFile(abspath)){
			var stats = fs.statSync(abspath);
			var asBytes = stats.size / 1024;
			grunt.log.writeln("Found file %s with size of %s kb", filename, asBytes);
		}
	});
});

grunt checkFileSize:true
in that way, debug is passed as true

```

### create custom grunt plugins

clone the grunt-init-gruntplugin repo into ~/.grunt-init folder

```
git clone https://github.com/gruntjs/grunt-init-gruntplugin gruntplugin
```
create a project folder
```
grunt-init gruntplugin
```

### multitask

```javascript
checkFileSize: {
	options: {  //globle options
		debug:true
	},

	dev: {
		src: ['./files']
	},
	prod: {
		options: { // overrides options
			debug: false		
		},

		files: [
			{src: './files'},
			{src: './files'}
		]
	}

}
```

```

	function dumpDebugInfomation(target) {
		var options = target.options();
		var files = target.files;
		if(!options.debug) {return;}
		grunt.log.writeln('running target: '+target.target);
		grunt.log.writeflags(options, 'Target Options');
		target.filesSrc.forEach(function(filePath){
			grunt.log.writeln('Configured Folder: '+filePath);
		});
	}
	grunt.registerMultiTask('checkFileSize', 'description', function(){
		dumpDebugInfomation(this);
	})
```