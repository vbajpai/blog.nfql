---
layout: post
title: "Refactoring the Execution Engine"
description: ""
category: 
tags: []
---
{% include JB/setup %}
- moved `branch_start(…)` function (previously in `main(…)`) to `branch.{h,c}` (1)
- moved filter stage code to newly created `filter.{h,c}` (2)
- renamed `grouper_fptr.{h,c}` → `grouper.{h,c}` (3)
- moved groupfilter stage code to newly created `groupfilter.{h,c}` (4)
- renamed `merger_fptr.{h,c}` → `merger.{h,c}` (5)
- moved `iterate.{h,c}` code (used by merger) to `utils.{h,c}` (6)
- removed number of undefined function prototypes from `utils.h` (6)

      struct bsearch_handle *
      tree_create_uintX_t(
                          char **records, 
                          size_t num_records, 
                          unsigned short field_offset
                         );
      char **
      tree_find_uintX_t( 
                        struct bsearch_handle *handle,
                        char *record, 
                        unsigned short field_offset
                       );
      void 
      tree_destroy(struct bsearch_handle *handle);
		
	where `X = {8, 16, 32, 64}`		



- reorganized the whole engine's `src` directory structure:

		├── auto-generated/
		│   ├── auto_assign.c
		│   ├── auto_assign.h
		│   ├── auto_comps.c
		│   ├── auto_comps.h
		│   └── fun_gen.py*
		├── bin/
		│   ├── filter-poop.py*
		│   ├── flowy-engine*
		│   ├── query.json
		│   ├── traces-long.ft*
		│   └── traces.ft*
		├── helpers/
		│   ├── error_handlers.c
		│   ├── error_handlers.h
		│   ├── utils.c
		│   └── utils.h
		├── pipeline/
		│   ├── branch/
		│   │   ├── branch.c
		│   │   ├── branch.h
		│   │   ├── filter.c
		│   │   ├── filter.h
		│   │   ├── grouper.c
		│   │   ├── grouper.h
		│   │   ├── groupfilter.c
		│   │   └── groupfilter.h
		│   ├── merger.c
		│   ├── merger.h
		│   └── pipeline.h
		├── Doxyfile
		├── Makefile
		├── README.md
		├── base.c
		├── base.h
		├── flowy.c
		├── flowy.h
		├── ftreader.c
		└── ftreader.h
		
Resources: 

(1) [`branch` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/branch_8h.html)  
(2)	[`filter` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/filter_8h.html)  
(3) [`grouper` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/grouper_8h.html)  
(4) [`groupfilter` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/groupfilter_8h.html)  
(5) [`merger` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/merger_8h.html)  
(6) [`utils` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/utils_8h.html)
	
