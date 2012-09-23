---
layout: post
title: "Evaluation of Ruleset Lengths at Runtime"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Previously a `JSON` query also described the number of rules contained
in the `rulesets` that followed.  Since `JSON` is based on Javascript
Objects there is a possibility to let the engine calculate the length of
the array at runtime.

    {
      "branchset": [
        "num_branches": 2
        {
          "filter": {
            "num_rules": 2,
            "ruleset": [...]
          },
        ...
    }]}

A similar `JSON` query is condensed to now look like:

    {
      "branchset": [
        {
          "filter": {
            "ruleset": [...]
          },
      ...
    }]}
