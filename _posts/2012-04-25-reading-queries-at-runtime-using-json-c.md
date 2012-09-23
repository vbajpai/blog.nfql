---
layout: post
title: "Reading Queries at Runtime using json c"
description: ""
category: 
tags: []
---
{% include JB/setup %}

The complete query is now read in at runtime. The query is supplied as a
`JSON` file.    The branchsets and each ruleset of the pipeline is a
`JSON` array. Each array is preceded by the number of items in that
array.  A sample `JSON` query is shown below:

    {
      "branchset": [
        "num_branches": 2
        {
          "filter": {
            "num_rules": 2,
            "ruleset": [...]
          },

          "grouper": {
            "num_rules": 2,
            "ruleset": [...]
          }

          "aggregation": {
            "num_rules": 4,
            "ruleset": [...]
          },
          
          "groupfilter": {
            "num_rules": 1,
            "ruleset": [...]
          },
        },
        { 
          ...
        }
      ],
      "merger": {
      "num_rules": 2,
        "ruleset": [...]
      },
    }


`json-c` (1) is used to parse the query read by calling `parse_json_query(...)`
	
	struct json* json_query = parse_json_query(param_data->query_mmap);

The structure thus filled looks like:	

	struct json {
	  size_t                          num_branches;
	  size_t                          num_mrules;
	  
	  struct json_branch_rules**      branchset;
	  struct json_merger_rule**       mruleset;
	};
	
where each branch rule looks like:

	struct json_branch_rules {
	  size_t                          num_frules;
	  size_t                          num_grules;
	  size_t                          num_arules;
	  size_t                          num_gfrules;
	  
	  struct json_filter_rule**       fruleset;
	  struct json_grouper_rule**      gruleset;
	  struct json_aggr_rule**         aruleset;  
	  struct json_gfilter_rule**      gfruleset;    
	};
	
The `json_query` is then used to prepare the `flowquery`struct used by
the stages (2).

	struct flowquery* fquery = prepare_flowquery(param_data->trace, json_query);
	
Practically, the `json_query` is just an intermediate and shouldn't be
needed.  Essentially `parse_json_query(...)` should directly fill in and
create the `flowquery` struct.  I see it as a future refactor item.

Resources:

(1) [`json-c` Library &rarr;](http://oss.metaparadigm.com/json-c/)  
(2) [`flowquery` structs and its descendents &rarr; ](http://mthesis.vaibhavbajpai.com/post/20901257812/complete-engine-refactor)
