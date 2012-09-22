---
layout: post
title: "Installing and Running the Parser"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Install PyTables

	$ brew install hdf5
	$ brew install lzo

	$ pip install numpy
	$ pip install numexpr
	$ pip install cython

Install Python Lex & Yacc

	$ pip install ply==2.5

Install `pyflowtools`

	$ brew install https://raw.github.com/hoxworth/homebrew/25921d95ff10d1b505e933a581e0b4fb8d72d952/Library/Formula/flow-tools.rb
	$ pip install git+https://git.gitorious.org/flow-tools/pyflowtools.git


Install Misc Dependencies
	
	$ pip install netaddr

Freeze

	$ pip freeze  
	Cython==0.15.1
	distribute==0.6.24
	logilab-astng==0.23.1
	logilab-common==0.57.1
	netaddr==0.7.6
	numexpr==2.0.1
	numpy==1.6.1
	ply==2.5
	pyflowtools==0.3.4.1
	pylint==0.25.1
	tables==2.3.1
	wsgiref==0.1.2

Test Run Flowy

	[parser] $ python ft2hdf.py ../../traces/ ft-traces.h5
	[parser] $ python printhdf.py ft-traces.h5
	[parser] $ python print_hdf_in_step.py ft-traces.h5
	[parser] $ python flowy.py query.flw

where `query.flw` refers to the input `ft-traces.h5` and the to-be created `output.h5`
