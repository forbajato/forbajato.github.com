---
layout: post
title: "Python Puzzle   Counting characters in a string"
description: "More Checkio Python Puzzlers"
category: "Count the number of times a character occurs in a string, return the most frequently occurring character"
tags: [checkio, tutorial, python, puzzle, learning]
---
{% include JB/setup %}

#The Puzzle

This is a classic beginning python puzzler - if you are given a list of characters, how many times does "a" (or any other given character) occur?  The usefulness is obvious if you are thinking about big data sets - say you want to find rarely occurring letters or the most commonly occurring ones.  The principle would be the same if you were looking for words or nucleotide base sequences too I suppose.  The Checkio puzzle (found [here](http://www.checkio.org/mission/most-wanted-letter/)) states you need to find the most commonly occurring letter and report it.  If all the letters are the same frequency then report the earliest letter in the alphabet.

#Counting with dictionaries

A pretty common pattern to figure out the frequency of things is to count them in a dictionary.  All you have to do is set up an empty dictionary and then accumulate items to it.  Whenever a copy of the item (in the dictionary this will become what we call a "key") is already in the dictionary, increase that item's value by one.  If the item is not yet in the dictionary, add it.

So, first set up an empty dictionary:

    chars = {}

Next you will want to loop over a string (we will call ours "text" since "string" is a reserved word in Python) and check to see if each character in the string is already in the dictionary or not:

    for character in text:
        chars[character] = chars.get(character,0)+1

Pretty simple!  Now, what is going on here?  We started the `for` loop, telling it to loop over all the characters in the string "text".  The `chars.get` looks to see if the character is in the dictionary already. If the character is not in the dictionary already then it is added, if it is there then its value is increased by 1.  When the `for` loop is finished we will have a frequency table of characters in the string.

What if we want to make sure we only count letters (no special characters like spaces or punctuation)?  Simple just add:

    if character.isalpha

to the `for` loop.  Want to report only lower case characters?  A single character from a string of characters still gets all the string methods available in the standard library.  As such you can convert it and to other interesting things as needed.  To convert each character into the lower case version  before checking it in the dictionary:

    character = character.lower()

Now, putting it all together:

    chars = {}
    for character in text:
        character = character.lower()
        if character.isalpha():
            chars[character] = chars.get(character,0)+1

When that part of the function finishes we have a dictionary (called `chars`) that has key:value pairs of each letter occurring in the string as the key and the number of times it occurs as the value.

Now we need to find the key with the highest value.  It would be great if we could use something like the `max` method you use on iterables but we are using a dictionary. But what if we took just the values in the dictionary, made them an iterable, found the highest one and the looked to see what key went with that value?

    for k,v in chars.iteritems():
        if v == max(chars.values()):
            return k

Nothing to it.  The `chars.iteritems()` turns our dictionary into an iterable.  Then we look at the values in the dictionary, find the highest level one, look up its key and Voila! we have the letter that occurs most frequently in our text.

That seems like a lot of work though.  I am sure you can find a better way to do this, maybe exploring other iterables (like sets) and how the `max` function works.
