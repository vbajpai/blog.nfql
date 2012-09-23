---
layout: post
title: "More Resolved Issues"
description: ""
category: 
tags: []
---
{% include JB/setup %}
[Issue #68 &rarr;](https://github.com/vbajpai/mthesis-src/issues/68)

The `engine` now does NOT segfault when `srcaddr = dstaddr` is provided
at the grouper stage.  An example query script has been provided to
confirm this that finds all the TCP sessions between 2 endpoints.  A
test case for this query on both the traces has also been provided.

[Issue #84 &rarr;](https://github.com/vbajpai/mthesis-src/issues/84)

Simpler source organization by moving the headers from `include/` to `src/`. 

[Issue #93 &rarr;](https://github.com/vbajpai/mthesis-src/issues/93)

Previously 3/4 queries did not return any results on the example trace
file.  Therefore a new larger trace has been added at `examples/`.  All
the queries give results on this trace.   Additional `tcp-session`,
`dns` and `mdns` queries have also been added in `scripts/queries/`


[Issue #94 &rarr;](https://github.com/vbajpai/mthesis-src/issues/94)

Removed all trailing whitespaces from sources and headers

[Issue #95 &rarr;](https://github.com/vbajpai/mthesis-src/issues/95)

There was apparantely a segmentation fault when no grouper rules were
defined.  This was due to an attempt to free sorted/unique recordset
pointers which are not allocated when no grouper rules are defined.
