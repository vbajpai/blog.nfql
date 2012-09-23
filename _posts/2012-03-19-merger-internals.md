---
layout: post
title: "Merger Internals"
description: ""
category:
tags: []
---
{% include JB/setup %}
The pseudocode for the merger looks like: (1)

    get_module_output_stream(module m) {
      (branch_1, branch_2, ..., branch_n) = get_input_branches(m);
      for each g_1 in group_records(branch_1)
        for each g_2 in group_records(branch_2)
          ...
            ...
               for each g_n in group_records(branch_n)
                  if match(g_1, g_2, ..., g_n, rules(m))
                     output.add(g_1, g_2,..., g_n);
      return output;
    }

Implementing this pseudocode in `C` is not straightforward, since the level of nesting actually depends on the number of branches,
and is therefore not known until RUNTIME when is passed through the query.

As such we need to implement a utility that can provide all possible permutations of `N`-tuple group record IDs that we can use
to make a match, where `N` is the number of branches.

As such, we initialize an iterator passing it the number of branches, and information about each branch.
Then, we iterate over until the iterator returns `FALSE`. (2)
A sample code to print all possible groupID permutation tuples is shown below:

	/* initialize the iterator */
	struct permut_iter *iter = iter_init(binfo_set, num_branches);

	/* iterate over all permutations */
	unsigned int index = 0;
	while(iter_next(iter)) {
	  index++
	  for (int j = 0; j < num_branches; j++) {
        /* first item */
        if(j == 0)
          printf("\n%d: (%zu ", index, iter->filtered_group_tuple[j]);
        /* last item */
        else if(j == num_branches -1)
          printf("%zu)", iter->filtered_group_tuple[j]);
        else
          printf("%zu ", iter->filtered_group_tuple[j]);
      }
	}

	/* free the iterator */
	iter_destroy(iter);

The output gives all possible permutations, which we later use to try to make a match.
Here, the number of filtered groups passed to merger where: `(b1, b2, b3) = (3, 2, 2)`

	1: (1 1 1)
	2: (1 1 2)
	3: (1 2 1)
	4: (1 2 2)
	5: (2 1 1)
	6: (2 1 2)
	7: (2 2 1)
	8: (2 2 2)
	9: (3 1 1)
	10: (3 1 2)
	11: (3 2 1)
	12: (3 2 2)

Resources:

(1) [vmarinov's masters thesis &rarr;](https://svn.eecs.jacobs-university.de/svn/eecs/archive/msc-2009/vmarinov.pdf)  
(2) [`merger` utilities &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/utils_8c.html)
