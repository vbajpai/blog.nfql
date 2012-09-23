---
layout: post
title: "Flexible Group Filters and Aggregations"
description: ""
category: 
tags: []
---
{% include JB/setup %}
- `gfilter_rule` now takes in a `uint_X` RULE when mapping functions (1)

		  struct gfilter_rule gfilter_branch1[1] = {
	    -   {trace_data->offsets.dPkts, 200, 0, RULE_GT, NULL}
		+   {trace_data->offsets.dPkts, 200, 0, RULE_GT | RULE_S1_32, NULL}
		  };

	The additional RULE is used to map to a function that knows the type of the offset at RUNTIME (2).
	
		  switch (binfos[i].gfilter_rules[j].op) {
		-   case RULE_EQ:
		-     binfos[i].gfilter_rules[j].func = gfilter_eq;
		+   case RULE_EQ | RULE_S1_32:
		+     binfos[i].gfilter_rules[j].func = gfilter_eq_uint32_t;
		+   break;
        ...
		  }

	The additional switch cases and comparison functions are automatically generated using `fun_gen.py` (3).

- removed `uintX_t` assumptions from grouper aggregations

  The aggregation function needs to know the type of the offsets used previously in the filter and grouper rules
  to be able to fill in common fields in its cooked v5 group aggregation record. As a result a `get_aggr_fptr(…)` 
  function was defined that takes in those previous rules to fall through a switch to return a function pointer to
  an aggregation function of the correct type (2).
  
  
		  switch (op) {
		+   case RULE_EQ | RULE_S1_8:
		+   case RULE_NE | RULE_S1_8:
		+   case RULE_GT | RULE_S1_8:
		    ...
		+     aggr_function = aggr_static_uint8_t;
		+     break;
		  }
  
  
  
  This aggregation function is then later used to fill in the common fields (4). An example using the filter rules is given below:


      struct aggr (*aggr_function)(
                                    char **records,
                                    char *group_aggregation,
                                    size_t num_records,
                                    size_t field_offset,
                                    bool if_aggr_common
                                  ) = NULL;

      aggr_function = get_aggr_fptr( binfo->filter_rules[i].op );    

      (*aggr_function)(
                        group->members,
                        group_aggregation,
                        group->num_members, 
                        field_offset, 
                        TRUE
                      );

  A similar call is made for the grouper rules as well.  The additional
  switch cases and comparison functions are automatically generated using
  `fun_gen.py` (3)
    
  The idea of returning a function pointer from a function makes me
  question if I am trying to write LISP in C? A quick google search led me
  to (5)

Resources: 

(1) [`flowy` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/flowy_8c.html)  
(2) [`auto_assign` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/auto__assign_8c.html)  
(3) [`fun_gen` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/fun__gen_8py.html)  
(4) [`grouper` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/grouper_8c.html)  
(5) [greenspun's tenth rule, wikipedia &rarr;](http://en.wikipedia.org/wiki/Greenspun's_tenth_rule)
