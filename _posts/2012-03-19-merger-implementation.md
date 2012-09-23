---
layout: post
title: "Merger Implementation"
description: ""
category: 
tags: []
---
{% include JB/setup %}

The engine can now merge the groups from different branches according to
a merging criteria.  For example, a simple query to merge groups looks
like:

	merger m {
	  module m1 {
	    branches A, B
	    A.srcip = B.dstip
	    A.dstip = B.srcip
	  }
	}

such a merging rule reflects in the engine as (1):

	struct merger_rule mfilter[2] = {
      {
        &binfos[0], trace_data->offsets.srcaddr, 
        &binfos[1], trace_data->offsets.dstaddr, 
        RULE_EQ | RULE_S1_32 | RULE_S2_32, 0, NULL
      },

      {
        &binfos[0], trace_data->offsets.dstaddr, 
        &binfos[1], trace_data->offsets.srcaddr,
        RULE_EQ | RULE_S1_32 | RULE_S2_32, 0, NULL
      },
	};

where the rule maps to a specific merger function using a `switch` case (2):


	/* for loop for the merger */
	for (int j = 0; j < fquery->num_merger_rules; j++) {
	  switch (fquery->mrules[j].op) {
	    ...
	    case RULE_EQ | RULE_S1_32 | RULE_S2_32:
	      fquery->mrules[j].func = merger_eq_uint32_t_uint32_t;
	      break;
	    ...
	  }
	}    
	  
The merger functions are auto-generated using a python script `fun-gen.py` (3).

This is how the result looks:

    [engine]$ ./flowy-engine traces-long.ft query.json --verbose=1

	No. of Merged Groups: 2 (Tuples)
	
	... Sif   SrcIPaddress    SrcP  DIf   DstIPaddress    DstP  ... 
	
	... 0     192.168.0.135   0     0     216.46.94.66    80    ... 
	... 0     216.46.94.66    80    0     192.168.0.135   0     ... 
	
	... 0     192.168.0.135   0     0     209.84.12.126   80    ... 
	... 0     209.84.12.126   80    0     192.168.0.135   0     ... 
	
	
All the permutation of group tuples checked for a match can be seen by increasing the verbosity level.  


Resources:

(1) [`flowy` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/flowy_8c.html)  
(2) [`auto_assign` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/auto__assign_8c.html)  
(3) [`fun_gen` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/namespacefun__gen.html)
