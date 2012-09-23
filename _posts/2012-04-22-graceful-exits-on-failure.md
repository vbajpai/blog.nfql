---
layout: post
title: "Graceful Exits on Failure"
description: ""
category: 
tags: []
---
{% include JB/setup %}
using glibc `backtrace(...)` to print the stack trace on `errExit(...)`
		
	>> bin/engine foo bar
	ERROR: open

	Stack Trace: 
	0   engine                              0x000000010bc891c2 print_trace + 34
	1   engine                              0x000000010bc893cb errExit + 395
	2   engine                              0x000000010bc898de read_param_data + 174
	3   engine                              0x000000010bc8c600 main + 80
	4   engine                              0x000000010bc6e054 start + 52

gracefully exiting when the trace cannot be read/parsed.

	>> bin/engine examples/query-http-tcp-session.json foo
	ERROR: open	

	Stack Trace: 
	...
	
	>> sudo cat /boot/vmlinuz-3.2.0-23-generic | \
       bin/engine examples/query-http-tcp-session.json -

	: ftiheader_read(): Warning, bad magic number
	: ftiheader_read(): failed
	ERROR: ftio_init(...)
	
	Stack Trace: 
	...

gracefully exiting when the `JSON` query cannot be read/parsed.

	>> bin/engine examples/trace.ft examples/trace.ft
	ERROR: json_tokener_parse_ex(...)

	Stack Trace: 
	...

branch thread returns `EXIT_FAILURE` if either stage returns `NULL`

	branch->filter_result = filter(...);	
	if (branch->filter_result == NULL)
	  pthread_exit((void*)EXIT_FAILURE);
	  
	branch->grouper_result = grouper(...);
	if (branch->grouper_result == NULL)
      pthread_exit((void*)EXIT_FAILURE);

	branch->gfilter_result = groupfilter(...);
	if (branch->gfilter_result == NULL)
      pthread_exit((void*)EXIT_FAILURE);

branch thread returns `EXIT_SUCCESS` on normal exit

	void *
	branch_start(void *arg) {	  
	  ...
	  pthread_exit((void*)EXIT_SUCCESS);
	}
