---
layout: post
title: "Packaging for Mac OS X Homebrew"
description: ""
category: 
tags: []
---
{% include JB/setup %}

This post jots down some notes I would like to remember during the
packaging process:

#### json-c v0.10 

`json-c` starting from `v0.11` has renamed the library from `libjson` to
`libjson-c`. Debian-based systems are currently providing `v0.10` in the
stable repository. Therefore the current `cmake` is made to search
`libjson` which breaks on Mac OS X. This is because Homebrew now ships
`v0.11`. The homebrew packaging system does not allow one to specify the
version in the package dependency, unless the pacakage is shipped
through
[homebrew/versions](https://github.com/Homebrew/homebrew-versions).
There are two options at this stage:

- Edit `cmake` to allow searching for both `libjson` and `libjson-c`
and use whichever is available. I could not find a way to do this.

- Stick to `v0.10` and make Homebrew provide a `v0.10` version through
the homebrew/versions repository.

I have gone with the latter  and sent a [pull
request](https://github.com/Homebrew/homebrew-versions/pull/320) for
`v.10`. Let's see  ...

#### Makefiles within CMake

In NFQL `v0.7.1`, I have a custom Makefile to automate the `cmake` build
process. This is useful for people unfamiliar with `cmake` and also
helps quickly install the software when manually building it.  However,
the Makefile interferes when building through Homebrew (and possibly
other distributions). There are 2 action items:

- Remove `Makefile` before the next release.
- Update the installation instructions to use `cmake` commands.

#### gettext keg

The homebrew `gettext` keg was required to be force linked to allow
linking with `-lintl` library. 

     $ brew link --force gettext

However, the homebrew package will automatically link the keg during the
package build and the above need to be required.
