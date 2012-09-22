---
layout: post
title: "Installing and Running the Execution Engine"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Install `flow-tools`

For 32-bit machines, go ahead, but for `AMD64` follow (1)

	$ brew install https://raw.github.com/hoxworth/homebrew/25921d95ff10d1b505e933a581e0b4fb8d72d952/Library/Formula/flow-tools.rb

Building on the command line

	[engine] $ make

Building using Xcode 

![Imgur](http://i.imgur.com/NrB3s.png) 

Running on the command line

	[engine] $ ./flowy-engine --help
	[engine] $ ./flowy-engine $TRACE

Running using Xcode

Goto Product &rarr; Edit Scheme to Arguments.

![Imgur](http://i.imgur.com/vRdSN.png)

Resources:

(1) [`flow-tools` on `AMD64` &rarr;](http://mthesis.vaibhavbajpai.com/post/19183305904/flow-tools-on-amd64)
