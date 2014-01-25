---
layout: post
title: "unit test in javascript"
description: ""
category: "javascript"
tags: [grunt]
---
{% include JB/setup %}


	module.exports = function(grunt) {
	
	
		grunt.registerTask('depTask','this task should run before test task', function(){
			console.log('finished');
			
		});
		
		grunt.registerTask('testTask','this is a test Task', function(target) {
			this.requires('depTask');
			console.log(this);
		});
		
		grunt.registerTask('run', '',function(){
			grunt.task.run('depTask');
			grunt.task.run('testTask');
		});
		
		grunt.registerTask('default', ['run']);
	
	};