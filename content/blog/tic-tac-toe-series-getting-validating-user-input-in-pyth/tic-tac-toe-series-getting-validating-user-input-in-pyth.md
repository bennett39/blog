---
title: Tic-tac-toe series - Getting & validating user input in Python
date: 2019-04-25T16:24:45.552Z
description: Yesterday, I started a series about learning Python and software development incrementally using tic-tac-toe as an example.
---



I [started a series](/@BennettGarner/tic-tac-toe-series-starting-small-with-python-86e2f49db797) about learning Python and software development incrementally using tic-tac-toe as an example.

This whole series is based on the idea that you learn new things very gradually. It takes tons of tiny experiments to get good at coding. The best thing you can do to learn is [start small](/@BennettGarner/learning-python-start-small-29d15881f780) and then push the edge a little at a time, every day.

I intend to use this series as an example.

# Not a tutorial; a learning experience

In that first post, I stressed that this is not a tutorial! I’m making this up as I go along. Although I’ve been writing Python code for while, I’ll likely make mistakes, code myself into a corner, and need to refactor as we go along in this process.

You can follow along by building your own version of the tic-tac-toe game, making your own choices. Feel free to suggest improvements to my version!

I’m learning from this project, too! And I want it to feel like a collaborative community.

# The future of tic-tac-toe

Eventually, we’ll add an AI that can play tic-tac-toe against you, a full test suite for our application, version control via git, deploy the game to the web, add CI/CD for our web app, set up user auth so users can log in to save their scores, saving scores will require a database so we’ll do that, maybe an all-time leader board, perhaps multi-player online tic-tac-toe where players can challenge each other.

The possibilities are widespread and exciting! This will be really fun.

But first, we’re following the most important principle: start small, with something trivial, and add complexity a tiny bit at a time.

If you’re learning how to swim, don’t drop yourself in the middle of a lake. That’s too big of a task at the beginning!

# Where we left off

So, in the [last post](/@BennettGarner/tic-tac-toe-series-starting-small-with-python-86e2f49db797), we started super simple — just creating and displaying the board to our users on the command line.

We learned a lot, though! We worked with list comprehensions, ran a speed test on different variations, and made some data structure decisions.

Need a refresher on yesterday? [Here’s the code](https://github.com/bennett39/ttt-medium/blob/day1/ttt.py).

# Goal for today

In today’s post, we’ll start accepting user input and updating our board.

Here are some specifications:

*   User should see a prompt
*   User can select a square on the board using numbers 1–9
*   Any invalid selection displays an error

Things we won’t worry about today, but need to think about tomorrow:

*   Selecting a number places a symbol in that square
*   The symbol placed alternates between “X” and “O”
*   Once a square has a symbol, it can’t be overwritten

We won’t worry about finding out when someone has won or when the board is full — we’ll leave that to later.

Okay, so let’s work through each specification incrementally, taking tiny steps to avoid getting overwhelmed.

# Mini-step #1: Prompt the user

The goal here is to prompt the user to select a square in order to make a move in the game.

To do so, we’ll want the user to select one of the nine available squares on the board:

![](https://miro.medium.com/max/600/1*QFXmQSXzcDZjd5aatJ6mMw.png?q=20)



Cool, so we just need to get a number from the user! Not too hard.

Python makes it really easy to get user input. Let’s quickly implement an input statement and save the user’s response as a variable.

If you’re just learning Python, **try implementing it yourself before you look at my solution**! Read the docs on [how Python inputs work](https://docs.python.org/3/library/functions.html#input).

…

…

…

selection = input("Select a square: ")

Let’s see how it works!

\>>> selection = input("Select a square: ")  
Select a square: 1  
\>>> print(selection)  
1

It looks like it’s working! But is it doing what we want it to?

We need to dig a little deeper in our next mini-step.

# Mini-step #2: Type conversion

Let’s go back to the code we just ran:

\>>> selection = input("Select a square: ")  
Select a square: 1  
\>>> print(selection)  
1

Just looking at it, `selection` certainly looks like a number, but is it?

If it’s a number, we should be able to add to it:

\>>> selection + 1  
Traceback (most recent call last):  
  File "<stdin>", line 1, in <module>  
TypeError: must be str, not int

Ouch, TypeError. And weird debug message “must be str, not int” — what does that mean?

Let’s try another mini-experiment (that’s how you learn new things after all!):

\>>> selection == 1  
False

How can that be? We just saw that selection was “1”, how come it doesn’t `== 1` ?

Well, the traceback above told us we have a TypeError, so let’s check on the type of `selection`:

\>>> type(selection)  
<class 'str'>

Hmmm, so `selection` is a string, not a number! That’s our problem. Let’s do the equality check again:

\>>> selection == 1  
False  
\>>> selection == '1'  
True

For those of you who clicked the link to the `[input](https://docs.python.org/3/library/functions.html#input)` [documentation](https://docs.python.org/3/library/functions.html#input) that I shared above, you’re not surprised. `input` always returns a string!

If you want to convert a string into an integer, you’ll have to do some [type conversion](https://www.programiz.com/python-programming/type-conversion-and-casting).

**Try it yourself! See if you can write code so that** `**selection == 1**` **returns True.**

…

…

…

\>>> selection = int(input("Select a square: "))  
Select a square: 1  
\>>> selection == 1  
True  
\>>> type(selection)  
<class 'int'>

# Mini-step #3: Validate 1–9

Why did we need to do the type conversion in the first place?

Well, there are only nine spots on the board. So, if a user inputs `-1` or `10` , we should tell them that’s an invalid entry.

To do so, we need to be able to check that the value falls within our given range. And that means we need `selection` to be a number, so we can use comparison operators (e.g. `<`, `>`, `≤`, etc).

So, how should we check that the user’s input falls between 1–9, inclusive?

**Try it yourself!**

…

…

…

My idea is a simple if statement:

selection = int(input("Select a square: "))  
if selection > 9 or selection < 1:  
    print("Sorry, please select a number 1-9.")  
else:  
    print(selection)

Try it:

$ python ttt.py   
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
Select a square: 1  
1$ python ttt.py   
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
Select a square: 10  
Sorry, please select a number 1-9.$ python ttt.py   
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
Select a square: -1  
Sorry, please select a number 1-9.

Great, it’s working! We’ll decide what to do with those invalid selections later — maybe we can re-prompt the user somehow instead of having the program shut down?

# Mini-step #4: Exceptional problems

Before we decide what to do with the invalid number selections, we have a bigger issue to deal with.

What happens if the user inputs something that’s not a number at all?

$ python ttt.py   
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
Select a square: aTraceback (most recent call last):  
  File "ttt.py", line 11, in <module>  
    selection = int(input("Select a square: "))  
ValueError: invalid literal for int() with base 10: 'a'

Uh oh, ValueError and our application crashes!

Since we’re converting our user’s input into an integer, Python doesn’t know what to do when that conversion fails. There’s no such thing as `int('a')`.

This is the perfect opportunity to practice our error and exception handling skills in Python!

If you’re new to Python error handling, check out the official documentation on [errors and exceptions](https://docs.python.org/3/tutorial/errors.html). There’s literally a perfect example in there that applies directly to our problem.

**Try to do your own error handling before checking out my solution!**

…

…

…

try:  
    selection = int(input("Select a square: "))  
    if selection > 9 or selection < 1:  
        print("Sorry, please select a number 1-9.")  
    else:  
        print(selection)  
except ValueError:  
    print("Sorry, that's not a number!")

We take the exact same code as before, but we place it under a `try` block, so that we can handle the errors that get raised without them crashing the program.

# Bonus points — refactoring

In the code above, we print error messages in two different places. That’s a problem when we could do it better with one well-written error message.

Also, we have a complicated control flow with try, if, else, except all packed in together. That might get confusing later. Can we make it simpler?

Well, if you think about it, any value that isn’t 1–9 is a problem. We could say that anything that isn’t 1–9 should be a ValueError, including other integers!

Lucky for us, Python allows us to easily raise new Errors using the `[raise](https://docs.python.org/3/tutorial/errors.html#raising-exceptions)` [statement](https://docs.python.org/3/tutorial/errors.html#raising-exceptions).

**Try to raise your own error and any other refactoring you can think of before you look at my solution.**

…

…

…

The code cleans up to this:

try:  
    selection = int(input("Select a square: "))  
    if not 1 <= selection <= 9:  
        raise ValueError  
    print(selection)  
except ValueError:  
    print("Sorry, please select a number 1-9")

Notice how I also changed around the comparison operator. Chained comparisons are faster, are considered more Pythonic, and I also think they’re easier to read.

# Bonus points #2–More refactoring

Here’s another thing to think about: we’re probably going to need to ask for user input a lot.

Any time there’s something we do repeatedly, that’s a good candidate for factoring out into its own function.

**So, try to create a function** `**select_square()**` **that gets a user’s input, returns the input if valid, and raises an error if invalid.**

…

…

…

Here’s the new refactored application:

\# ttt.py  
  
def print\_board(board):  
    for row in board:  
        print(row)  
  
  
def select\_square():  
    selection = int(input("Select a square: "))  
    if not 1 <= selection <= 9:  
        raise ValueError  
    return selection  
  
  
board = \[\["\_" for \_ in range(3)\] for \_ in range(3)\]  
print\_board(board)  
try:  
    selection = select\_square()  
except ValueError:  
    print("Sorry, please select a number 1-9")  
    # TODO - Find a way to re-prompt the user here  
\# TODO - Use selection to update the board (tomorrow's task)

It’s best practice in software development that each function should do only one thing. By refactoring out `select_square()` we may have made our application a few lines longer, but now it’s more readable and extensible in the future.

When I was thinking about the `select_square()` function, I was considering having it handle the try/except error inside the function. Then, if there was an error, it could return `None` or something.

While that’s also a perfectly good solution, raising an error might actually make the function more usable for other developers. They won’t need to know that the function returns `None` or `0` or whatever when the input is invalid. Instead, the exception speaks for itself. I credit [this Stack Overflow answer](https://stackoverflow.com/a/51524955/10558777) with making that point clear to me.

I learned something incrementally today!

# Wrapping up

That’s enough for today. Hopefully, you’ve learned something new from incrementally exploring today’s problems.

Today, I learned:

*   Chained comparisons are faster because the middle value only needs to be evaluated once. In terms of style, they’re more Pythonic. They’re also not available in all languages. Python is special!
*   When you get a `TypeError` during addition, the error message assumes that the first data type you provided is what you want to match. Thus, these equivalent statements produce different error messages:

\>>> 'abc' + 1  
Traceback (most recent call last):  
  File "<stdin>", line 1, in <module>  
TypeError: must be str, not int\>>> 1 + 'abc'  
Traceback (most recent call last):  
  File "<stdin>", line 1, in <module>  
TypeError: unsupported operand type(s) for +: 'int' and 'str'

That’s why the TypeError message was strange when we tried `selection + 1`, earlier.

*   It’s personal preference, but I think I like raising an error rather than returning `None` from a function — N.B. that you can either explicitly code `return None` or you can leave it out and Python will implicitly return None when it reaches the end of the function

See, even something as simple as accepting and validating inputs can be a great place to learn small, new things about Python!

Here’s the [code from today](https://github.com/bennett39/ttt-medium/tree/day2).

Check back in the next post in the series to see how we’ll place “X”s and “O”s on the board and prompt the user repeatedly!

## About Bennett

I’m a software developer in New York City. I do web stuff in Python and JavaScript.

Want to be updated when I release a new post? I have an [email list](http://eepurl.com/goJwcT) you can subscribe to. I have nothing to sell, only free content to share with the community, no time wasters. I’d love to have you there.

Check out the complete list of [all posts in this tic-tac-toe series](/@BennettGarner/the-tic-tac-toe-series-master-list-a4a908f015f9).