---
layout: post
title: "Complete Execution Engine Profiling"
description: ""
category: 
tags: []
---
{% include JB/setup %}
- Before:

		$ git checkout v0.1
		$ valgrind bin/flowy-engine bin/traces-long.ft bin/query.json
		
		==19000== 
		==19000== HEAP SUMMARY:
		==19000== in use at exit: 131,519 bytes in 1,182 blocks
		==19000== total heap usage: 2,609 allocs, 1,427 frees, 1,631,199 bytes allocs
		==19000== 
		==19000== LEAK SUMMARY:
		==19000== definitely lost: 6,912 bytes in 472 blocks
		==19000== indirectly lost: 0 bytes in 0 blocks
		==19000== possibly lost: 0 bytes in 0 blocks
		==19000== still reachable: 124,607 bytes in 710 blocks
		==19000== suppressed: 0 bytes in 0 blocks
		==19000== Rerun with --leak-check=full to see details of leaked memory
		==19000== 
		==19000== For counts of detected and suppressed errors, rerun with: -v
		==19000== Use --track-origins=yes to see where uninitialised values come from
		==19000== ERROR SUMMARY: 75 errors from 10 contexts (suppressed: 1 from 1)
		
- After:

		$ git checkout master
		$ valgrind bin/flowy-engine bin/traces-long.ft bin/query.json
		
		==19164== 
		==19164== HEAP SUMMARY:
		==19164== in use at exit: 20,228 bytes in 37 blocks
		==19164== total heap usage: 3,646 allocs, 3,609 frees, 1,647,767 bytes allocs
		==19164== 
		==19164== LEAK SUMMARY:
		==19164== definitely lost: 0 bytes in 0 blocks
		==19164== indirectly lost: 0 bytes in 0 blocks
		==19164== possibly lost: 0 bytes in 0 blocks
		==19164== still reachable: 20,228 bytes in 37 blocks
		==19164== suppressed: 0 bytes in 0 blocks
		==19164== Rerun with --leak-check=full to see details of leaked memory
		==19164== 
		==19164== For counts of detected and suppressed errors, rerun with: -v
		==19164== Use --track-origins=yes to see where uninitialised values come from
		==19164== ERROR SUMMARY: 300 errors from 6 contexts (suppressed: 1 from 1)	
	The `20kB` created and still living blocks are due to the following libraries: 

	- `dyld`: all 81 mallocs have these common calls: 
	
		![dyld](http://i.stack.imgur.com/KgXih.png)
	
		On GNU/Linux `dyld` would be replaced by `dlopen`. 
	
	    
	- `{libsystem_c, libsystem_notify, libdispatch}.dylib`: all 10 mallocs have these common calls:
	
		![localtime](http://i.stack.imgur.com/iIXOH.png)  
	
		 `localtime(...)` defined in `time.h` uses `tzset(...)` to initialize and return me 
		 a `struct tm*` which I shouldn't `free` myself because I did not allocate it. They are apparently not my fault.
		 The Mac OS X kernel will (like any Unix-like kernel) recover that memory when the process exits anyway.
