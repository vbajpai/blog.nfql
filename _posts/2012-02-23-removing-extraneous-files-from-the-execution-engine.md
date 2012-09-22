---
layout: post
title: "Removing Extraneous Files from the Execution Engine"
description: ""
category: 
tags: []
---
{% include JB/setup %}

The engine currently uses comparison functions as the default method for efficient rule processing. These functions are auto-generated by `fun_gen.py` to `auto_comps.{h,c}` and later called by `grouper_fptr.c`. 

The same python script also generates `auto_switch.c` as an alternative method which can be later called by `grouper_switch.c`. 
Since, this method is not currently used by the program. I have removed it from the `HEAD` since it can anyway be later retrieved from the `git` history.

	[engine] $ rm auto_switch.c grouper_switch.c

The engine also performs a divide and conquer approach for fast relative comparsions. The code was initially prepared as a separate compilation unit for testing purposes and was later incorporated into the engine. The separate units are therefore no longer needed.

	[engine] $ rm treesearch.c treesearchmain.c

The `filter` stage of the processing pipeline was moved out into a separate compilation unit to allow benchmarking with contemporary tools like `flow-tools` and `nf-dump`. This should be done by passing an `--absolute` switch to the engine, which reduces its functionality down to only the `filter` stage. As such, I am removing the separate units.

	[engine] $ rm filter.c

The references to these units were also removed from the `Makefile`.
