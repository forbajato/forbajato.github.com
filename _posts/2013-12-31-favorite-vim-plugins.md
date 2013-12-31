---
layout: post
title: "Favorite Vim Plugins"
description: "My most used Vim plugins as of right now"
category: "Blog"
tags: [vim, programming, plugins]
---
{% include JB/setup %}

#Introduction

I love Vim.  You don't have to go too far on the internet to see that there are two raging camps out there.  I admit to never having tried emacs but, having spent so much time learning and tweaking vim I am unlikely to try it now.  What I like about vim:

1. It gets out of my way - OK, there is a learning curve.  Once you get basic movement, search, copy and replace down though vim stays out of your way.  The screen is blank with a flashing cursor - nothing to distract.
1. It is amazingly extensible - I suspect this is true for emacs as well.  I find that whenever I am doing a task repeatedly there is either an easy way to automate it in vim or someone has already written a plugin to make it faster.
1. Vi is everywhere - most all Linux based systems will have some form of Vi installed by default.  Vim builds on Vi but knowing the Vim basics means I can administer systems that have just started up easily and using familiar keybindings.

As great as Vim is there are things it doesn't do easily, that is where plugins come in.  I have never written a plugin myself but since there are so many smart people out there who make their plugins available I haven't needed to write anything from scratch.  Following is a list of my most used plugins.

#Pathogen - where it all should begin

Getting Vim plugins to work is not generally difficult but sometimes there are a number of files that have to be put in the correct spots in the file tree.  Along came [Pathogen](https://github.com/tpope/vim-pathogen).  This amazing little plugin allows you to install and manage plugins for Vim in their own private directories.  After a bit of configuring (see the Pathogen git repo for instructions) installing a plugin (like, say NerdTree) is simple:

    cd ~/.vim/bundle
	 git clone https://github.com/scrooloose/nerdrtee.git

Simple!

#Plugins I installed using Pathogen

Before I discovered Pathogen I installed a few plugins and will discuss those below but first let's deal with the Pathogen managed plugins.

##Multiple Search 2

Ever wish you could do a search for a term then search a different term without loosing the highlighting on the first term?  If not, move on, nothing to see here - but if so, checkout [MultipleSearch2](https://github.com/vim-scripts/MultipleSearch2.vim).  Really handy if you need to do multiple searches through your document and keep them both accessible.

##Nerd Commenter

Sometimes when you are coding you need to comment out something for the purposes of debugging.  I know when I run tests I just need to get some pesky code out of the way to see where things are going wrong.

Enter [NerdCommenter](https://github.com/scrooloose/nerdcommenter) - highlight some text you want commented, hit a quick key combination and you have commented out your code.  This thing even knows how to comment out different types of code!  I use this every coding session.

##Nerd Tree

OK, you are editing some code, building your killer Django app and you realize you need to check the model (does that ever happen to anyone else?).  If the directory tree is simple you can just open the file you need but if it is a complicated directory tree using a tree navigator can be really helpful.  [NerdTree](https://github.com/scrooloose/nerdtree) fits the bill.

##Tagbar

Sometimes I need to move through my code quickly to find a class, function, variable, etc.  Folds can help with this but wouldn't it be nice if something could keep track for you automatically?  [Tagbar](https://github.com/majutsushi/tagbar) opens a window (on the right by default) with a list of the code tags in your file, move up and down the window to jump directly to the class, function, etc. you need to edit.

##Snipmate

While I haven't used this in coding much (I do a lot of python coding and don't find a need for this there) I do use it in writing papers, developing slides and writing tests in LaTeX.  [SnipMate](https://github.com/msanders/snipmate.vim) has a host of predefined code snipits that it can insert into your text with a simple command.  When you are writing slide environments for beamer or doing test questions for eqexam (LaTeX plugins) this can save lots of time, also helps to ensure I get the tags correct when I have not used the language for a while.

##Vim-Surround

Like snipmate this one is designed to get you coding quickly.  Anyone who does html will benefit from this - [Vim-Surround](https://github.com/tpope/vim-surround) will simple surround your highlighted text with a particular tag.

#Essential non-pathogen plugins

I came late to the pathogen party so I have a few plugins I still use from before.  It may be that these are usable as pathogen plugins now but I installed the manually.

##[Python.vim](http://www.vim.org/scripts/script.php?script_id=790)

Most programming languages have their own plugins to help with syntax highlighting, code testing, etc.  Python is no different.

##[Potwiki](https://github.com/vim-scripts/potwiki.vim)

Ever wanted to turn Vim into a text based wiki?  This one will handle it for you!

##[Tasklist](https://github.com/vim-scripts/TaskList.vim)

Add GTD like task list support to your text based wiki and you have a powerful productivity tool (remember - greater productivity means more time for Steam games).

There you have it - my favorite and most used Vim plugins.  There are tons more, if I missed one that you particularly love make your case for it in the comments!
