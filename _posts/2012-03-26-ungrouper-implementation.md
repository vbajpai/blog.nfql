---
layout: post
title: "Ungrouper Implementation"
description: ""
category: 
tags: []
---
{% include JB/setup %}
- added `ungrouper.{h,c}` (1)
- extended the `flowquery` struct to keep information coming from the ungrouper (2)

		  struct flowquery {
		    struct group*** group_tuples;
		    size_t num_group_tuples;
		    size_t total_num_group_tuples;
		+   struct stream** streamset;
		+ };
		+
		+ struct stream{
		+   char ** recordset;
		+   size_t num_records;
		+ };

- given the `group_tuples` the ungrouper returns a set of stream of flow records (3).

      fquery->streamset = ungrouper(
                                    fquery->group_tuples,
                                    fquery->num_group_tuples
                                   );


- The ungrouper returns as many streams as there are number of matched group tuples. An example is given below:

	    [engine]$ bin/flowy-engine bin/traces-long.ft bin/query.json

		No. of Streams: 2
		----------------- 
		
		No. of Records in Stream (1): 24 
		
		... Sif   SrcIPaddress    SrcP  DIf   DstIPaddress    DstP  ... 
		... 0     192.168.0.135   56225 0     216.46.94.66    80    ... 
		... 0     216.46.94.66    80    0     192.168.0.135   56228 ... 

		No. of Records in Stream (2): 62 
		
		... Sif   SrcIPaddress    SrcP  DIf   DstIPaddress    DstP  ... 
		... 0     192.168.0.135   56241 0     209.84.12.126   80    ... 
		... 0     209.84.12.126   80    0     192.168.0.135   56240 ...


	Each stream represents a connection between a `srcIP` and `dstIP` where the traffic is being	carried over `port 80`	
	The flow records returned as a part of a stream are not ordered according to their timestamps and is a future work item.
	
Resources:	

(1) [`ungrouper` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/ungrouper_8c.html)  
(2) [`pipeline` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/pipeline_8h_source.html)  
(3) [`flowy` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/flowy_8c.html)
