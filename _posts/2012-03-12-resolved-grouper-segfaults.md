---
layout: post
title: "Resolved Grouper Segfaults"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Removed GNU99 extensions to increase portability

- Replaced `Boolean enum`  with `C99 bool`
- Removed Anonymous Unions
- Added a `C99` flag in the Makefile.

The order of arguments passed in `qsort_r(…)` and `bsearch_r(…)` was incorrect.
This could also be due to the different order of arguments in `glibc` and `BSD`.

    void 
    qsort_r(
            void *base, size_t nel, 
            size_t width, 
            void *thunk, 
            int (*compar)(void *, const void *, const void *)
           );
    		
    void *
    bsearch_r(
              const void *key, 
              const void *base, 
              size_t nel, 
              size_t width, 
              void *thunk, 
              int (*compar) (const void *, const void *)
             );
    		  
Removed the `--absolute` switch. This should be implied from the supplied query.

Pretty Printing each sub stage of the grouper pipeline.

- printing the sorted filtered records on `--debug`. 
	The records are sorted to the first rule in the grouper module:

		grouper g_www_req {
		   module g1 {
		      srcip = srcip
			  ...
		   }	  
		}
		
	then the filtered records are sorted according to the `srcip`

	 	[engine]$ ./flowy-engine --debug traces-long.ft query.json
	
		No. of Sorted Records: 166
		
		...	SrcIPaddress    ...
	
		...	4.23.48.126     ...
		...	4.23.48.126     ...
		...	4.23.48.126     ...
		...	8.12.214.126    ...

		

- printing the unique sorted records on `--debug`.	The duplicate sorted records are filtered out:

		No. of Unique Records: 32
		
		...	SrcIPaddress    ...
	
		...	4.23.48.126     ...
		...	8.12.214.126    ... 
		
- printing the formed groups on `--debug`. The example shows groups formed with records having the same `srcIP` and `dstIP`
and having `srcP 80` (which was the rule in the filter stage)

      No. of Groups: 32 (Verbose Output)

      ...	SrcIPaddress    SrcP  DIf   DstIPaddress   ...

      ...	4.23.48.126     80    0     192.168.0.135  ...
      ...	4.23.48.126     80    0     192.168.0.135  ...

      ...	8.12.214.126    80    0     192.168.0.135  ...
      ...	8.12.214.126    80    0     192.168.0.135  ...
