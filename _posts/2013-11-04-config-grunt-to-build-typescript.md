---
layout: post
title: "How to auto build typescript file with grunt and node"
description: ""
category: "Javascript"
tags: [grunt,typescript]
---
{% include JB/setup %}

## How to auto build typescript file with grunt and node

### install nodejs

### install typescript with admin or sudo

	npm install -g typescript


### install grunt-cli with admin

	npm install -g grunt-cli

### install grunt-init
	npm install -g grunt-init

### download grunt-init template or create yourself

	git clone https://github.com/gruntjs/grunt-init-gruntfile.git ~/.grunt-init/grunt-init-gruntfile

### go to project folder

### create package.json

	npm init

### then create gruntfile
 	
	grunt-init grunt-init-gruntfile


### install grunt-typescript  `--save-dev` means depancence will not be installed under production you need have package.json created first
	
	npm install grunt-typescript --save-dev


### config grunt-typescript

	  grunt.loadNpmTasks('grunt-typescript');

	  // Project configuration.
	  grunt.initConfig({
	    // Task configuration.
	     typescript: {
	      base: {
	        src: ['TypeScriptFundamental/scripts/*.ts'],
	        dest: 'TypeScriptFundamental/output',
	        options: {
	          module: 'amd', //or commonjs
	          target: 'es5', //or es3
	          base_path: 'TypeScriptFundamental/scripts',
	          sourcemap: false,
	          fullSourceMapPath: false,
	          declaration: false,
	        }
	      }
	    }

	  });

when you run grunt typescript, you will see your javascript generated in output folder

### then install grunt watch

	npm install grunt-contrib-watch --save-dev

### config watch

	/*global module:false*/
	module.exports = function(grunt) {

	  // These plugins provide necessary tasks.
	  grunt.loadNpmTasks('grunt-typescript');
	  grunt.loadNpmTasks('grunt-contrib-watch');
	  // Project configuration.
	  grunt.initConfig({
	    // Task configuration.
	     typescript: {
	      base: {
	        src: ['TypeScriptFundamental/scripts/*.ts'],
	        dest: 'TypeScriptFundamental/output',
	        options: {
	          module: 'amd', //or commonjs
	          target: 'es5', //or es3
	          base_path: 'TypeScriptFundamental/scripts',
	          sourcemap: false,
	          fullSourceMapPath: false,
	          declaration: false,
	        }
	      }
	    },
	    watch: {
	      files: ['TypeScriptFundamental/scripts/*.ts'],
	      tasks:['typescript']
	    }
	  });
	
	
	
	  // Default task.
	  grunt.registerTask('default', ['watch']);
	
	};

done.