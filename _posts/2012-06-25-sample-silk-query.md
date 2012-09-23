---
layout: post
title: "Sample SiLK Query"
description: ""
category: 
tags: []
---
{% include JB/setup %}

A sample `SiLK` query to imitate the functionality of `NFQL` is given
below.  Additional `SiLK` queries and example traces are available in
`examples/silk/`

HTTP TCP Session
----------------
- - -

Filter:

	>> rwfilter --sport=80 --proto=6 --pass=sport80.raw 100K.rwf.gz
	>> rwfilter --dport=80 --proto=6 --pass=dport80.raw 100K.rwf.gz	

Grouper:

	>> rwsort --fields=sIP,dIP dport80.raw | \ 
	   rwgroup --id-fields=sIP,dIP --summarize > dport80group.raw
	
	>> rwsort --fields=sIP,dIP sport80.raw | \
	   rwgroup --id-fields=sIP,dIP --summarize > sport80group.raw	

Group Filter:

	>> rwfilter sport80group.raw --pass=sport80gfilter.raw --packets=200-
	>> rwfilter dport80group.raw --pass=dport80gfilter.raw --packets=200-
	
Merger:

	>> rwmatch --relate=1,2 --relate=2,1 \
	   sport80gfilter.raw dport80gfilter.raw http-tcp-session.raw
