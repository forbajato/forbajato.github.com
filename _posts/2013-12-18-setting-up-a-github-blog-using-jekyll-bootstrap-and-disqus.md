---
layout: post
title: "Setting up a github blog using Jekyll Bootstrap and Disqus"
description: "How I did it in as simple language as possible"
category: Blog
tags: [jekyll, disqus, tutorial]
---
{% include JB/setup %}

#Rationale

Ever have a problem with your computer, robot or other tech toy?  If you are like me, you go to [Duck Duck Go](http://duckduckgo.com) and search out an answer, try out a few of the first hits, find the perfect solution, tutorial, etc., fix the problem then promptly forget it.  Next time you run into the problem you have to cycle through the steps again.  I am tired of the "lather, rinse, repeat" cycle of technology problem solving - time to start documenting all the best answers someplace that I know I can find again, in a system that I can control and one that I can use to get feedback from a community of nerds to improve my process.  Thus this blog was born!

I had heard about Twitter bootstrap before and what it promised sounded really good - a way to set up the design of websites so that they look good (something important to me but not important enough to inspire me to figure out the ins and outs of design) and can be rendered on multiple platforms (computer, tablet, phone) nicely.  When I found that Github has a thing that allows static pages websites and that you can use Jekyll-Bootstrap to make the sites I was sold.  I had to figure out how to set this up and use it to start a tech blog to document all my tutorial finds, technology configuration wins, etc.

This first post on the new blog will document how to set up the beast.  In the process of starting this I tried multiple tutorials on Jekyll-Bootstrap (and variations) as well as Pelican.  I liked the idea of Pelican since it is Python based and I know a little Python but in the end Jekyll-Bootstrap turned out to be the one that I managed to make work.  Getting this hosted on Github was a natural fit.

Without further adieu, here is how I managed to make it work.  As always, your mileage may vary but if you have questions on the process documented here or ideas to improve the process please leave a comment!

#Prepare Github

[Github](http://github.com) has a static web pages hosting service.  Through this service you can host static web sites, that would be sites that do not interact a lot with the user but simply display information.  If you want a simple blog this is a great way to go.  Add to that you can control your own content, develop your content locally and manage versions (through git) and you have a pretty great package.

If you don't have a github account yet you will want to go [here](http://github.com) and sign up for one. The accounts are free and afterwards you can put all kinds of projects under version control.  Do keep in mind that if you want an account that hides code (or blog articles) you will need a paid account.  If you want private repositories for code development you may want to try [Bitbucket](http://bitbucket.org).  Bitbucket allows git and mercurial repos but doesn't have the static webpage hosting service.

Once you have your github account you will need a repository to host the blog.  Click the "Create a new repo" icon (top right hand side, next to your username). Name your repo something like USERNAME.github.com.  It turns out you can probably put most anything you want in for USERNAME but most examples use your github username, I know that works as that is what I did.

At this point the repo isn't fully initialized - there are no files under version control (unless you chose to initialize it when creating by checking the "Initialize this repository with a README" box when creating the repo).  Doing it this way is probably the easiest as you don't end up with colliding repos.  Next you want to go to the Jekyll-Bootstrap startup page to get things going with Jekyll-Bootstrap.

If none of this is working and you need a little help getting started or getting familiar with git, github has some great [help pages](https://help.github.com).  There are also some great tutorials for using git around the web, I started with the free one from [Code School](http://codeschool.com/courses/try-git).

#Set up Jekyll-Bootstrap

Once you have your github repo set up it is time to head over to [Jekyll Bootstrap](http://jekyllbootstrap.com).  Jekyll Bootstrap is designed to get you up and running with a blog on github as fast as possible.  I have tried ruhoh (linked at the bottom of the JB front page) but that didn't really work well for me.  Jekyll Bootstrap had me up and running, theming and creating content very quickly.  My needs are simple, again, your mileage may vary.

Click the "0 to Blog in 3 Minutes" button on the Jekyll Bootstrap page.  This will send you to a form that will start giving you code to enter into the command line to get JB set up.  If you put your github username in the box at the top of the page it will even customize the code for you.

You will notice that step two involves some git commands - again, if you are not familiar with git you may want to do the tutorial linked above to get your head around git.  If you follow these steps exactly and have already initialized the repo with a README you may run into a problem when you try to push the local repo to github.

    git push origin master

You will get an error stating that the repo on github's server is ahead of the repo on your local machine. You will need to do a pull, merge, then push:

    git pull
	 git merge
	 git push origin master

That should set things straight again.

I didn't bother with the "Run Jekyll locally" steps but if you want to (and are using Ubuntu) you will need to install a few things first:

    sudo apt-get install ruby rubygems

I don't know enough ruby to say but after installing these it looks like installing other ruby packages is simply

    gem install <package>

I found installing rake (kind of like make but for ruby packages as best I can tell) using the Ubuntu repos was helpful.

    sudo apt-get install rake

#Creating posts

Obviously you made the blog to create posts.  Starting a post is really easy:

    rake post title="Hello World"

This will create a markdown file in `_posts` directory with the date and post name followed by the markdown extension.  An example of what you will have:

	---
	layout: post
	title: "Hello World"
	description: ""
	category: ""
	tags: []
	---
	{% include JB/setup %}

Now you will just need to edit that text file to finish the post.  Put in a short description of the post under "description:", add in a category and some tags to track it.  Finally the post will need some text.  Here is where you get to put your Markdown prowess to work.

##So you say you don't know Markdown?

Say it ain't so!  Markdown is a fabulous text markup language that allows you to write your text in human readable form and then translate it into multiple formats.  Someday I will do a post extolling the virtue of [Pandoc](http://johnmacfarlane.net/pandoc/) for making web pages, beamer slide shows and pdf documents out of markdown files.  For now though you don't need to worry about any of that - just need to learn enough Markdown to make a renderable post.

Markdown text is all in plain text (can you say "easy grepping"?  I knew you could!) with a few symbols along the way to help you.  Precede your line of text with a hash mark (#) and you get a headline, increase the number of hash marks for decreasing levels of headlines:

    #Heading 1
	 ##Heading 2
	 ###Heading 3

Want to make a list?  Do you need numbers with that?

    1. Item one
	 1. Item two
	 1. Item three

Don't worry - the server will fix the numbering (getting an idea of how easy this is to use?).  If you don't need enumerated lists you just:

    * Red
	 * Blue
	 * Yellow

There is much more to Markdown than this, head over to [Daring Fireball](http://daringfireball.net/projects/markdown/basics) for more.  The beautiful thing about using Jekyll is that when you push your markdown files (in the `_posts` directory and with a `.md` or `.markdown` file extension) to github, their server will render the files into blog posts for you - magic!

#Set up comments with Disqus

What is a blog without comments?  On a blog like this one I need comments because I am talking about how I do things - how else can people get further help or make suggestions to improve my processes without a form of comments?

Jekyll allows for a number of possible comment engines.  Looking in the `_config.yml` file you see a section that looks like this:

    # Settings for comments helper
     # Set 'provider' to the comment provider you want to use.
     # Set 'provider' to false to turn commenting off globally.
     #
      comments :
        provider : disqus
      disqus :
        short_name : yourdisqusshortname
      livefyre :
        site_id : 123
      intensedebate :
        account : 123abc
      facebook :
        appid : 123
        num_posts: 5
        width: 580
        colorscheme: light

This blog uses [Disqus](http://disqus.com) so to set up comments I went to their site and set up an account.  In setting things up I had to give the blog a name, give Disqus information about the blog (minimal) and set a short name.  When I had that I could put the shortname in the `_config.yml` file.  Next you will need to follow the link for setup instructions for "Universal Code".  This will give you a segment of html that you can place in your website pages to connect your Disqus comment account with the blog.  Put this block of code in your `_layouts/posts.html` file.  This file is a template that grabs code from various places, I simply copied the html from Disqus and pasted it at the bottom of the posts.html file, this places the Disqus comment box at the bottom of all post pages the site generates.

#Checking the post to be sure it will render

Before you push this bad boy to the github servers you may want to check and be sure it is compiling correctly.  If you push a markdown page up to github and it isn't compiling correctly github simply won't make changes to your website and you will be stuck wondering what is going on (like I have been for the past hour or so).

Checking this requires that you have Jekyll installed locally.  I decided to use the gems (I think of these like pip install commands in Python and assume you are getting the freshest packages that way).

    sudo gem install jekyll

This install will take a few minutes as a few dependencies need to come along with the jekyll package.  When it is installed you can start a local jekyll instance with a server to see if your markdown compiles correctly.  You need to start this server from the directory that has the site in it:

    cd /home/USERNAME/git-repos/USERNAME.github.com
    jekyll serve

If you get a lot of error messages you may want to parse through this output with a trace:

    jekyll serve -trace | less

I made a few errors and each error takes up several lines so I piped the output through less so I could make changes.

Once you have the markdown on your post correctly written you should see this when you start the server:

    Configuration file: /home/USERNAME/git-repos/USERNAME.github.com/_config.yml
                Source: /home/USERNAME/git-repos/USERNAME.github.com
           Destination: /home/USERNAME/git-repos/USERNAME.github.com/_site
          Generating... Maruku#to_s is deprecated and will be removed or changed in a near-future version of Maruku.
    done.
        Server address: http://0.0.0.0:4000
      Server running... press ctrl-c to stop.

Errors I had made:

1. malformed URL (hope that will improve as I work with markdown more)
2. not putting a `_config.yml` in backticks (the preceding underscore is a special character in markdown and needs to be escaped with backticks)

Now you are ready to update the local repo, commit and push to github.

#Getting the posts on the blog

One of the great things about this approach is you can use your tools on your computer and then push the content to github for hosting.  When you have your posts ready all you need to do is be sure the files are staged for commit:

    git status

This will show you if there are files that are untracked in your local directory.  Add them with

   git add .

Then commit and send to github:

   git commit -am "Commit message, make it something descriptive or you will be sorry later!"
	git push origin master

Note: the -a switch on `git commit` makes sure that all files that are tracked but have changed will also be committed, one of the annoyances of git (for a mercurial user) is that changed files have to be added this way in order to get them to still be tracked.

#Conclusion

There is much more to Jekyll Bootstrap (themes, templates, Liquid, etc.), Markdown (tons of options to format the text, use Pandoc to make lots of different outputs, etc.), and Disqus (connect more blog and other sites for comments, control users, approve comments, etc.).  This should have you up and running with a blog pretty quickly though.  Now that you have something - go create!
