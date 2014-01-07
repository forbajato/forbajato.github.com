---
layout: post
title: "i3 Challenge"
description: "Checking out the tiling window manager i3"
summary: "Tiling window managers have always intrigued me.  I have tried a number of them but never for long enough to really get into them.  Now I am starting the new year (well, ending 2013 too) with a one week challenge to use i3 as my window manager.  Here is the chronicle of how that went."
category: "Blog"
tags: [window managers, i3, challenge]
---
{% include JB/setup %}

#Rationale

Tiling window managers are interesting.  I have tried out dwm, awesome and i3 in the past but only ever really spent time with awesome.  Awesome has had a tendency to make rather jarring changes in how they configure the WM and I have never really mastered it.  When I recently checked out [i3](http://i3wm.org) I decided to give it a go.

Why use a tiling window manager?  Well, I find that I spend most of my time with my applications maximized.  Even though KDE has a great desktop with widgets galore available, I find that I don't use them much because my browser or terminal is maximized and covers up all those cool widgets.  Since I am covering up those widgets anyway, why use them?

I love Vim, I love that I can use the editor without my fingers leaving the keyboard.  I have installed Pentadactyl to allow a similar functionality in Firefox.  What if the whole window manager could be driven with keyboard only rather than having to reach for the mouse every couple of seconds?

Add to that my tendency to use computers way past their "sell by" dates (my eeePC is over 7 years old).  Tiling window managers are well known for being light on resources.  All in all I have good reason to want to check out this sort of interface.

#Installing i3

In Ubuntu installing i3 was a breeze:

    sudo apt-get install i3

There is a metapackage that handles installing i3 and some other goodies (like i3lock, i3status and dmenu).  Dmenu is essential as far as I am concerned because otherwise you have to launch every application through a terminal, either spawning a lot of terminals or using the "&" switch a lot at the end of your commands.

#Configuration

i3 comes with the default $mod key being either the `<Alt>` key or the `<Super>` key (the one with an Ubunutu logo on my System76 laptop but probably has some Windows related logo on other keyboards).  There is a great overview of configuration and use of i3 on Youtube posted by [gotbletu](http://www.youtube.com/watch?v=yAq_Enj_d2Q).  I started there and have made some tweaks that fit my work flow.

I like using the `<Alt>` key for my $mod key so at the top of the .i3/config file you set:

    set $mod Mod1

If you want to use the `<Super>` key instead just set that to Mod4.

I love Synapse, the program launcher that I have used for years in XFCE4, KDE and even Unity.  Firing it up is now part of my muscle memory and loosing that would be a shame.  So to start dmenu I need to have it use the same key combination that I use to access Synapse in the other desk top environments:

    bindsym Mod4+space exec dmenu_run

I complete agree with gotbletu, making the moving focus to be vim compatible is very helpful.  Search down the config file for the line "# change focus" and make some changes:

    bindsym $mod+h focus left
	 bindsym $mod+j focus down
	 bindsym $mod+k focus up
	 bindsym $mod+l focus right

I made the similar changes in the "# move focused window" section.

The maker of the youtube video referenced above sees things differently than I do.  I think that when you do a "split vertical" you should end up with two windows, one stacked on top of the other in vertical alignment.  Apparently the makers of i3 agree so they set up the splits with $mod+v giving a vertical split and $mod+h giving a horizontal split.  That would be great but we just defined $mod+h to focus left.  If you tried to restart i3 with the new config after making the focus changes you got an error, i3 won't let you have two actions bound to the same keybindings (thanks to the devs for that!).  So I remapped the split horizontal to $mod+b, the 'b' key is right next to the 'v' key in a qwerty keyboard layout so that made sense to me.

    bindsym $mod+b split h

Now I love keyboard bindings and try to learn a few bindings for common tasks on my most used applications on a routine basis.  My hands have gotten used to the `<Alt>fd` combination to send unsent mail in Thunderbird.  Unfortunately i3 has $mod+f mapped to go full screen.  Easy enough to change, we will just map that to the Ubuntu key instead of the `<Alt>` key:

    bindsym Mod4+f fullscreen

Notice to map it to the `<Super>` key I used the full name of the key, not the variable (i.e. not "$mod4").

I have been using a 4 workspace model with KDE for sometime now - terminal (either in a workspace or using guake), web (browser, email client, etc.), fun (gotta have a place for Steam!) and media.  With i3 you can name those workspaces and the key combinations to jump to them.  Search down the config file for the "# switch to workspace" line and add:

    bindsym $mod+1 workspace 1: terminal
	 bindsym $mod+2 workspace 2: web
	 bindsym $mod+3 workspace 3: fun
	 bindsym $mod+4 workspace 4: media

It is important to note that the name of the workspace here starts with the digit following the word "workspace", anything you put out there will name the space. Another thing to keep in mind is that you are creating new workspaces with this set of commands.  What that means is that even though you have a workspace named "1: terminal" that doesn't mean you can't have another workspace named "1".  I discovered this when trying to move an application to workspace 1 when I already had workspace "1: terminal" - i3 created another workspace named "1" and moved my application there.  As such, in order to avoid confusion you may want to make similar changes to the move to workspace section.

    bindsym $mod+Shift+1 move container to workspace 1: terminal
    bindsym $mod+Shift+2 move container to workspace 2: web
    bindsym $mod+Shift+3 move container to workspace 3: fun
    bindsym $mod+Shift+4 move container to workspace 4: media

It sure would be nice if you could define certain applications to always open in certain workspaces, wouldn't it?  i3 (and other tiling window managers as well) has that covered.

    assign [class="Mcomix"] 4: media
    assign [class="Calibre"] 4: media
    assign [class="Gpodder"] 4: media
    assign [class="Vlc"] 4: media
    assign [class="Steam"] 3: fun
    assign [class="Firefox"] 2: web
    assign [class="Thunderbird"] 2: web

How do you find what to put in the class="?" section?  Use xprop or xwininfo. All you have to do is fire up xprop in a terminal then click on the window you need the class information for. Look through the output for the line that starts WM_CLASS(STRING), it will look something like this:

    WM_CLASS(STRING) = "Navigator", "Firefox"

Use one of those names in the class variable above.

Now I like to cycle though workspaces sometimes rather than jumping directly to the space I need.  i3 doesn't do that by default but let's face it - we control the machine, not the other way around!  So the i3 devs gave a way to set this up:

    bindsym $mod+n workspace next
	 bindsym $mod+p workspace previous

Ain't open source wonderful?!?

Now, with a tiling window manager you won't be seeing your wallpaper often but just in case you want some interesting wallpaper you can use feh to handle that for you:

    exec feh --bg-scale /path/to/wallpaper/file.png

There are a number of applications that I can't live without, you can set them to autostart when i3 fires up:

	 # start clipboard manager
    exec parcellite
	 # gotta turn off that mousepad when I am typing, glad KDE came with synaptiks to make that easy to configure!
	 exec synaptiks
	 # there are probably people who can live without a drop down terminal, I am not one of them
	 exec sleep 3 && guake

There are still a few things I haven't figured out yet, chief among them is volume control with the multimedia keys.  Other than that little annoyance I am thoroughly enjoying my i3 experience (having used it exclusively for a week), even my wife is tolerating it!  If you have other thoughts on tweaks for i3 or config ideas please feel free to leave them in the comments.
