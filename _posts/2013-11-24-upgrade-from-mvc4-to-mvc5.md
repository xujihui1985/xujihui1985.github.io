---
layout: post
title: "Upgrade from MVC4 to MVC5"
category: "csharp"
tags: [MVC]
---
{% include JB/setup %}

First, make sure you are using Framework 4.5 or higher.

Second, go to Nuget management for solution and upgrade Microsoft ASP.NET MVC, Microsoft.ASP.NET Web API2, from 4.0 to 5.0, if you are using Entityframework, also upgrade Entityframework to 6.0
check your installed package, `Microsoft.AspNet.Web.Helpers.Mvc` has renamed to `ASP.NET.Web Helpers Library` remove it if you are reference it.

Check your package.config, make sure the main library has been upgraded.

open the web config, upgrade configsection the version, for example, upgrade webpages:version to 3.0.0.0, and upgrade bindingRedirect to latest version.
don't forget to upgrade the webconfig in Views folder


last, Unload the project, remove the guid start with `E3E379` end with `ABE47` this guid indicate this is a MVC project, and in MVC5 there is no need to identify this project an MVC project
and MVC and webform can be combine with each other, there is only ASP.NET prject
