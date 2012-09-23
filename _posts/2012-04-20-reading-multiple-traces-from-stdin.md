---
layout: post
title: "Reading Multiple Traces from stdin"
description: ""
category: 
tags: []
---
{% include JB/setup %}
The engine can now read traces from `stdin`.  
This means, now we can concatenate multiple traces using `flow-cat` and then pipe them in to the engine for processing - 

	flow-cat $TRACE[s] | bin/engine $QUERY -
	
A typical query to find all the HTTP (over TCP) sessions for April resulted in 285 streams and took around 2 minutes.

	>> time flow-cat /Netflow/ft-data/2012/2012-04 | \
       bin/engine examples/query-http-tcp-session.json -

	No. of Streams: 285
	--------------- 
	...
