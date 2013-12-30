---
layout: post
title: "Finding Non Unique Elements in a list with python"
description: "Solving the first puzzle on Checkio"
summary: "A few ways to solve the first Checkio problem in python."
category: Python
tags: [python, puzzle, checkio, learning, tutorial]
---
{% include JB/setup %}

#Introduction

I am always looking for ways to keep learning different things.  I love programming and my most common language of choice is python (though I have played with arduino, processing, javascript and bash).  Fortunately there is no shortage of web based tutorials and challenges to get you going in Python programming.

Recently I stumbled on the [Checkio](http://www.checkio.org) site.  This is like the [Python Challenge](http://pythonchallenge.com) site in that there are staged puzzles that you must solve in order to move forward.  Unlike the Python Challenge each level has a number of puzzles that you can choose from so if you are flailing away at one problem you can always switch to another to keep moving forward and keep learning.

#The puzzle

The first puzzle on the site seems to be the "Non-unique Elements" puzzle.  In it you have to take a list of arbitrary length and find all the elements in it that are not unique.  For instance the list [1,1,3,3,2] would return a list of only the non-unique elements [1,1,3,3] and discard the 2.  

#First solution (i.e. "The inelegant one!")

I always fall into this trap - thinking I have to build a list from another list.  It works but is clunky and makes the code rather long.  Anyway this worked:

    def checkio(data):
        ans = []
        for r in data:
            if data.count(r) > 1:
                ans.append(r)
        return ans

All the function is doing is setting up an empty list (to collect the answers) then looping through the list passed in to it and counting the number of times each element shows up in the original list.  If that count is greater than 1 (i.e. the element is non-unique) then the element is appended to the answer list.  Finally the answer list is returned.

This has a number of problems.

1. It is rather long - too many lines of code for something that could be done in a much simpler way.
1. Nagging feeling that there is a better way - ever have that nagging feeling that you are doing something you have done wrong before and know there is a better way?  This gave me that feeling too.
1. Needlessly counting every element as it is encountered - why should you have to count the first element again when it is encountered the second time in the list?  You already know it is in the answer list.  For short lists this is not important but if you are dealing with large datasets this could be annoying and time consuming.

#Using a list comprehension

OK, the nagging went away when I realized the better solution is with a list comprehension. You think of a list comprehension as a way to make a list from a data set and should always think of it when you have the pattern:

    ans=[]
	 <do something>
	 ans.append(list_element)

If you are looping and doing a lot of "loop.appending" there is a better way.  List comprehensions can also have a conditional tied to them so here is a much better way (still not the best, you have to solve the puzzle yourself to see the best submitted answer!).

    def checkio_list_comprehension(data):
        ans = [r for r in data if data.count(r) > 1]
        return ans

So much simpler, easier to read (assuming you know about list comprehensions which, now you do!) and easier to debug.  You could even set the return to the list comprehension and eliminate a line of code further.

I still don't have a great answer to how to keep from checking repeated elements repeatedly.  If you have a thought leave a comment!

