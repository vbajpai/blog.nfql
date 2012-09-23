---
layout: post
title: "Python Scripts to Automate Benchmarking"
description: ""
category: 
tags: []
---
{% include JB/setup %}
- - -

Requirements: Python 2.7+

To run the `NFQL` benchmarks:

	[engine] $ make
	[engine] $ sudo benchmarks/nfql.py bin/engine trace[s]/ querie[s]/
	benchmarking nfql ...
	executing: [bin/engine query-dns-udp trace-2012]:  1 ... 10 (0.874602 secs)
	executing: [bin/engine query-dns-udp trace-2009]:  1 ... 10 (0.028223 secs)
	...	
	
	
Example `NFQL` traces and queries are provided in `examples/` 
	
To run the `SiLK` benchmarks:	
	
	[engine] $ sudo benchmarks/silk.py trace[s]/ querie[s]/
	benchmarking silk ...
	executing: [silk http-octets trace-2009]:  1 ... 10 (0.135265 secs)
	executing: [silk http-octets trace-2012]:  1 ... 10 (0.175890 secs)
	...	
	
Example `SiLK` traces and queries are provided in `examples/silk/`
`SiLK` query files are simply bash commands separated by a delimiter.  A
HTTP TCP session `SiLK` query file for instance is shown below:

	rm -f /tmp/A.raw /tmp/B.raw /tmp/result.raw; \
	rwfilter --sport=80 --proto=6 --pass=stdout %s | \
	rwsort --fields=sIP,dIP | \
	rwgroup --id-fields=sIP,dIP --summarize | \
	rwfilter --input-pipe=stdin --pass=/tmp/A.raw --packets=200-; \
	rwfilter --dport=80 --proto=6 --pass=stdout %s | \
	rwsort --fields=sIP,dIP | \
	rwgroup --id-fields=sIP,dIP --summarize | \
	rwfilter --input-pipe=stdin --pass=/tmp/B.raw --packets=200-; \
	rwmatch --relate=1,2 --relate=2,1 \
	/tmp/A.raw /tmp/B.raw /tmp/result.raw;
