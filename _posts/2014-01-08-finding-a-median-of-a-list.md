---
layout: post
title: "Finding a median of a list"
description: "Moving on to my second Checkio challenge"
summary: "If given a list of numbers, how can you use python to find the median?"
category: "Python"
tags: [checkio, tutorial, python, puzzle, learning]
---
{% include JB/setup %}

#The Puzzle

The description on the website of a median is not too clear to me, fortunately I have a little bit of statistics in my educational background.  The median is the middle value of a data set.  When you are at the median 50% of the data should be higher than that value, 50% should be lower.  In order to find a median you need to have an ordered data set and then simply find the middle value.  The rub comes with data sets with an even number of elements.  What is the middle value of the list `[1,2,3,4]`?  Simply take the two values in the center and get their average.

#Odd numbered lists are easy

Given a data set like `[1,2,3,4,5]` you can eyeball the solution, the 3 is obviously in the center (with two digits above and 2 digits below).  Python can simply check the length of the list:

    l = [1,2,3,4,5]
    len(l)

In this case Python will return 5.  Remember that Python lists are 0 indexed (i.e. the first element is element number 0) so the median in this list will be element number 2.  How is 2 related to five?  Well it is the whole number portion of the result of dividing 5 in half.  In Python 2.7 this is the result of the division `5/2` (called "integer division").  If you want the remainder portion you have to do a modulo `5%2` (answer will be 1 because 5 divided by 2 is 2 with one left over). 

So to get the median all you need is the number in the number 2 position of the list.  Even better, all you need is the element in the number 5/2 position of the list.  

But wait, where did the 5 come from?  Actually, all we need is the element in the number len(l)/2 position of the list. It is time to make a function that will take a list as an input and give us the middle element:

    def median(input_list):
        return input_list[len(input_list)/2]

Simple (at least for the easiest case!).  Now, this will only work if a list has an odd number of elements.  How can we check?  The modulo operator helps here.  If you divide an even number by 2 you will never get a remainder.  In Python that means the modulo of the division will be 0. We can use that to ensure we are working with an odd numbered list.

    def median(input_list):
        if len(input_list) % 2 == 1:
            return input_list[len(input_list)/2]

OK, now we are checking to be sure the list has an odd number of elements in it - the modulo result of an odd number and 2 will be 1 (there should always be a remainder if dividing an odd number by 2, right?).  But what if someone gives us a list of digits that are out of order and wants the median?  Fortunately Python can sort a list for you.  This sort takes place in place, in other words the original list is replaced by a sorted version:

    l = [1,2,3,3,2,1,4,4,3,2]
    l.sort()

This results in the list l now being `[1,2,2,2,3,3,3,4,4]`.  Once the list is sorted it is easy to pass to our growing function to get the median:

    def median(input_list):
        input_list.sort()
        if len(input_list) % 2 == 1:
            return input_list[len(input_list)/2]
    
Almost there, one possible case to consider first - lists with an even number of elements.

#What about even numbered lists?

What if the list we need to investigate has an even number of elements?  First of all, how can we find out?  Simple - just check the modulo.  If the modulo result of dividing the length of the list by 2 is 0 (i.e. there is no remainder) you know the list has an even number of elements.

The median of a list with an even number of elements is simply the average of the two middle most elements.  How can you use list methods to find the two middle most elements?  Consider the simple list `[1,2,3,4]`.  This list has a len of 4.  If you divide the len by 2 you get 2.  If you chose the element number 2 (from len(list)/2) you would get 3.  So you are half way there.  All you need is the element before that one and then take the average.  We can modify our function one more time (if statements just cry out for an `else` clause don't they?):


    def median(input_list):
        input_list.sort()
        if len(input_list) % 2 == 1:
            return input_list[len(input_list)/2]
        else:
            return (input_list[len(input_list)/2]+input_list[len(input_list)/2 -1])/2.0

You will note we used the 2.0 in the final statement, this is to ensure that Python uses real division, giving us a number with a decimal as the result.

There are ways to make this shorter but, to me, the code then gets more difficult to read.  One of the advantages of a clear language like Python is that you can tell what is happening just by reading the code, thus don't have to document quite as much.

Keep hacking!
