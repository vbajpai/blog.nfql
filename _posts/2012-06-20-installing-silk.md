---
layout: post
title: "Installing SiLK"
description: ""
category: 
tags: []
---
{% include JB/setup %}
[SiLK &rarr;](http://tools.netsa.cert.org/silk/)

> SiLK is a suite of network traffic collection and analysis tools developed and maintained by the CERT Network Situational
> Awareness Team (CERT NetSA) at Carnegie Mellon University to facilitate security analysis of large networks. The SiLK tool suite 
> supports the efficient collection, storage, and analysis of network flow data, enabling network security analysts to rapidly 
> query large historical traffic data sets.

Since SiLK is the only tool that comes even remotely closer to the
functionality offered by the NFQL, we used it as a reference to compare
the performance of the execution engine.

Download and Install SiLK
-------------------------
- - -


Download [SiLK &rarr;](http://tools.netsa.cert.org/silk/)

	>> wget http://tools.netsa.cert.org/releases/silk-2.4.7.tar.gz
	>> sha1sum silk-2.4.7.tar.gz | grep 2ff0cd1d00de70f667728830aa3e920292e99aec

Install SiLK
	
	>> ./configure
	>> make
	>> sudo make install
	>> sudo ldconfig

.  
SiLK Analysis Tools:
--------------------
- - -

Absolute Filtering `(dst port 80 or (src port 80 and dst port 25))`

	>> rwfilter --dport=80 --pass=out1.rwf.gz in.rwf.gz
	>> rwfilter --sport=80 --dport=25 --pass=out2.rwf.gz in.rwf.gz
	
Concatenating Flow Records

	>> rwcat --output=out.rwf.gz out1.rwf.gz out2.rwf.gz
	>> rwcat out1.rwf.gz out2.rwf.gz >> out.rwf.gz
	>> rwappend [--create] out.rwf.gz out1.rwf.gz out2.rwf.gz
	
Reading Flow Records

	>> rwcut out.rwf.gz
	
Generating Statistical Summary

	>> rwstats --overall-stats out.rwf.gz
	
Creating Time Series (10 minute interval)
 
	>> rwcount --bin-size=600 out.rwf.gz
	
Sorting Flow Records (on `srcIP`)
	 
	>> rwsort --fields=1 --output=out-sort.rwf.gz out.rwf.gz
	
Grouping Flow Records and Setting Thresholds

	>> rwuniq --field=1 out.raw --bytes --packets=1000 --flows=200
	>> rwgroup --id-field=1,2,3,4 \
       --delta-field=9 --delta-value=3600 in.rwf.gz > out.rwf.gz

Remove Duplicate Flow Records

	>> rwdedupe --stime-delta=100 out1.rwf.gz out2.rwf.gz > out.rwf.gz
	
Splitting Flow Records

	>> rwsplit out.rwf.gz --basename=splits --flow-limit=1000
	
Show SiLK File Characteristics

	>> rwfileinfo out.rwf.gz
	
Merging Flow Records 

	srcIP = dstIP
	dstIP = srcIP
	srcPort = dstPort
	dstPort = srcPort

	>> rwmatch --relate=1,2 --relate=2,1 --relate=3,4 --relate=4,3 \ 
       query.rwf.gz response.rwf.gz stdout

.
 
Generate SiLK Flow Records:
---------------------------
- - - 
	
Generate Flow Records from Text Files

	>> rwtuc --fields=1-9 out.txt > out.rwf.gz
	
Generate Flow Record for each Dumped Packet from `tcpdump`

	>> rwptoflow out.pcap > out.rwf.gz
