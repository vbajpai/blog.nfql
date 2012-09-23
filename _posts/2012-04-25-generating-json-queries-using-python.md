---
layout: post
title: "Generating JSON queries using Python"
description: ""
category: 
tags: []
---
{% include JB/setup %}

The `JSON` query is verbose and quite cumbersome to manually edit.  The
Python Parser will eventually emit this intermediate format, so the next
logical step is to generate the query from Python.  A sample Python
script looks like -

    class FilterRule: ...
    class GrouperRule: ...
    class AggregationRule: ...
    class GroupFilterRule: ...
    class MergerRule: ...

    bset = []
    bset.append (
                  {
                   'filter': {'num_rules': len(fruleset), 'ruleset': fruleset},
                   'grouper': {'num_rules': len(gruleset), 'ruleset': gruleset},
                   'aggregation': {'num_rules': len(aruleset), 'ruleset': aruleset},
                   'groupfilter': {'num_rules': len(gfruleset), 'ruleset': gfruleset},
                  }
                );

    query = {
              'num_branches': len(branchset),
              'branchset': branchset,
              'merger': merger
            }

    fjson = json.dumps(query, indent=2)



where each `ruleset`is a list of Python objects. For instance,
`fruleset`is a list of `FilterRule`objects.  At this point, the Python
Parser just needs to create each stage rule objects and this script will
take care to emit the `JSON`.

Some example scripts to generate `JSON` queries is given in `scripts/queries/`

	|-- scripts/
	|   |-- queries/
	|   |   |-- build-ftp-tcp-session.py*
	|   |   |-- build-http-octets.py*
	|   |   |-- build-http-tcp-session.py*
	|   |   `-- build-https-tcp-session.py*
