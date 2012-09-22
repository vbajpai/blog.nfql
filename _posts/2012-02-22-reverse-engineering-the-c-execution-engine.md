---
layout: post
title: "Reverse Engineering the C Execution Engine"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Install Doxygen

	$ brew install doxygen

Initialize Doxygen

	[engine] $ doxygen -g

Edit the Doxyfile

	[...]

	# Extract documentation even from those elements you haven't yet commented.
	EXTRACT_ALL = YES	

	# Extract the relevant parts of the source and associate them with your description.
	INLINE_SOURCE = YES

	# Use GraphVIZ for class and collaboration diagrams.
	HAVE_DOT = YES

	# Generate a dependency graph for functions and methods.
	CALL_GRAPH = YES

	# Skip generating LaTeX sources for PDF.
	GENERATE_LATEX = NO
	
	# Show directory hierarchy in the documentation
	SHOW_DIRECTORIES = YES
	
	# Recursively search for input files.
	RECURSIVE = YES
	

Execute Doxygen

	[engine] $ doxygen Doxyfile

`flowy.c` Dependency Graph

![Imgur](http://i.imgur.com/QXVy4.png) 


`flowy.c` &rarr; `main(…)` Call Graph

![Imgur](http://i.imgur.com/uEOVw.png)

A similar Call Graph can also be generated using `cflow`

	$ brew install cflow

	$ cflow --format=posix --omit-arguments flowy.c
	    1 main: int (), <flowy.c 204>
	    2     ft_open: <>
	    3     calloc: <>
	    4     perror: <>
	    5     exit: <>
	    6     assign_fptr: <>
	    7     malloc: <>
	    8     pthread_attr_init: <>
	    9     pthread_create: <>
	   10     branch_start: void * (), <flowy.c 149>
	   11         filter: char ** (), <flowy.c 58>
	   12             malloc: <>
	   13             perror: <>
	   14             exit: <>
	   15             func: <>
	   16             realloc: <>
	   17         printf: <>
	   18         grouper: <>
	   19         free: <>
	   20         group_filter: struct group (), <flowy.c 92>
	   21         perror: <>
	   22         exit: <>
	   23         pthread_exit: <>
	   24     pthread_attr_destroy: <>
	   25     pthread_join: <>
	   26     printf: <>
	   27     free: <>
	   28     merger: <>

`struct flowquery` Collaboration Graph

![Imgur](http://i.imgur.com/tj99w.png)


[Complete Documentation &rarr;](http://www.vaibhavbajpai.com/documents/thesis/docs/docs-engine/html/index.html)