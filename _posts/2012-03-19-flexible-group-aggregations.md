---
layout: post
title: "Flexible Group Aggregations"
description: ""
category: 
tags: []
---
{% include JB/setup %}
The group aggregation functions were hardcoded in the `group_aggr` struct (1, 2).
The functions are now replaced with RULES that map to a specific aggregation function.

	struct grouper_aggr group_aggr_branch1[4] = {
	-    { 0, trace_data->offsets.srcaddr, aggr_static_uint32_t },
	-    { 0, trace_data->offsets.dPkts, aggr_sum_uint32_t },
	-    { 0, trace_data->offsets.dOctets, aggr_sum_uint32_t },
	-    { 0, trace_data->offsets.tcp_flags, aggr_or_uint16_t }
	+    { 0, trace_data->offsets.srcaddr, RULE_STATIC | RULE_S1_32, NULL },
	+    { 0, trace_data->offsets.dPkts, RULE_SUM | RULE_S1_32, NULL },
	+    { 0, trace_data->offsets.dOctets, RULE_SUM | RULE_S1_32, NULL },
	+    { 0, trace_data->offsets.tcp_flags, RULE_OR | RULE_S1_16, NULL }
	   };

The mapping of the RULE to the function is done using a `switch` case: (3)

    /* for loop for the group-aggregation */
    for (int j = 0; j < binfos[i].num_aggr; j++) {
      switch (binfos[i].aggr[j].op) {
        ...
        case RULE_SUM | RULE_S1_32:
          binfos[i].aggr[j].func = aggr_sum_uint32_t;
          break;
          ...
      }    
    }
		
The aggregation functions are auto-generated using a python script
`fun-gen.py`. (4) In addition, the group aggregation records now also
have common fields from the filter stage, so a query like:

    filter f {
      srcport = 80
    }    
	...
    
would give group aggregation:    

	No. of Groups: 32 (Aggregations)
	
	... Sif   SrcIPaddress    SrcP  DIf   DstIPaddress    ...
	... 0     216.92.252.68   80    0     192.168.0.135   ...
	... 0     74.125.43.155   80    0     192.168.0.135   ... 
	... 0     209.85.129.104  80    0     192.168.0.135   ... 
	

Resources:

(1) [`pipeline` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/pipeline_8h.html)  
(2) [`flowy` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/flowy_8c.html)  
(3) [`auto_assign` references &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/auto__assign_8c.html)  
(4) [`fun_gen` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/namespacefun__gen.html)
