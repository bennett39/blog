---
title: NumPy Tutorial - Learn the Basics in Minutes!
date: 2020-04-27T13:46:21.765Z
description: Do fast things with lots of data, but make sure you know when and why (and when not) to use it!
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5f896074e13f&operation=register&redirect=https%3A%2F%2Flevelup.gitconnected.com%2Fgetting-started-with-numpy-learn-the-basics-in-minutes-5f896074e13f&source=post_actions_header--------------------------bookmark_preview-----------)

![](https://miro.medium.com/max/600/1*0rwFN3MMpWrnqdBBvOMJvg.png?q=20)



Wanna do something in Python with lots of data? You should learn NumPy.

NumPy is an awesome tool that you should be familiar with if you’re a Python developer, data scientist, or just somebody interested in doing things with data.

But maybe you haven’t tried to use it or have been intimidated by its apparent learning curve and differences from standard Python data structures.

In this post, we’ll take a look at NumPy, what you can use it for, when you definitely should use it, when not to use it, and why it’s so popular.

# NumPy Advantage #1: Speed

![](https://miro.medium.com/max/600/1*6EswSN4MTsOSAZJlLepcJQ.jpeg?q=20)



Speed, baby

When it comes to doing heavy data processing and calculations with Python, you’ll often hear about how Python is natively pretty slow.

However, despite its [relative slowness](https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/python.html), Python has become the go-to language for data science. Applications that process millions or billions of data points use Python.

That’s odd. Why would data scientists and engineers choose a language that’s even a fraction slower? Over the course of billions of calculations, surely that adds up?

Enter Python’s C API. While Python may be one of the slower languages, the most common distribution of Python is built atop one of the fastest languages, C. When we need blazing fast calculations, Python has an interface for running C code. These C extensions make it easy to offer C-level performance inside a Python application.

NumPy is one such C extension. While CPython itself is written in C, it has to be good for general purpose use and a lot of various data types and edge cases. By contrast, NumPy implements its own custom data types in C that are optimized for fast calculations and modifications. NumPy can make calculations happen much faster, sometimes even 10x faster.

Here’s an example (thanks to [tom10](https://stackoverflow.com/a/994545) for this code snippet, comments are mine):

from numpy import arange  
from timeit import TimerNelements = 10000  
Ntimeits = 10000x = arange(Nelements) # <-- A NumPy range  
y = range(Nelements) # <-- A standard Python range\# The numpy .sum() method is optimized for calculations  
t\_numpy = Timer("x.sum()", "from \_\_main\_\_ import x")\# The CPython sum() function is more generalized  
t\_list = Timer("sum(y)", "from \_\_main\_\_ import y")print("numpy: %.3e" % (t\_numpy.timeit(Ntimeits)/Ntimeits,))  
print("list:  %.3e" % (t\_list.timeit(Ntimeits)/Ntimeits,))

On my computer, that output:

python time-sum.py   
numpy: 2.650e-05  
list:  5.758e-04

In this case, NumPy is 20x faster at summing values in a range. That’s significant, and it starts to give you an insight into why NumPy is so valuable and widely used.

# NumPy Advantage #2: Memory

![](https://miro.medium.com/max/600/1*9mDNpcAmftJpsafZ1TC9Tg.jpeg?q=20)



Less RAM for more numbers

NumPy is more efficient with memory and faster at reading/writing. This is because NumPy places restrictions on how you can build your arrays.

In standard Python, a list can include any values:

\# This is a valid list  
my\_list = \['a', 2, len, {'z': 1}\]

In that list, we have a string, integer, function, and dictionary. But Python doesn’t actually store those 4 things sequentially in memory. Instead, a Python list is just a series of pointers to the objects themselves, wherever they exist.

So, the string `'a'` lives somewhere else. The integer `2` is a singleton that Python creates at startup (as are all integers between -1 and 255). The function `len` is defined in the standard library. And the dictionary is actually a separate complex object, also composed of pointers to its sub-objects.

All those pointers add to the complexity and size of Python lists. Every value in a Python list is at least 20 bytes. 4 bytes per pointer, plus 16 bytes for even the smallest Python object (4 for type pointer, 4 for reference count, 4 for value — and the memory allocators rounds up to 16). \[Thanks to [Alex Martelli](https://stackoverflow.com/a/994010) for that math.\]

We can test out that list objects are passed by reference for ourselves:

\>>> my\_list = \['a', 2, len, {'z': 1}\]  
\>>> my\_list\[2\](my\_list)  
4  
\>>> len(my\_list) # Equivalent to the line above  
4

By contrast, NumPy arrays are smart enough to figure out if you’ve passed in a list of uniform values. If you did, NumPy will optimize storage and operations on those values.

\>>> a = np.array(\[2,3,4\])  
\>>> a  
array(\[2, 3, 4\])  
\>>> a.dtype  
dtype('int64') # Stored as a 64-bit integer, instead of Python obj\# We can still create a np array from my\_list  
\>>> b = np.array(my\_list)  
\# But everything is stored in the standard Python way as objects  
\>>> b  
array(\['a', 2, <built-in function len>, {'z': 1}\], dtype=object)

When you give NumPy standardized inputs, the memory optimizations can be substantial. For instance, Python would take 12GB of memory to handle a billion `float`s. NumPy on the other hand, could do so with about 4GB. \[Hat tip again to [Alex Martelli](https://stackoverflow.com/a/994010)\]

# NumPy Advantage #3: Convenience

![](https://miro.medium.com/max/600/1*IBRIX3IhRuQr1bP2bnnffQ.jpeg?q=20)



Kick back, relax, and let NumPy do the work

If you’re doing stuff with large datasets, there are some common operations you’ll need to undertake.

Vectors, matrix manipulation, multidimensional arrays, and regressions are all common.

In standard Python, these can be non-trivial to implement. In NumPy they come out-of-the-box, with optimized and tested implementations.

Moreover, you can read directly from files into a NumPy array and do other I/O tasks more efficiently for large datasets.

Additionally, most other data packages, including statistical and machine learning packages support NumPy data structures, making NumPy a good foundation for any data intensive project, even if memory and speed aren’t concerns.

# Try It Out!

Hopefully, you’ve now seen how useful NumPy is and made a decision about whether it makes sense for your application.

In any case, it’s a powerful technology that’s worth learning how to use if you’re someone who comes in contact with data often. An understanding of NumPy now could pay off later when you encounter a task that could use it.

## 1\. Installation

NumPy works best when used as part of an established scientific computing Python distribution. That way you can get the most out of it.

By far, the most popular scientific Python distribution is Anaconda. There’s also a smaller version called Miniconda that will give you access to most of what you need.

Create a new directory and install Anaconda/Miniconda in a virtual environment:

\# Install the distribution  
pyenv install miniconda3-latest\# Create a virtual environment using that distribution  
pyenv virtualenv miniconda3-latest <your-env-name>\# Set that virtual environment as the default for this directory  
pyenv local <your-env-name>

Now that we have it installed, we need to activate Anaconda. Specifically, Anaconda uses its own package manager (like `pip` that you might be familiar with). This package manager is called `conda` .

You can use the `conda` package manager with many types of shells:

*   bash
*   fish
*   tcsh
*   xonsh
*   zsh
*   powershell

To activate it:

conda init <your shell name>  
eval $SHELL # Restart your shell so changes take effect  
conda activate <your-env-name>

Now, we can install NumPy.

conda install numpy

Check it out!

$ python  
Python 3.8.0 (default, Nov  1 2019, 18:25:29)   
\[GCC 7.3.0\] :: Anaconda, Inc. on linux  
Type "help", "copyright", "credits" or "license" for more information.  
\>>> import numpy  
\>>> numpy.\_\_version\_\_  
'1.11.3'

## 2\. Try Some Tutorials

This post is already long, so I won’t make it into a beginner NumPy tutorial. Plus, there are a lot of great resources already out there. Here are my top picks, assuming you already have some Python experience:

1.  [The Official Quickstart Tutorial](https://numpy.org/devdocs/user/quickstart.html) for the basics
2.  [SciPy Lecture Notes](https://scipy-lectures.org/) for an introduction to the scientific computing ecosystem
3.  [From Python to NumPy](https://github.com/rougier/from-python-to-numpy) for learning how to practically convert Python functions into much faster NumPy equivalents

## 3\. Build something with NumPy

Test and expand your new knowledge with some exercises:

*   [100 NumPy Exercises](https://github.com/rougier/numpy-100/blob/master/100_Numpy_exercises.md)

Then try some simple projects:

*   [Sudoku solver](https://medium.com/datadriveninvestor/solving-sudoku-in-seconds-or-less-with-python-1c21f10117d6) using NumPy arrays
*   Average delays for your [city’s subway system](https://opendata.cityofnewyork.us/)
*   Calculate returns of [various portfolio allocations](https://finance.yahoo.com/quote/%5EGSPC/history?p=%5EGSPC) over 1, 5, 10, 20 years

# How & Why to Use NumPy

NumPy isn’t a perfect solution for every use case. Most notably, using NumPy can make your code more difficult to read, so be sure to comment heavily.

Nor are NumPy’s data structures a complete replacement for Python’s lists. Instead, you should treat NumPy as an additional tool that makes working with uniform datasets much easier.

As a general rule of thumb, whenever you have lots of data in the same format, you’re probably good to use NumPy, especially if there will be millions or billions of data points.

Chances are, if you have a data intensive application, there’s a way NumPy can help you optimize operations.

# About Bennett

I’m a software developer in New York City. I do web stuff in Python and JavaScript.

Like what you’ve read here? I have an [email list](https://mailchi.mp/2e671faffc04/bennett-medium) you can subscribe to. Infrequent emails, only valuable content, no time wasters. I’d love to have you there.

*   **📧** [**Get an email when I publish something new**](https://mailchi.mp/2e671faffc04/bennett-medium) **📧**