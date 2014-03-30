---
layout: post
title: "Ipython debugging - where have you been all my life?!?"
description: "I just discovered how to step through code with the Ipython debugger"
summary: "Ipython is a great interactive development environment, since I discovered it I haven't looked back at IDLE.  I have always wondered what the debugger was about though, just read some things that blew my mind!"
category: "Blog"
tags: [python, ipython, debugging, tutorial]
---
{% include JB/setup %}

Ever write a bit of quick code and then run it in your interpreter only to find it error out?  How do you go about debugging?  I generally look at the output, wish I understood it better and then start putting in print statements so I can figure out what is going on at various places in the code.  What if there was a better way?  I had heard about this python debugger (pdb) but had never really understood what it did.  Mike Driscoll, of the Mouse vs. Python blog, has done an awesome job explaining the python debugger (you can check it out [here](http://www.blog.pythonlibrary.org/2014/03/19/pytho-101-an-introduction-to-pythons-debugger/)).  Only problem - I use Ipython so what does this mean to me?

Ipython employs magic commands to do a number of functions, debugging is one of them.  For instance, say you have the same silly code from Driscoll's article:

    def doubler(a):
        result = a*2
        print result
        return result

	def main():
        """"""
        for i in range(1,10):
            doubler(i)

    if __name__ == "__main__":
        print my_result
        main()

OK, I changed the code a bit so that we would throw an exception to demonstrate the %debug magic word.  Run the code in the ipython interpreter:

    %run debug_test.py

If you do this you will get an error like so:

    NameError                                 Traceback (most recent call last)
    /usr/lib/python2.7/dist-packages/IPython/utils/py3compat.pyc in execfile(fname, *where)
        176             else:
        177                 filename = fname
    --> 178             __builtin__.execfile(filename, *where)

    /home/forbajato/pdb_learning/debug_test.py in <module>()
         13 
         14 if __name__ == "__main__":
    ---> 15         print my_result
         16         main()
         17 

    NameError: name 'my_result' is not defined

This is a trivial example and the error is likely obvious but if it wasn't you could use the %debug magic word to figure things out:

    None
    > /home/forbajato/pdb_learning/debug_test.py(15)<module>()
         14 if __name__ == "__main__":
    ---> 15         print my_result
         16         main()

Now you have an arrow pointing to the line with the error in it and a line showing the offending line (that is what that `debug_test.py(15)` means).  The debugger doesn't reproduce the entire trace of the error - that is helpful to me since I figure the standard library (the part pointed to by the `/usr/lib/pyth...` line) is OK and I don't need to change it much.  Now all I have to do is fix that line, in this case we will delete it.

Now when I do a `%run debug_test.py` I everything works great.  What if I wanted to step through it one step at a time.  This could be helpful if the errors are complicated and I need to see the code piece by piece or, alternatively, if the code is someone else's and I am trying to figure out what it does.  Stepping through code line by line can be both useful to debug and educational to study.

To run through the code step by step you can just tell the `%run` magic command to help you with that:

    %run -d debug_test.py

This output will work through the file line by line, putting an arrow at the line currently under execution and showing it to you.  The first line outputs something like this:

    > /home/forbajato/pdb_learning/debug_test.py(3)<module>()
	       2
    1---> 3 def doubler(a):
          4         """"""

Now you can see that the first function, doubler, is being called.  Go to the next line to execute by entering `n` in the `ipdb>` prompt.  All you have to do is work your way through all the steps until you see the total execution.

Other useful commands for the ipython debugger are jump (j) and quit (q).  With jump (enter a `j` instead of a `n` at the `ipdb>` prompt) allows you to go to a specific line of code to execute next.  For instance entering `ipdb>j 15` allows you to jump down to the business end of the `if __name__ == "__main__":` section and move on from there.

There is lots more to learn about debugging but this should get things started.  Have fun and keep learning!
