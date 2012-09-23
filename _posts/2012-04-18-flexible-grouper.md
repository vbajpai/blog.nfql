---
layout: post
title: "Flexible Grouper"
description: ""
category: 
tags: []
---
{% include JB/setup %}
The `grouper(...)` (1) call was plagued with hardcoded `uint32_t` type assumption on the field offset being used 
to make grouper rule comparisons. The function now internally calls `get_gtype(...)` to fall through a switch
case to determine the type of the field offset at RUNTIME. Here is a snippet:


	struct grouper_type* get_gtype(uint64_t op) { 
	  ...
	  switch (op) {
	  
	    case RULE_S2_8:
	      gtype->qsort_comp = comp_uint8_t;
	      gtype->bsearch = bsearch_uint8_t;
	      gtype->alloc_uniqresult = alloc_uniqresult_uint8_t;
	      gtype->get_uniq_record = get_uniq_record_uint8_t;      
	      gtype->dealloc_uniqresult = dealloc_uniqresult_uint8_t;

	      break;
	    case RULE_S2_16:
	      ...
	      break;
	      
	    case RULE_S2_32:
	      ...
	      break;
	      
	    case RULE_S2_64:
	      ...
	      break;
	  }
	  return gtype;
	}
	
At this point I do not think I have any assumptions on any field offset for any pipeline stage. As such 
experimentation on different style of queries should now work and is the next item on the list to investigate.


Resources:

(1) [`grouper` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/grouper_8h_source.html)
