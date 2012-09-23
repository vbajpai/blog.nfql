---
layout: post
title: "Lazy Comparator Assignments"
description: ""
category: 
tags: []
---
{% include JB/setup %}
There are dedicated comparator functions for each `uintX_t` type of the field offset.
Up until now, the choice for the function was made using a single function, `assign_fptr(...)` (1)
which was called before the start of the pipeline to ensure all function pointers point
to the right functions of each stage. Here is small snippet:


	assign_fptr(struct flowquery *fquery) {
	
	  for (int i = 0; i < fquery->num_branches; i++) {
	
	    struct branch* branch = fquery->branchset[i];    
	    
	    /* for loop for the filter */
	    for (int j = 0; j < branch->num_filter_rules; j++) {…}
	    
	    /* for loop for the grouper */
	    for (int j = 0; j < branch->num_grouper_rules; j++) {…}    
	    
	    /* for loop for the group-aggregation */
	    for (int j = 0; j < branch->num_aggr_rules; j++) {…}
	    
        /* for loop for the group-filter */
	    for (int j = 0; j < branch->num_gfilter_rules; j++) {…}	
	  }
	}
	
This function is quite computationally expensive, since it falls through a HUGE switch
statement to determine the function of right type. It is not guaranteed that given the 
type of the query and trace, the program will eventually go through each stage of the
pipeline. It is also possible that the program exits before, because there is nothing
more for the next stage to compute. As such the function pointers should be SET only
from within the stage for the current stage. Here is a snippet:

	assign_filter_func(struct filter_rule* const frule) {...}
	assign_grouper_func(struct grouper_rule* const grule) {...}  
	assign_aggr_func(struct aggr_rule* const arule) {...}
	assign_gfilter_func(struct gfilter_rule* const gfrule) {...}
	assign_merger_func(struct merger_rule* const mrule) {...}
	
Each of these functions are called from their respective stages just before when the 
comparison is required to be performed. As a result, we save the computation time
wasted in setting the function pointer for stage X if X is never executed.	

Resources:

(1) [`auto_assign` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/auto__assign_8c.html)
	
