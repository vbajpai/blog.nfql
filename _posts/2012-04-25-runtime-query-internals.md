---
layout: post
title: "Runtime Query Internals"
description: ""
category: 
tags: []
---
{% include JB/setup %}

When reading the `JSON` query at RUNTIME, the field offsets of the
NetFlow v5 record `struct` are read in as a `char` pointer.  A utility
function `get_offset(...)` (2) was thus introduced that maps the read
offset names to struct offsets.

	size_t
	get_offset (
                const char * const name, 
                const struct fts3rec_offsets* const offsets
               ) {
	  
	  #define CASEOFF(memb)                       \
	  if (strcmp(name, #memb) == 0)               \
	    return offsets->memb
	  
      CASEOFF(unix_secs);
      CASEOFF(unix_nsecs);
      ...	
			  
	  return -1;
	}

Similary the type of each offset and the operation type are also read in
as `char` pointers.   This information is saved and thus used in the
engine using an `enum` defined in `pipeline.h` (1).  Therefore, another
utility function `get_enum(...)` (2) was defined to map this information
to the unique enum members.

	uint64_t
	get_enum(const char * const name) {
	  
	  #define CASEENUM(memb)                      \
	  if (strcmp(name, #memb) == 0)               \
	    return memb
	  
	  CASEENUM(RULE_S1_8);
	  CASEENUM(RULE_S1_16);
	  ...
	  
  	  CASEENUM(RULE_S2_8);
	  CASEENUM(RULE_S2_16);
	  ...
	
	  CASEENUM(RULE_ABS);
	  CASEENUM(RULE_REL);
	  CASEENUM(RULE_NO);
	  ...	  
	
	  CASEENUM(RULE_EQ);
	  CASEENUM(RULE_NE); 
	  ...	  
	
	  CASEENUM(RULE_STATIC);
	  CASEENUM(RULE_COUNT);
	  ...	  
	
	  CASEENUM(RULE_ALLEN_BF);
	  CASEENUM(RULE_ALLEN_AF);
	  ...	  
	  
	  return -1;
	}

Resources:

(1) [`pipeline` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/pipeline_8h.html)  
(2) [`utils` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/utils_8c.html)
