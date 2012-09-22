---
layout: post
title: "The Execution Engine currently segfaults"
description: ""
category: 
tags: []
---
{% include JB/setup %}
![Imgur](http://i.imgur.com/qiyVa.png)

	$ cflow --format=posix --omit-arguments flowy.c
	    1 main: int (), <flowy.c 200>
	    6     ft_open: <>
	   14     branch_start: void * (), <flowy.c 146>
	   15         filter: char ** (), <flowy.c 57>
	   22         grouper: <>

So essentially the `build_record_trees(…)` function segfaults when it calls `qsort_r(…)`. Need to investigate on it ...