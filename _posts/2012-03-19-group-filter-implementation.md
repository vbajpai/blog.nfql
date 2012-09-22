---
layout: post
title: "Group Filter Implementation"
description: ""
category: 
tags: []
---
{% include JB/setup %}
The engine can now filter the groups according to a filtering criteria.  
For example, a simple query to filter groups with `sum(pkts) > 200` looks like:

	grouper g {
	  module g1 {
	    srcip = srcip
	    dstip = dstip
	  }
	  aggregate srcip, dstip, sum(packets) as pkts
	}
	
	groupfilter gf {
	  pkts > 200
	}


such a filtering rule reflects in the engine as (1):

	struct gfilter_rule gfilter_branch2[1] = {
      {trace_data->offsets.dPkts, 200, 0, RULE_GT, NULL}
	};

where the rule maps to a specific group-filter function using a `switch` case (2):


	/* for loop for the group-filter */
	  for (int j = 0; j < binfos[i].num_gfilter_rules; j++) {
	    switch (binfos[i].gfilter_rules[j].op) {
          ...
          case RULE_GT:
            binfos[i].gfilter_rules[j].func = gfilter_gt;
            break;
          ...
	    }
	  }
	  
The group-filter functions are auto-generated using a python script `fun-gen.py` (3).

This is how the result looks:

    [engine]$ ./flowy-engine traces-long.ft query.json --debug
	...
	
	No. of Filtered Groups: 5 (Aggregations)
	
	...	   SrcIPaddress   	...     DstIPaddress      Sum(Pkts)
	...	   216.46.94.66    	...  	192.168.0.135     345
	...	   209.84.12.126   	...  	192.168.0.135     475
	...	   207.171.166.252 	...  	192.168.0.135     212
	

Resources:	

(1) [`flowy` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/flowy_8c.html)  
(2) [`auto_assign` references &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/auto__assign_8c.html)  
(3) [`fun_gen` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/namespacefun__gen.html)
