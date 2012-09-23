---
layout: post
title: "Execution Engine Verbosity Levels"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Without verbosity or debugging levels the echo is minimalistic with only the end-result.  
Offcourse, since the ungrouper is yet to be implemented, the run results in no echo.

	[engine]$ bin/flowy-engine bin/traces-long.ft bin/query.json
	
Verbosity Levels:	

The engine is interactive to help you choose the right switches with required options.

	[engine]$ bin/flowy-engine bin/traces-long.ft bin/query.json --verbose
	flowy-engine: option `--verbose' requires an argument

	[engine]$ bin/flowy-engine bin/traces-long.ft bin/query.json --verbose
	ERROR: valid verbosity levels: (1-3)
	
`--verbose=1`:  shows the results of each stage of the pipeline

	[engine]$ bin/flowy-engine bin/traces-long.ft bin/query.json --verbose=1
	No. of Filtered Records: 166
	...
	No. of Groups: 32 (Aggregations)
	...
	No. of Filtered Groups: 5 (Aggregations)
	...
	No. of Merged Groups: 3 (Tuples)
	...
	
`--verbose=2`: additionally also shows the intermediate results of each stage of the pipeline

	[engine]$ bin/flowy-engine bin/traces-long.ft bin/query.json --verbose=1
	No. of Filtered Records: 166
	...
	No. of Sorted Records: 166
	...
	No. of Unique Records: 32
	...
	No. of Groups: 32 (Verbose Output)
	...
	
	... 0     216.137.61.203  80    0     192.168.0.135 ... 
	... 0     216.137.61.203  80    0     192.168.0.135 ... 
	
	... 0     8.12.214.126    80    0     192.168.0.135 ...
	... 0     8.12.214.126    80    0     192.168.0.135 ...
	
	No. of Groups: 32 (Aggregations)
	...
	
	... 0     216.137.61.203  80    0     192.168.0.135 ... 
	... 0     8.12.214.126    80    0     192.168.0.135 ...
	
	No. of Filtered Groups: 5 (Aggregations)
	...
	No. of (to be) Matched Groups: 15 
	...
	
	... 0     192.168.0.135   0     0     204.160.123.126 80    ...
	... 0     87.238.86.121   80    0     192.168.0.135   0     ...
	
	... 0     192.168.0.135   0     0     216.46.94.66    80    ... 
	... 0     216.46.94.66    80    0     192.168.0.135   0     ... 	
	
	No. of Merged Groups: 3 (Tuples)
	...
	
	... 0     192.168.0.135   0     0     216.46.94.66    80    ... 
	... 0     216.46.94.66    80    0     192.168.0.135   0     ... 	
	
`--verbose=3`: additionally also prints the original flow-record trace.

	#
	# mode:                 normal
	# capture hostname:     ihp.jacobs.jacobs-university.de
	# capture start:        Fri, 02 Mar 2012 00:00:01 +0100
	# capture end:          Fri, 02 Mar 2012 00:05:00 +0100
	# capture period:       299 seconds
	# compress:             on
	# byte order:           little
	# stream version:       3
	# export version:       5
	# lost flows:           0
	# corrupt packets:      0
	# sequencer resets:     0
	# capture flows:        430
	#
	
	... Sif SrcIPaddress    SrcP  DIf   DstIPaddress    DstP  ... 
	...  0  10.50.244.59    138   0     10.50.255.255   138   ... 
	
	
	No. of Filtered Records: 166
	...
	No. of Sorted Records: 166
	...
	No. of Unique Records: 32
	...
	No. of Groups: 32 (Verbose Output)
	...
	No. of Groups: 32 (Aggregations)
	...
	No. of Filtered Groups: 5 (Aggregations)
	...
	No. of (to be) Matched Groups: 15 
	...
	No. of Merged Groups: 3 (Tuples)
	...
