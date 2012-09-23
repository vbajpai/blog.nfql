---
layout: post
title: "Parser on Ubuntu"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Install External Dependencies

	$ sudo apt-get install libhdf5-serial-dev
	$ sudo apt-get install liblzo2-dev


Setup the Python Packaging Virtual Environment

    $ sudo apt-get install python-pip
    $ sudo pip install pip --upgrade
    $ sudo pip install virtualenv
    $ sudo pip install virtualenvwrapper
    
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