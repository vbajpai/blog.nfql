---
layout: post
title: "Few Closed Issues"
description: ""
category: 
tags: []
---
{% include JB/setup %}
- Greedily deallocating non-filtered records.

	Each branch of the pipeline is executed by a separate thread. Since each branch does NOT have a copy of the trace
	but points to the original records, individual branches CANNOT free the records that were not filtered by them, since
	they could be filtered by some other branch. As a consequence, the records that were not filtered by any branch can only be 	free'd once all threads join the `main(…)`, ie. before calling the `merger(…)`. The following implementation is costly and
	runs in worst case `O(nkm)` where `n` is the number of records in the trace, `k` is the number of branches, and `m` is the
	number of filtered records in each branch.
	
		-  for (int i = 0; i < param_data->trace->num_records; i++) {
		-    bool if_filtered_record = false;
		-    char* record = param_data->trace->records[i];
		-    for (int j = 0; j < fquery->num_branches; j++) {
		-      struct branch_info* branch = fquery->branchset[j];        
		-      for (int k = 0; k < branch->filter_result->num_filtered_records; k++) {
		-        char* filtered_record = branch->filter_result->filtered_recordset[k];
		-	  ...
		-    }
		-    if (!if_filtered_record) {
		-      free(record); record = NULL;
		-      param_data->trace->records[i] = NULL;      
		-    }
		
          struct merger_result* mresult = merger(...)
          ...
		
	Instead, it would be better to extend the trace structure to allow a flag that stores meta-information about the record.
	
          struct ft_data {
      -     char**                          records;
      +     struct record**                 recordset;
      +     int                             num_records;
          };
		 
		+   struct record {
		+     char*                           record;
		+     bool                            if_filtered;
		+   };	
		
	
	Now, the same can free the non-filtered records in worst case `O(n)` time where `n` is the number of records in the trace.
		
		
		+  for (int i = 0; i < param_data->trace->num_records; i++) {      
		+    struct record* recordinfo = param_data->trace->recordset[i];
		+    if (recordinfo->if_filtered == false)
		+      free(recordinfo->record); recordinfo->record = NULL;
		   }

- Resolved a Grouper segfault when NO records got filtered.

         struct filter_result* fresult = filter(...);
		+  if (fresult->num_filtered_records == 0)
		+    pthread_exit(NULL);
		      
		   struct grouper_result* gresult = grouper(...);  
