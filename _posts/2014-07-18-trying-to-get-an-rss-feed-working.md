---
layout: post
title: "Trying to get an RSS feed working"
description: "I wonder if this thing will work"
category: ""
tags: [blog, RSS, tutorial]
---
{% include JB/setup %}

#Github pages seems to be ignoring my RSS feed

Not only that but it seems to be ignoring all my additions to the index.md file.  I wonder how to force it to recompile that file.  When I make additions and do `jekyll build` the site works as expected.  When I make changes and push to github there are no changes evident.

I am wondering if the thing only recompiles everything after a blog post has been added.  So I am adding this stub to see if I can force github to act as I expect (i.e. update index.html when a change is made).
