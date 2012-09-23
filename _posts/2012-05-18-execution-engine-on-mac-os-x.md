---
layout: post
title: "Execution Engine on Mac OS X"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Tried on Mac OS X 10.7.

Install [Homebrew &rarr;](http://mxcl.github.com/homebrew/)

Install CMake

	>> brew install cmake
	
Install Flow-Tools from source

	>> wget http://dl.dropbox.com/u/500389/flow-tools-0.68.4.tar.bz2
	>> tar -xvf flow-tools-0.68.4.tar.bz2

	[flow-tools-0.68.4] >> ./configure
	[flow-tools-0.68.4] >> make 
	[flow-tools-0.68.4] >> make install	
	
Install JSON Manipulation Library Package

	>> brew install json-c
	
Build Engine

	[engine] >> make CMAKE_PREFIX_PATH=/usr/local/flow-tools/

	
Run Engine

	[engine] >> bin/engine examples/query-http-tcp-session.json examples/trace.ft
	
	
Install Doxygen (optional)

	>> brew install doxygen

Install GraphVIZ (optional)

	>> brew install graphviz
	
Generate Documentation (optional)

	[engine] >> make doc	

Cleanup
	
	[engine] >> make clean
