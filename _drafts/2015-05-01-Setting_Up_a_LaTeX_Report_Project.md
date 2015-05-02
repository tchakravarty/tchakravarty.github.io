---
layout: post
title:  "Setting up a LaTeX Report Project"
date:   2015-03-06 00:29:38
categories: latex
---

It has been a while since I needed to get any work done in $$ \LaTeX $$, but I was recently working through Bruce Hansen's as yet unpublished econometrics textbook, [Econometrics](http://www.ssc.wisc.edu/~bhansen/econometrics/), and I decided that it might be useful to type them up. I do have a set of $$ \LaTeX $$ macros from grad school, but given that it has been a while since I last used them, and have largely forgotten _why_ some of the macros were written the way they are written, I decided to start again from scratch, and re-write the macros that I might use in this project.

 Here is a first set of things that I need to do to get this going:

 1. Make a new `problem` and `solution` environment, and create a new `problemcount` counter. My assumption was that every time the counter is used, it is auto-incremented, but the counter has to be managed by the user.
 2. Create a revision history. Add the last compiled information.