---
layout: post
title: "Refactored JSON Query Scripts"
description: ""
category: 
tags: []
---
{% include JB/setup %}

The classes defined for each pipeline stage are now refactored out into `scripts/queries/pipeline.py`.  

	def protocol(name):
	  return socket.getprotobyname(name)
	
	class FilterRule: ...
	class GrouperRule: ...
	class AggregationRule: ...
	class GroupFilterRule: ...
	class MergerRule: ...
	
Scripts that generate `JSON` queries can now `import` this module to reduce code redundancy.
	
	import json
	from pipeline import FilterRule, GrouperRule, AggregationRule
	from pipeline import GroupFilterRule, MergerRule
	from pipeline import protocol
	
	if __name__ == '__main__':
	
	  fruleset = []
	  fruleset.append(vars(FilterRule(...)))
	  ...
	  filter = {'ruleset': fruleset}
	
	  gruleset = []
	  gruleset.append(vars(GrouperRule(...)))
	  ...
	  grouper = {'ruleset': gruleset}
	
	  aruleset = []
	  aruleset.append(vars(AggregationRule(...)))
	  a = {'ruleset' : aruleset}
	
	  gfruleset = []
	  gfruleset.append(vars(GroupFilterRule(…)))
	  gfilter = {'ruleset' : gfruleset}
	
	  branchset = []
      branchset.append (
                         {
                          'filter': filter,
                          'grouper': grouper,
                          'aggregation': a,
                          'gfilter': gfilter,
                         }
                       )

	  mruleset = []
	  mruleset.append(vars(MergerRule(...)))
	  merger = {'ruleset' : mruleset}
	  query = {'branchset': branchset, 'merger': merger}
	  ...	
