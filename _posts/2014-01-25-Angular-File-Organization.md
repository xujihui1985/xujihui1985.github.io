---
layout: post
title: "File Organization in Angular"
description: ""
category: "Javascript"
tags: [Angular]
---
{% include JB/setup %}

file Organization in any application is important.

if you are just doing a demo, you can simply use the following structure

	App
		js
			controller.js
			service.js
			directive.js
			filters.js
		css
		partials
		img
		lib

the drawback of this sturcture is obvious. once your project grow bigger and bigger, these js file will become unmaintainable.


then the following structure can handle a middle size application

	App
		js
			controllers
				controller.js
				controller2.js
			services
				service1.js
			filters
				filter1.js
			directives
				directive1.js
				directive2.js
			partials
				view1.html	
				directive2View.html

it's much better


for a real big application it's better to organize the app by feature

	App
		authentication
			authCtrl.js
			authSvc.js
			authDirective.js
			authDirective.html
			auth.html
			authFilter.sj
		schedule
			scheduleCtrl.js
			scheduleSvc.js
			schdule.html
	css

order by feature then by type

	App
		schedule
			scheduleDetails
				controllers
					scheduleCtrl.js
				services
					scheduleSvc.js
				partials
					schedule.html


by feature then by sub feature  -- for huge application

	App
		schedule
			scheduleDetails
				scheduleCtrl.js
				scheduleSvc.js
				schedule.html
			scheduleTitle
				scheduleTile.js
				scheduleTile.html
				scheduleTileSvc.js
			common
				filter.js
				directives.js
