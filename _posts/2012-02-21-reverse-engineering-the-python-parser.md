---
layout: post
title: "Reverse Engineering the Python Parser"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Install GraphVIZ

	$ brew install graphviz
	$ brew linkapps

Install PyLint

	$ pip install pylint


Generate UML Diagrams using PyReverse

	$ pyreverse -o png -p parser parser/

UML Diagram of the Python Parser

[Packages &rarr;](http://www.vaibhavbajpai.com/documents/thesis/docs/docs-parser/packages-parser.png)

[Classes &rarr;](http://www.vaibhavbajpai.com/documents/thesis/docs/docs-parser/classes-parser.png)
