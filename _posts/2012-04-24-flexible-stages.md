---
layout: post
title: "Flexible Stages"
description: ""
category: 
tags: []
---
{% include JB/setup %}
`grouper(...)` and `groupfilter(...)` only proceed when previous stage returned something.

	/* grouper */
	struct grouper_result*
	grouper(...) {

	  /* go ahead if there is something to group */
	  if (fresult->num_filtered_records > 0) {...}    
	}

	/* group filter */
	struct groupfilter_result*
	groupfilter(...) {

	  /* go ahead if there is something to group filter */
	  for (int i = 0, j = 0; i < gresult->num_groups; i++) {...}
	}

`merger(...)` proceeds only when every branch has non-zero filtered
groups. This is achieved using the iterator.   `iter_init(...)`
deallocates and returns `NULL` if any one branch has 0 filtered groups.
Consequently a check is performed in the `merger(...)` to make sure
`iter` is NOT `NULL`before proceeding.

	/* merger */
	struct merger_result*
	merger(...) {

	  /* initialize the iterator */
	  struct permut_iter* iter = iter_init(num_branches, branchset);
	  if (iter == NULL)
	    return mresult;
	  ...
	}

As a result, some issues were resolved and closed - 

- merger segfault when `num_filtered_records` for either branch is 0  
- merger segfault when `num_filtered_groups` in either branch is 0
	
