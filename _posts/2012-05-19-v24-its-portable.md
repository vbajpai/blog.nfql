---
layout: post
title: "v2.4: it's portable"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Summary: (since after `v0.3`)

    >> git show v0.4
    
    tag v0.4
    Tagger: Vaibhav Bajpai <contact@vaibhavbajpai.com>
    Date:   Fri May 18 15:07:42 2012 +0200
    Commit 00c17385e37dd944c9139205a5eb3660c707858a	
    
    *  _GNU_SOURCE feature test MACRO and -std=c99
    *  (__FreeBSD, __APPLE__) and __linux MACROS around qsort_r(…)
    *  reverted to a flat source structure for the CMake build process.
    *  CMake command to call a script to create auto-generated sources and headers.
    *  CMake command to call a scripts in queries/ to put JSON queries in examples/
    *  Makefile to automate invocation of CMake commands.
    *  installation instruction for Ubuntu.
    *  installation instruction for Mac OS X.
