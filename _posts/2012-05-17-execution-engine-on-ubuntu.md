---
layout: post
title: "Execution Engine on Ubuntu"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Tried on Ubuntu 10.04 (LTS) x86_64 and 12.04 (LTS) x86_64

Install CMake

	>> sudo apt-get install cmake
	
Install Flow-tool Development Package

	>> sudo apt-get install flow-tools-dev	
	
Install Compression Library Development Package

	>> sudo apt-get install zlib1g-dev

Install JSON Manipulation Library Development Package

	>> sudo apt-get install libjson0-dev

Build Engine

	[engine] >> make

Run Engine

	[engine] >> bin/engine examples/query-http-tcp-session.json examples/trace.ft

Install Doxygen (optional)

	>> sudo apt-get install doxygen
	
Install GraphVIZ (optional)

	>> sudo apt-get install graphviz
	
Generate Documentation (optional)	

	[engine] >> make doc
	
Cleanup
	
	[engine] >> make clean