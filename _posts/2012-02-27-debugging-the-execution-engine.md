---
layout: post
title: "Debugging the Execution Engine"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Debugging the execution engine.

- Added a <s>`DEBUGENGINE` macro</s> `debug bool` in `base_header.h` (1)

Changes in `ftreader` (2)

- Added `ftio_check_xfield(…)` to check the proper format before retrieving the xfield using `ftio_xfield(…)` 
- Added `flow_print_record(…)` to print the flow-records.
- `ftreader` now prints the read trace in a `flow-print-f5` format when `DEBUGENGINE` is set.


Changes in `pipeline` (3)

- Saving pointers to the filtered records when `DEBUGENGINE` is set.

		struct branch_info {
		  
		  /* have to be filled manually */
		  ...
		  
		#ifdef DEBUGENGINE
		  char** filtered_records;
		  size_t num_filtered_records;  
		#endif
		  
		  /* will be filled by individual branches */
		  ...
		};



Changes in `flowy` (4)

- Added a bunch load of code comments to describe the flow.
- Added separate macros for each stage of the pipeline for now, until the segfault is resolved.
- Printing the filtered flow-records when the <s>`DEBUGENGINE`</s> `--debug` is set.


Resources -

(1) [`base_header` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/base__header_8h.html)

(2) [`ftreader` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/ftreader_8c.html)

(3) [`pipeline` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/pipeline_8h.html)

(4) [`flowy` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/flowy_8c.html)
