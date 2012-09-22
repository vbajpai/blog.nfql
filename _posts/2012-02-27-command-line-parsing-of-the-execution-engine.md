---
layout: post
title: "Command Line Parsing of the Execution Engine"
description: ""
category:
tags: []
---
{% include JB/setup %}
Command Line Parsing of the Execution Engine

- Renamed `base_header.h` to `base.{h,c}` (1)
- Defined global variables (command line flags) used in `base.{h,c}` (1)
- Moved `usageError(…)` to `error_functions` (2)
- using `debug` and `absolute` global flags now instead of macros (3)
- using `getopt_long(…)` for command line parsing (3)

	- print usage on insufficient arguments

			[engine]$ ./flowy-engine
			Usage: ./flowy-engine $TRACE

	- track invalid options

			[engine]$ ./flowy-engine -x traces.ft
			flowy-engine: invalid option -- x

	- `--debug` flag: `flow-print` functionality

			[engine]$ ./flowy-engine --debug traces.ft
         #
         # mode:                 normal
         # capture hostname:     melnikovkolya-laptop
         # capture start:        (null)  
         # capture end:          Mon Feb 12 18:29:48 2418
         # capture period:       299 seconds
         # compress:             on
         # byte order:           little
         # stream version:       3
         # export version:       5
         # lost flows:           0
         # corrupt packets:      0
         # sequencer resets:     0
         # capture flows:        132
         #

        ... SrcIPaddress    SrcP  DIf   DstIPaddress    DstP  ...

        ... 10.50.249.229   138   0     10.50.255.255   138   ...
        ... 10.50.223.240   138   0     10.50.255.255   138   ...
  
	- <s>`--absolute` flag: can be used to restrict functionality</s> dropped, see (4)

		  	[engine]$ ./flowy-engine --absolute traces.ft

			No. of Filtered Records: 4

			...	SrcIPaddress    SrcP  DIf   DstIPaddress    DstP  ...

			...	10.50.239.4     39788 0     209.85.135.19   80    ...
			...	10.50.239.4     40339 0     81.19.80.14     80    ...
			...	10.50.239.4     46499 0     209.85.135.19   80    ...


Resources -

(1) [`base` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/base_8h.html)  
(2) [`error_functions` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/error__functions_8h.html)  
(3) [`flowy` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/flowy_8c.html)  
(4) [resolved grouper segfaults &rarr;](http://mthesis.vaibhavbajpai.com/post/19179727822/resolved-grouper-segfaults)  
