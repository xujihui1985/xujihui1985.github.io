---
layout: post
title: "generate maven project"
description: ""
category: "java"
tags: [maven]
---
{% include JB/setup %}


#### generate maven project

```
mvn archetype:generate -DgroupId={project-packaging}
   -DartifactId={project-name}
   -DarchetypeArtifactId=maven-archetype-quickstart
   -DarchetypeCatalog=local
   -DinteractiveMode=false
```
