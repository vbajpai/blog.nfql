---
layout: post
title: "Regression Test Suite for the Execution Engine"
description: ""
category: 
tags: []
---
{% include JB/setup %}

A regression test-suite has been added in `tests/`.  The test `asserts`
the numbers of items in each stage of the pipeline for each query and
for each trace in `--debug` mode.  It also looks for `segfaults` that
might have occured in any particular query-trace combination.

To run the complete regression test-suite:

	[engine] $ make
	[engine] $ tests/regression.py [-v]
	............................................................
	----------------------------------------------------------------------
	Ran 60 tests in 32.533s
	
	OK
	
Regression tests can also be run individually on a specific example
query type:

	[engine] $ tests/test-query-http-tcp-session.py [-v]
	..........
	----------------------------------------------------------------------
	Ran 10 tests in 8.672s

	OK	
