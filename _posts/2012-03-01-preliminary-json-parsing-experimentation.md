---
layout: post
title: "Preliminary JSON Parsing Experimentation"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Preliminary JSON parsing experimentation

- added filter comparison functions in `auto_assign.c` using `fun_gen.py` (1, 2)
- using the new comparison functions in the `filter_rule` struct (3)
- added a temporary `filter-poop.py` to spit out JSON representation of a python object using the `json` module (4, 5)

	where the python class is: 

		class FilterStatement: 		  
		  def __init__(self, name, value, datatype, delta, op):
		    self.offset = {
		      'name': name,
		      'value': value,
		      'datatype': datatype
		    }		    
		    self.delta = delta
		    self.op = op		

	executing the python file gives:
			    
		$ python filter-poop.py
	    {
		  "delta": 0, 
		  "op": "RULE_EQ", 
		  "offset": {
		    "datatype": "u_int16", 
		    "name": "dstport", 
		    "value": 80
		  }
		} 		    
	
- The execution engine can now parse this JSON file to read the parameters for the filter stage (for now)  (3, 6)

		$ ./flowy-engine --absolute traces.ft query.json
		
      No. of Filtered Records: 4

      ...	SrcIPaddress    SrcP  DIf   DstIPaddress    DstP  ...
      ...	10.50.239.4     39788 0     209.85.135.19   80    ...
      ...	10.50.239.4     40339 0     81.19.80.14     80    ...
      ...	10.50.239.4     46499 0     209.85.135.19   80    ...



Resources -

(1) [`auto_assign` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/auto__assign_8c.html)  
(2) [`fun_gen` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/fun__gen_8py.html)  
(3) [`flowy` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/flowy_8c.html)  
(4) [`filter-poop` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/namespacefilter-poop.html)  

External Resources - 

(5) [`python json module` reference &rarr;](http://docs.python.org/library/json.html)  
(6) [`json-c library` reference &rarr;](http://oss.metaparadigm.com/json-c/)   
