---
layout: post
title: "New Grouper Features"
description: ""
category: 
tags: []
---
{% include JB/setup %}
- Pretty printing group aggregations as new flow record representatives. For example:

		grouper g_www_res {
		   module g1 {
		      srcip = srcip
		      dstip = dstip
		   }
		   aggregate srcip, dstip, sum(bytes) as bytes, bitOR(tcp_flags) as flags, 
		}
		
  will form groups with same `srcIP` and `dstIP` and display as:		
	
		No. of Groups: 32 (Aggregations)
		
		...	   SrcIPaddress    ...	  	DstIPaddress      OR(Fl)    Sum(Octets)
	
		...	   4.23.48.126     ...  	192.168.0.135     3         81034
		...	   8.12.214.126    ...  	192.168.0.135     2         5065
		...	   80.157.170.88   ...  	192.168.0.135     6         18025
		
	The idea is to return aggregates, common fields and sets of uncommon fields.  
	The feature to return set of uncommon fields still needs to be implemented.  
	
- There can be a situtation as in the example above, where the query designer might incorrectly
ask for aggregation on a field already specified in a grouper module. Of course, the aggregation
does not make any sense, since the grouped record will always have the same value for that field.
The engine is now smart to realize it and ignores such aggregations as above.

- Records are clubbed together into one group if no group modules are defined

		grouper g_www_res {
		   module g1 {}
		   aggregate sum(bytes) as bytes
		}
	
	Previously such a query would form groups for each individual filtered record. That was less useful
	since then one could not just perform meaningful aggregates (as above). Now, when the group modules
	are empty, all the filtered records are clubbed into one group to allow such aggregations.
	
		No. of Groups: 1 (Aggregations)
	
      ...    Sum(Octets)
      ...    2356654
