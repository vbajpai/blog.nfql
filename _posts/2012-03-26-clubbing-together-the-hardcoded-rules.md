---
layout: post
title: "Clubbing Together the Hardcoded Rules"
description: ""
category: 
tags: []
---
{% include JB/setup %}
- clubbed number of rules into one place in (1)

      /* this should go away once the rules come from the JSON */		
      #define NUM_BRANCHES 2
      #define NUM_FILTER_RULES_BRANCH1 1
      #define NUM_FILTER_RULES_BRANCH2 1
      #define NUM_GROUPER_RULES_BRANCH1 2
      #define NUM_GROUPER_RULES_BRANCH2 2
      #define NUM_GROUPER_AGGREGATION_RULES_BRANCH1 4
      #define NUM_GROUPER_AGGREGATION_RULES_BRANCH2 4
      #define NUM_GROUP_FILTER_RULES_BRANCH1 1
      #define NUM_GROUP_FILTER_RULES_BRANCH2 1
      #define NUM_MERGER_RULES 2        


- Instead of manually filling up structs of each branch as shown below: [2]

      -  binfos[0].branch_id = 0;
      -  binfos[0].filter_rules = filter_rules_branch1;
      -  binfos[0].num_filter_rules = 1;
         ...
            
      -  binfos[1].branch_id = 1;
      -  binfos[1].filter_rules = filter_rules_branch2;
      -  binfos[1].num_filter_rules = 1;
         ...

  The number of branches and rules come from `flowy.h` and the code now loops through the rules to assign values (2).

      +  /* rules for each branch */
      +  for (int i = 0; i < fquery->num_branches; i++) {
         
      +    struct filter_rule* frule = calloc(fquery->branches[i].num_filter_rules, 
      +                                       sizeof(struct filter_rule));    
      +    for (int j = 0; j < fquery->branches[i].num_filter_rules; j++) {
      +      ...
      +    }
      +  ...		  
		   
   Rules are still hardcoded but now they are in one place. It will make debugging and transitioning to `JSON` queries easier.
   

Resources:

(1) [`flowy.h` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/flowy_8h_source.html)  
(2) [`flowy.c` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/flowy_8c.html)
