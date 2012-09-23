---
layout: post
title: "v2.5: it's verifiable"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Summary: (since after `v0.4`)

	>> git show v0.5
	
	tag v0.5
	Tagger: Vaibhav Bajpai <contact@vaibhavbajpai.com>
	Date:   Wed Jul 11 10:33:58 2012 +0200
	Commit 8d2f9b374a1e104e97398de47542cd5c0479a0dc
	
	* better engine usage on run.
	* evaluation of query ruleset lengths at RUNTIME.
	* python pipeline module to encapsulate pipeline stage classes.
	* painless parser installation using make.
	* parser installation instructions on debian/ubuntu and osx.
	* regression test-suite for the execution engine.
	* silk installation and usage instructions.
	* instructions to convert flow-tools traces to silk.
	* automated benchmarking suite.
	* resolved issues:
	  * no segfault on srcIP = dstIP in a grouper rule.
	  * no segfault when no grouper rules are defined.	