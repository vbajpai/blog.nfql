---
layout: post
title: "Parser on Mac OS X"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Install [Homebrew &rarr;](http://mxcl.github.com/homebrew/)

Install Python

	$ brew install python --framework
	
Put `easy_install` in PATH

	$ export PATH=/usr/local/share/python:$PATH
	
Setup the Python Packaging Virtual Environment

    $ easy_install pip
    $ pip install pip --upgrade
    $ pip install virtualenv
    $ pip install virtualenvwrapper
    $ source /usr/local/bin/virtualenvwrapper.sh
    
Install External Dependencies

	$ brew install hdf5
	$ brew install lzo   
    
Create a Virtual Environment

	[parser] $ mkvirtualenv parser

Install Python Dependencies

	(parser)
	[parser] $ make
	
List the Installed Dependencies

	(parser)
	[parser] $ pip freeze	
	
Cleanup:

Remove the build files

	(parser)
	[parser] $ make clean

Deactivate the Virtual Environment

	(parser)
	[parser] $ deactivate
	
Destroy the Virtual Environment

	[parser] $ rmvirtualenv parser
