---
layout: post
title: "v2.3: it's flexible"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Summary: (since after `v0.2`)

	>> git show v0.3
	
	tag v0.3
	Tagger: Vaibhav Bajpai <contact@vaibhavbajpai.com>
	Date:   Wed May 16 18:25:22 2012 +0200
	Commit 1c323fa66b9aaaad56ad7c4127b8d187eaf4ec0c
	
	* complete query is read at RUNTIME using JSON-C
	* JSON queries are generated using python scripts
	* glibc backtrace(...) to print the back trace on errExit(...)
	* gracefully exiting when trace cannot be read
	* gracefully exiting when JSON query cannot be parsed
	* branch thread returns EXIT_FAILURE if either stage returns NULL
	* branch thread returns EXIT_SUCCESS on normal exit
	* each stage proceeds only when previous returned results
	* flow-cat ... | flowy-engine $QUERY -	
