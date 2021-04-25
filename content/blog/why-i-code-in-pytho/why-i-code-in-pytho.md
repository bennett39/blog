---
title: Why I Code in Python
date: 2018-12-12T19:59:54.734Z
description: I’m by no means an expert developer, but I’ve learned other languages and Python suits me best. Here’s why.
---



](/why-i-code-in-python-a1e4012eb859?source=post_page-----a1e4012eb859--------------------------------)

![](https://miro.medium.com/max/600/0/1*4ZZtp1Zcw3RlUBZGFwxrpA.png)

I’m by no means an expert developer, but I’ve learned other languages and Python suits me best. Here’s why.

It’s often said there’s no such thing as the “best programming language,” only languages that are well-suited to particular use cases and contexts. I agree.

If you want to build enterprise-level, secure software, Java is probably your best bet. Doing something interactive in the browser? JavaScript is just about your only option. Making a high-frequency trading algorithm where microseconds matter? C or even Assembly are your best bets.

However, there are a whole host of programming problems that fall somewhere in the middle. They don’t require a specific tool, they just need a good, usable, readable, and extendable language to get the job done.

Python is king among programming languages for its low learning curve, clean syntax, and powerful tools. This makes it a great choice for quickly developing high-level applications that are straightforward to maintain.

# Open-source tools make Python more powerful

Perhaps more than any other language, Python has established itself as a solid tool to do almost anything. Where Python really shines, however, is in its various packages that extend the language into hundreds of use cases.

*   Building a web app? [Django](https://www.djangoproject.com/) or [Flask](http://flask.pocoo.org/) can get you started quickly.
*   Doing stuff with data? [NumPy](http://www.numpy.org/) and [Pandas](https://pandas.pydata.org/) are the go-to tools of data wizards.
*   Building a video game? Civ IV, Battlefield 2, The Sims 4, and Overwatch all use Python for tasks.
*   Science research you say? [SciPy](https://www.scipy.org/) is king here, with specialized packages existing for specific domains like [Astropy](http://www.astropy.org/) and [Biopython](https://biopython.org/)
*   Getting into machine learning/AI? Python has become the go-to language for this stuff, with [TensorFlow](https://www.tensorflow.org/), [PyTorch](https://pytorch.org/), and many others.

Adding these tools is often as simple as a `sudo apt-get` or `pip install` on the command line and immediately you’ve extended your programming capabilities into new realms with powerful, well-supported tools.

# Dynamic typing and automatic memory management

Compared to a compiled language like C, Python requires you to think less when you write code. You don’t need to declare variable types or watch your memory for leaks.

Old-school developers will see this as a drawback, not a benefit, of Python. They argue it abstracts away an important understanding of how code actually gets implemented.

I agree. If you learn only Python, you’ll have a limited understanding of how programming actually works. Ideally, every developer should have experience in compiled, statically typed languages as well.

Even better, if you’re up for a challenge, figure out how to build your own compiler and learn how machine code works.

However, once you have the basic understanding of what’s happening under the hood, the dynamic typing and memory management of Python is incredibly liberating.

Gone is all the boilerplate and overhead of thinking about locations in memory and allocating enough space.

Something like this in C:

// If characters in keyphrase are not alphabetical, give error.    for (int i = 0, n = strlen(argv\[1\]); i < n; i++) {  
    if (isalpha(argv\[1\]\[i\]) == false) {  
        printf("Alphabetic characters only, please.\\n");  
        return 1;  
    }      
}

Becomes this in Python:

if sys.argv\[1\].isalpha() == False:  
    print("Alphabetic characters only please.")  
    sys.exit(1)

We avoided the curly braces and parentheses. More importantly, Python has built-in string handling, so we didn’t need a for loop to look through all the characters in `argv[1]`. Also, gone are the days of looking for missed semicolons at compile time.

This may seem trivial, but it’s not. The time difference between writing these two simple functions adds up. Even if it’s a few seconds and a few drips of mental energy, writing code in C is simply slower and more taxing than writing in Python.

Unless there’s a really good reason to write your code in a more complicated language, writing in a simple language like Python should be the default.

# These are all arguments for Python I’ve heard before!

Fair enough. I promised my take on why I code in Python.

To be clear, I don’t \*only\* code in Python. For instance, I’ve recently been implementing [Project Euler in C](https://medium.com/@BennettGarner/consider-yourself-a-developer-you-should-solve-the-project-euler-problems-ed8d13397c9c). It’s important as a developer not to become a one-trick pony. Learn other stuff and expand the way you think about code.

However, Python is the language I’ve chosen to get good at for a few reasons:

1.  It’s extensible, and built to be extensible. People can add on to Python in myriad ways, and it’s unlikely to get pigeonholed into a single use.
2.  Python packages are powerful. If you ever get bored of Python, just add a new package and try to learn it. I promise you’ll never run out of new tools to explore. Best of all, these new tools are still Python and look like Python, unlike for instance JavaScript frameworks that completely change the syntax of a language.
3.  The latest trends in tech are choosing Python. Data science and machine learning have firmly chosen Python. Many top startups like Dropbox and Instagram use Django as their web backend of choice.
4.  It’s pretty to look at. Python code is just visually appealing, and when you’re spending hours a day looking at, reading, and deciphering code, you want it to be as nice-looking as possible.

# Python isn’t perfect

Of course, there are valid critiques of Python. No programming language is perfect.

*   Speed is an issue if you need high performance. The CPython implementation can’t possibly compete with code written in C/C++ or even Java in terms of computation speed.
*   Relatedly, the Global Interpreter Lock makes multithreading less efficient in Python. Running native Python code across multiple CPUs is problematic, but using extensions like NumPy you can often get around the GIL.
*   Dynamic typing, while easier on the initial writing of the code, can introduce bugs or downright security vulnerabilities in the long run. Statically typed languages are generally considered more secure.
*   There are a million other things that developers like about the first language they learned that Python doesn’t do the same or at all. For example: closures, switch statements, break/continue, tail call optimization, etc.

# I still ❤ u Python

Python is a modern, useful, easy-to-learn language. Give it a try if you haven’t. There are so many things you can build with Python, you’re sure to find something.

_Did I miss anything important about Python? Comment below and I’ll add the best points into the article!_