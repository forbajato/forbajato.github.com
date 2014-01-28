---
layout: post
title: "Fixing i3status and window titles"
description: ""
summary: "The status bar for i3 is nice, very useful and a little ugly.  Did a bit more reading and found a great, easy way to fix the font and make it more attractive."
category: "Blog"
tags: [window managers, i3, challenge]
---
{% include JB/setup %}

#Everything is configurable

i3 allows you to configure and change pretty much everything. I am still loving using the window manager and have found it can make my work flow faster and much more efficiently.  Ran across [this post](http://xpressrazor.wordpress.com/2014/01/27/introduction-to-i3/) with some more tips on using and customizing i3.

#i3status

i3status is documented separately from the normal i3 config, check `man i3status` for the man page.  You can change what items are on the status line through the config file (located at `/etc/i3status.conf`).  You can change how it appears in the normal i3 conf file though - located at `.i3/conf`.  I made a few changes to the `bar` section of the i3 configuration file:

    bar {
        status_command i3status
        font pango:Droid Sans, Ioicons, FontAwesome 8
        colors {
            background #000000
            statusline #33aaff
            focused_workspace #11aaff #005500
            active_workspace #11aaff #005500
            urgent_workspace #ffffff #990000
            }
    }

#Even change the window title fonts!

OK, so this isn't really hidden but up in the `.i3/config` there is a section that defines the font for the window titles.  Look for the `# Font for window titles` section.  I changed the font line to give me Verdana font for my window titles:

    font pango:Verdana 10

Nothing to it!
