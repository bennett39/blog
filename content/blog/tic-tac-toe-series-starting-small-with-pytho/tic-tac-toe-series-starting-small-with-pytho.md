---
title: Tic-tac-toe series - Starting small with Python
date: 2019-04-24T15:09:02.814Z
description: I recently wrote a post about the importance of starting small when you’re learning a new language or framework.
---



](/tic-tac-toe-series-starting-small-with-python-86e2f49db797?source=post_page-----86e2f49db797--------------------------------)

I recently wrote a post about the [importance of starting small](https://medium.com/@BennettGarner/learning-python-start-small-29d15881f780) when you’re learning a new language or framework.

In that post, I suggested a bunch of tiny, throwaway experiments you could do to stop using tutorials and start coding independently in a new language.

The key insight from that post, however, was incrementalism. You learn by building something easy and then gradually making things just one level more complex. Stretch the limits of your understanding, but in manageable ways.

# Simple apps teach big lessons

In this [series of posts](https://medium.com/@BennettGarner/the-tic-tac-toe-series-master-list-a4a908f015f9), I want to illustrate what I mean by incrementalism.

We’ll start by building something trivial, and then we’ll continuously make it more complex.

Who knows where we’ll end up by the end of this series? But each post in the series should be able to stand alone and teach some good lessons.

The idea is for me to stretch the limits of my Python and software development understanding as well. As such, don’t treat these articles like a tutorial. Instead, think of it as a collaborative effort. Feel free to follow along, build your own version of the app, and suggest improvements to my app!

# Ok, so what are we building?

I think a simple game of tic-tac-toe is a good starting point.

The game itself should be pretty easy to implement. But it will also offer all kinds of opportunities for future development and complexity without getting too massive.

I’ve got all kinds of ideas for the eventual game — including creating an AI that never loses at tic-tac-toe for players to play against. Sounds fun right?

Along the way, we’ll use git for version control, test the application using pytest, maybe port it over to be playable online? There are tons of options!

But for now, we need to build the base of the game. So, let’s just build a simple version of tic-tac-toe for the command line.

# Start super small — tiny increments!

The point of this app is to prove the value of incrementalism. So, don’t get overwhelmed if you don’t know anything about testing, version control, or deploying python apps online.

We’ll get to all that eventually, but first we’re going to start with a trivial mini-app that doesn’t even do anything.

Let’s just render the tic-tac-toe board, first. That’s it!

# Mini-step #1: Define the board

Okay, so let’s think about a tic-tac-toe board:

![](https://miro.medium.com/max/600/1*9LBDOx6gG4mTuPGPDp888w.png?q=20)



It’s a 3x3 board. Each spot on the board is in one of three states: blank, X, or O.

So, let’s just create a static board for now so that we can practice rendering it in Python.

**WAIT! Before you look at my idea for how we should define the board, think of your own!**

This isn’t a tutorial. Don’t just copy my code or skim through it. Think about the problem for yourself before you look at what I came up with!

…

…

…

Okay, here’s my idea of how to define the board.

board = \[  
    \["\_", "\_", "O"\],  
    \["X", "O", "\_"\],  
    \["O", "\_", "X"\]  
\]

I created the board as a list of lists, that way we can keep track of the rows easily. But you could do it as a single list, too.

board = \[None, None, "O", "X", "O", None, "O", None, "X"\]

Both work. There are tradeoffs in readability vs. time/space performance. But since this app is so tiny and for learning purposes, I decided to err on the side of readability.

I also decided to use `"_"` for my blank squares, but you could imagine using another value, like `None` or anything really. The reason why I chose `"_"`is because it will display nicely on the console. Additionally, it will be easy to check equality with `"_"` later, rather than dealing with the confusion of checking equality with `None`.

We’re going to use the first, list-of-lists version.

That’s it! We’ve thought about and defined our board! Yours might be different from mine, and that’s okay. That’s actually the whole point of this series.

# **Mini-step #2: Print the board**

Okay, we have the board defined. Now, let’s print it.

First, let’s see what it looks like if we just print the board.

\# ttt.pyboard = \[                                                                                                                                                               
    \["\_", "\_", "O"\],                                                                                                                                                    
    \["X", "O", "\_"\],                                                                                                                                                    
    \["O", "\_", "X"\]                                                                                                                                                     
\]                                                                                                                                                                       
                                                                                                                                                                        
print(board)

Run it:

$ python ttt.py  
\[\['\_', '\_', 'O'\], \['X', 'O', '\_'\], \['O', '\_', 'X'\]\]

Hmm, that prints the board, but it’s not intuitive. Think we can stack the rows on top of each other?

**Mini-step: try to print each row to the console on a different line. Don’t look at my solution until you have one of your own!**

…

…

…

\# ttt.py                                                                                                                                                                
                                                                                                                                                                       
board = \[                                                                                                                                                               
    \["\_", "\_", "O"\],                                                                                                                                                    
    \["X", "O", "\_"\],                                                                                                                                                    
    \["O", "\_", "X"\]                                                                                                                                                     
\]                                                                                                                                                                       
                                                                                                                                                                       
for row in board:                                                                                                                                                       
    print(row)

Run it:

$ python ttt.py  
\['\_', '\_', 'O'\]  
\['X', 'O', '\_'\]  
\['O', '\_', 'X'\]

That looks good!

# Mini-step #3: Make it reusable

One key element of software development is making code reusable.

We’ll probably need to print our board multiple times in the final application. We shouldn’t repeat that same `for` loop everywhere we want to print the board.

Additionally, what if we want to change how we print the board? If we repeat the `for` loop in multiple places, then we’ll need to make changes in multiple places.

It’s much better if we define a `print_board` function. That’s pretty easy to do.

**Try it!**

…

…

…

\# ttt.py                                                                                                                                                                
                                                                                                                                                                       
board = \[                                                                                                                                                               
    \["\_", "\_", "O"\],                                                                                                                               
    \["X", "O", "\_"\],                                                                                                                                           
    \["O", "\_", "X"\]  
\] def print\_board(board):                                                                                                                                                                                                                                                                                                                                       
    for row in board:                                                                                                                                                       
        print(row)print\_board(board)

# Mini-step #4: List comprehension to create board

Great, we’re well on our way to playing tic-tac-toe.

Now, let’s get rid of our sample board and initialize the board in its empty state at the beginning of a game.

We could easily do something like this:

board = \[                                                                                                                                                               
    \["\_", "\_", "\_"\],                                                                                                                               
    \["\_", "\_", "\_"\],                                                                                                                                           
    \["\_", "\_", "\_"\]  
\]

And that would work perfectly well.

But we’re trying to incrementally get better at Python. This board creation is crying out for a [list comprehension](https://www.pythonforbeginners.com/basics/list-comprehensions-in-python)! We can do this in a single line of code!

**Try it!**

…

…

…

board = \[\["\_" for \_ in range(3)\] for \_ in range(3)\]

Notice how I used the `_` . If you’ve never seen that before, it’s a common way in Python to create a loop when we don’t actually need access to the iterator itself.

It’s a [throwaway value](https://stackoverflow.com/questions/5893163/what-is-the-purpose-of-the-single-underscore-variable-in-python), among other things. Actually, reading that stackoverflow answer taught me about other formally defined uses of underscore in Python that I didn’t really know! Learning stuff incrementally!

## Bonus points:

You might be thinking,

> “Hey, wait a minute! If we can do a list comprehension to create the board, can’t we also print the board in a single line?”

And you’d be right to think that! We can technically print the board in a single line using list comprehension:

def print\_board(board):  
    \[print(row) for row in board\]

But I was curious, is this actually a good idea? It seems like the list comprehension might add overhead to the print statements, creating an unnecessary list.

So — always be incrementally learning — I tested it!

\# ttt.pyimport timeitboard = \[\["\_" for \_ in range(3)\] for \_ in range(3)\]def print\_board\_list(board):  
    \[print(row) for row in board\]def print\_board\_for(board):  
    for row in board:  
        print(row)begin = timeit.default\_timer()  
print\_board\_list(board)  
end = timeit.default\_timer()  
print(f"Using list comprehension: {end - begin}")begin = timeit.default\_timer()  
print\_board\_for(board)  
end = timeit.default\_timer()  
print(f"Using a for loop: {end - begin}")

And it turns out that using list comprehension in this case is about twice as slow, on average:

$ python ttt.py   
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
Using list comprehension: 0.00018204300431534648  
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
Using a for loop: 9.22030012588948e-05

_(Notice the scientific notation_ `_e-05_` _at the end, if you’re confused.)_

So, this is a great example of where writing one-liners is an overreach. It actually slows down our program.

Now, to be fair, we’re talking a few hundred-thousandths of a second in a tic-tac-toe game. If you’re learning list comprehension, this might be fun to implement and the speed difference is harmless.

However, I had fun testing and timing both approaches! I learned something incrementally and pushed the limits of my understanding.

Check out the [speed test code on GitHub](https://github.com/bennett39/ttt-medium/tree/listspeed)

# Wrapping up for today

Maybe it feels like we didn’t do much today. If this were a tutorial, you’d be right to be disappointed. All we did was create a list of lists and then print it.

But I feel like we learned a lot!

I’ve got a fair amount of Python development experience, and I even learned stuff from these simple exercises.

That’s the whole point! Build something you know, then push it to the next level.

You can find [today’s code on GitHub](https://github.com/bennett39/ttt-medium/tree/day1).

In the [next post in the series](https://medium.com/@BennettGarner/tic-tac-toe-series-getting-validating-user-input-in-python-feaef58cc54), we’ll start experimenting with accepting user input, catching errors, and updating the board.

[

## Tic-tac-toe series: Getting & validating user input in Python

### Yesterday, I started a series about learning Python and software development incrementally using tic-tac-toe as an…

medium.com



](https://medium.com/future-vision/tic-tac-toe-series-getting-validating-user-input-in-python-feaef58cc54)

Check out the complete list of [all posts in this tic-tac-toe series](https://medium.com/@BennettGarner/the-tic-tac-toe-series-master-list-a4a908f015f9).

[

## The Tic-tac-toe Series: Master List

### Here are all the posts in the tic-tac-toe series so far:

bennettgarner.medium.com



](/the-tic-tac-toe-series-master-list-a4a908f015f9)

## Free Resources

Want my best content on web development and becoming a better programmer?

[Join my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).