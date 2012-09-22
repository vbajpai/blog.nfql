---
layout: post
title: "Refactoring the Execution Engine"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Refactoring the execution engine.

- Added a <s>`base_header`</s> `base` for common lib includes (1).

![base-header](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/base_8h__incl.png)

- Added `error_functions` for common error reporting, currently being used by `flowy` (2).
- Indented the whole project to use 2 spaces for a tab.
- Added `grouper_fptr, merger_fptr` headers (why were they missing again :-/) for the corresponding sources (3, 4).
- Added more details in the Doxygen documentation, added documentation for `ftlib` (5).

![flow-query](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/structflowquery__coll__graph.png)

- Moved all external lib includes in the header for consistency, added include-guards in few missed headers.
- Separated out pipeline structs in `pipeline` header to resolve circular dependencies to avoid forward declarations (6)

![pipeline](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/pipeline_8h__dep__incl.png)

- Updated `fun_gen` to reflect aforementioned resolution of circular dependencies (7)



Resources -

(1) [`base_header` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/base__header_8h.html)

(2) [`error_functions` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/error__functions_8h.html)

(3) [`merger_fptr` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/merger__fptr_8h.html)

(4) [`grouper_fptr` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/grouper__fptr_8h.html)

(5) [`ftlib` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/ftlib_8h.html)

(6) [`pipeline` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/pipeline_8h.html)

(7) [`fun_gen` reference &rarr;](http://dl.dropbox.com/u/500389/mthesis/docs-engine/html/namespacefun__gen.html)

