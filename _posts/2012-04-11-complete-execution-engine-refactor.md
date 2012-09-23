---
layout: post
title: "Complete Execution Engine Refactor"
description: ""
category: 
tags: []
---
{% include JB/setup %}
- The complete project flow is reorganized to increase readability (1, 2)

	![parse_cmdline_args(…)](http://i.imgur.com/bwNbS.png)  
	![parse_json_query(…))](http://i.imgur.com/fGEHp.png)  
	![prepare_flowquery(…)](http://i.imgur.com/o0aNo.png)  
	![read_param_data(…)](http://i.imgur.com/bNZyn.png)  
	![filter(…)](http://i.imgur.com/boMWR.png)  
	![get_grouper_intermediate(…)](http://i.imgur.com/0ajML.png)  
	![grouper_aggregations(…)](http://i.imgur.com/l9wcn.png)  
	![grouperfilter(…)](http://i.imgur.com/nI5EY.png)  
	![merger(…)](http://i.imgur.com/jR86d.png)   
	![ungrouper(…)](http://i.imgur.com/5CTgc.png)  



- Query rules are clubbed in `X_ruleset`, `X` is `filter`, `grouper`,
`gfilter`, `merger` (3)


	- `flowquery {...}`
	
			struct flowquery {  
			  size_t                          num_branches;  
			  struct branch**                 branchset;  
			  			  
			  size_t                          num_merger_rules;  
			  struct merger_rule**            mruleset;
			  
			  struct merger_result*           merger_result; 
			  struct ungrouper_result*        ungrouper_result;
			};	


	- `branch {...}`


        struct branch {
	
			  /*---------------------------------------------------------------*/  
			  /*                          inputs                               */
			  /*---------------------------------------------------------------*/  
			  ...		
			  size_t                          num_filter_rules;
			  size_t                          num_grouper_rules;
			  size_t                          num_aggr_rules;
			  size_t                          num_gfilter_rules;
			  
			  struct filter_rule**            filter_ruleset;  
			  struct grouper_rule**           grouper_ruleset;
			  struct aggr_rule**              aggr_ruleset;  
			  struct gfilter_rule**           gfilter_ruleset;  
			  /*---------------------------------------------------------------*/  
			
			  /*---------------------------------------------------------------*/  
			  /*                          output                               */
			  /*---------------------------------------------------------------*/  
			  struct filter_result*           filter_result;
			  struct grouper_result*          grouper_result;
			  struct groupfilter_result*      gfilter_result;
			  /*---------------------------------------------------------------*/  

			};

- Each stage returns `X_result` `X` is `filter`, `grouper`, `gfilter`, `merger`, `ungrouper` (3).
  
	- `filter(...)` returns `filter_result`
	
        struct filter_result* filter(...) {...}
	   where  
	   
	      struct filter_result {          
			  size_t                          num_filtered_records;
			  char**                          filtered_recordset;  
			};
			
	- `grouper(...)` returns `grouper_result`
	
        struct grouper_result* grouper(...) {...}
	   where  
	   
			struct grouper_result {
			  size_t                          num_unique_records;  
			  char**                          sorted_recordset;
			  char**                          unique_recordset;
			  
			  size_t                          num_groups;  
			  struct group**                  groupset;
			};		
	
	- `groupfilter(...)` returns `groupfilter_result`
	
	      struct groupfilter_result* groupfilter(...) {...}
	   where  
	   
			struct groupfilter_result {
			  size_t                          num_filtered_groups;  
			  struct group**                  filtered_groupset;
			};
	
	- `merger(...)` returns `merger_result`
	
        struct merger_result* merger(...) {...}
	   where  
	   
			struct merger_result {
			  size_t                          num_group_tuples;  
			  size_t                          total_num_group_tuples;
			  struct group***                 group_tuples;
			};
			
	- `ungrouper(...)` returns `ungrouper_result`	
	
        struct ungrouper_result* ungrouper(...) {...}
	   where  
	   
			struct ungrouper_result {
			  size_t                          num_streams;    
			  struct stream**                 streamset;
			};

- Added `echo.{h,c}` for verbose and debug modes (4)

	- with atleast `--verbose=1`:  

		![echo_filter](http://i.imgur.com/1AGfj.png)  

		- with atleast `--verbose=2`:  
		
			![echo_grouper](http://i.imgur.com/sGZff.png)   

  		
		![echo_group_aggr](http://i.imgur.com/IjdHh.png)  
		![echo_gfilter](http://i.imgur.com/4JqOp.png)  
		![echo_merger](http://i.imgur.com/aKQs0.png)  
		
	- always:  

		![echo_results](http://i.imgur.com/guxpg.png)

- `X_ruleset` are deallocated as soon as `X` stage returns.

	- `grouper(...)`

			 branch->grouper_result = grouper(...); 
			 if (branch->grouper_result == NULL)
			   errExit("grouper(...) returned NULL");
			 else {
			 			    
			   /* free filter rules */
			   ...		
			   /* free grouper rules */
			   ...				    
			   /* free grouper aggregation rules */
			   ...		
			 }  
			 
	- `groupfilter(...)`
	
			branch->gfilter_result = groupfilter(...);
			if (branch->gfilter_result == NULL)
			  errExit("groupfilter(...) returned NULL");
			else {
			   
			  /* free group filter rules */
			  ...
			}
			
	- `merger(...)`
	
			fquery->merger_result = merger(...); 
			  
			if (fquery->merger_result == NULL)
			  errExit("merger(...) returned NULL");
			else {
			    
			    /* free merger rules */	
			    ... 
			}    

References:

(1) [`flowy` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/flowy_8c.html)  
(2) [`branch` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/branch_8c.html)  
(3) [`flowquery` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/structflowquery.html)  
(4) [`echo` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/echo_8h_source.html)
