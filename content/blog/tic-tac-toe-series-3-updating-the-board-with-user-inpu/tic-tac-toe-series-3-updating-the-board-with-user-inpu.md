---
title: Tic-tac-toe series #3 - Updating the board with user input
date: 2019-04-26T15:34:25.830Z
description: The past few days, I’ve started a series of posts about learning software development techniques incrementally in Python.
---



](/tic-tac-toe-series-3-updating-the-board-with-user-input-9aa4fd435c44?source=post_page-----9aa4fd435c44--------------------------------)

![](https://miro.medium.com/max/600/8/1*jWJ6I9bqojDQL3pXTpvLhA.png)

Where we’re headed today

The past few days, I’ve started a [series of posts](https://medium.com/@BennettGarner/the-tic-tac-toe-series-master-list-a4a908f015f9) about learning software development techniques incrementally in Python.

The series has focused on building a command line tic-tac-toe game. So far, we have a board and a way to get user input. In the future, we’ll add all kinds of features to our game.

Who knows how long this series will last — months at least! Come along for the ride!

# Learning incrementally

This whole series is based on the idea that the best way to learn is in small increments.

To really get good at coding, you’ll need to stop relying on tutorials and cut-paste. The best way to practice your independent coding skills is with tiny experiments.

I wrote a whole post explaining my [philosophy of starting small](https://medium.com/@BennettGarner/learning-python-start-small-29d15881f780).

As such, this series isn’t a tutorial. Instead, treat it as a collaborative exercise, building and learning together. I’ve been coding Python for a while now, and I still learn new stuff working on this simple project.

# Where we left off

We started with rendering the tic-tac-toe board for the user.

Next, we created a function that accepts user input and handles errors from that input.

![](https://miro.medium.com/max/600/1*lqM3WOGIPW-s5iqaKSQtVg.png?q=20)



Yesterday’s program

Today, we’ll update the board using the validated user input we just collected.

As a refresher, here’s the [code up to this point](https://github.com/bennett39/ttt-medium/tree/day2).

# Today’s goal

There are three main tasks I want to accomplish today:

1.  Once we get a user input, add an “X” or “O” to the board in the correct position
2.  Re-prompt the user for the next move, filling the board
3.  Alternate between “X” and “O” on each move

Essentially, by the end of today, you should be able to play rudimentary tic-tac-toe!

As always, we’ll work in mini-steps to break down the tasks. I hope you’ll code alongside me, not just copy my solutions. That way, we’ll be learning together!

# Mini-step #1: Map 1–9 to i, j

To start, let’s just make the simplest change possible. Let’s create a function that parses the user input into data we can use.

Remember how we defined the board for the user:

![](https://miro.medium.com/max/600/1*QFXmQSXzcDZjd5aatJ6mMw.png?q=20)



But! Also remember how we defined the board behind the scenes as a list of lists:

board = \[  
    \['\_', '\_', '\_'\],  
    \['\_', '\_', '\_'\],  
    \['\_', '\_', '\_'\]  
\]

We index into lists using bracket notation, so a given location on the board will exist at `board[i][j]` where `i` is the row (0–2) and `j` is the column (0–2).

In other words, our board looks like this, in terms of `i` and `j`:

board = \[  
    \[0|0, 0|1, 0|2\],  
    \[1|0, 1|1, 1|2\],  
    \[2|0, 2|1, 2|2\]  
\]

We’re going to get a number 1–9 from the user, and we need to convert it to an `i` / `j` format we can use.

Examples:

*   1 => 0, 0
*   4 => 1, 0
*   8 => 2, 1

**That seems like a tricky challenge. Think you’re up for it? Give it a try!**

**Define a function** `**convert_selection(selection)**` **that returns the correct i, j pair** [**as a tuple**](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences)**.**

…

…

_(Need a hint? Think_ [_floor division_](https://python-reference.readthedocs.io/en/latest/docs/operators/floor_division.html) _+_ [_modulo_](https://stackoverflow.com/questions/4432208/what-is-the-result-of-in-python)_, and remember indices start at 0 in Python.)_

…

…

My solution:

def convert\_selection(selection):  
    selection -= 1  
    return (selection // 3, selection % 3)

I subtract one so that my range is now 0–8. Then, I can use floor division and modulo to supply the rows and columns.

I return a tuple — an immutable data type that won’t change — using parentheses.

# Mini-step #2: Update the board

Now that we’ve converted the user input, we can use it to update the board!

For now, let’s just keep it simple. Let’s only place an “X” in the location the user chooses.

So, what we need is a function `place_piece(selection, board)` that takes in a selection tuple and the board as arguments and makes the correct change to the board.

It may not make sense that this should be its own function right now, but trust me. It will help us later.

**Give it a shot!**

…

…

…

def place\_piece(selection, board):  
    board\[selection\[0\]\]\[selection\[1\]\] = "X"

We index into the `selection` tuple to get the right value for indexing into the `board`. Woahhhhh, meta!

# Mini-step #3: Add these new functions into the game

If you’re following along, you’ll notice that these new functions work great, but we’re not using them anywhere yet!

Let’s fix that.

**Try incorporating the new functions into our existing game flow, in the correct places. At the end,** `**print_board()**` **again to make sure it works.**

…

…

…

Here’s what I have:

\# ttt.py  
  
def convert\_selection(selection):  
    selection -= 1  
    return (selection // 3, selection % 3)  
  
  
def place\_piece(selection, board):  
    board\[selection\[0\]\]\[selection\[1\]\] = "X"  
  
  
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
    selection = convert\_selection(select\_square())  
    place\_piece(selection, board)  
except ValueError:  
    print("Sorry, please select a number 1-9")  
print\_board(board)

Notice I nested `select_square` inside `convert_selection`. That way the return value gets automatically passed through without me needing to store it as a variable.

For that matter, we could continue the nesting to the next level:

board = \[\["\_" for \_ in range(3)\] for \_ in range(3)\]  
print\_board(board)  
try:  
    place\_piece(convert\_selection(select\_square()), board)  
except ValueError:  
    print("Sorry, please select a number 1-9")  
print\_board(board)

Nesting functions like that can lead to decreased readability. It’s really a judgment call and a matter of preference.

I think I like the more verbose and explicit first version. But they’re entirely equivalent for our purposes, so it’s up to you which you prefer.

And when we run it:

$ python ttt.py  
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
Select a square: 6  
\['\_', '\_', '\_'\]  
\['\_', '\_', 'X'\]  
\['\_', '\_', '\_'\]

It places the piece in the correct spot! Awesome.

# Mini-step #4: Re-prompt the user

So, we could just copy and paste the game code over again to get the program to re-prompt the user. But that’s definitely not good design.

How can we make it so that the user can make multiple turns in a row?

I’ll let you think about it for a little **while** and try to come up with your own solution.

…

…

…

_(Hint: If only there were some way to make the code_ **_loop_** _back on itself… ;) )_

…

…

My solution — just change a single line!

board = \[\["\_" for \_ in range(3)\] for \_ in range(3)\]  
while True:  
    print\_board(board)  
    try:  
        selection = convert\_selection(select\_square())  
        place\_piece(selection, board)  
    except ValueError:  
        print("Sorry, please select a number 1-9")

Now, this loop will continue forever and it has no way of knowing when the game has ended. We’ll worry about solving that problem later.

For now, though, you should know that `ctrl-c` will stop our program.

Let’s try it!

python ttt.py   
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
Select a square: 4  
\['\_', '\_', '\_'\]  
\['X', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
Select a square: 2  
\['\_', 'X', '\_'\]  
\['X', '\_', '\_'\]  
\['\_', '\_', '\_'\]  
Select a square: 6  
\['\_', 'X', '\_'\]  
\['X', '\_', 'X'\]  
\['\_', '\_', '\_'\]  
Select a square:

## Bonus: Refactoring to main()

The body of our program is starting to contain some real logic now. But it’s possible that not everybody who accesses `ttt.py` will want to play tic-tac-toe.

For example, they may be using `ttt` like a module where they want to use in a different program one the functions we’ve defined here.

So, as has been a common trope in this series, we should separate the game logic into its own `main()` function that runs when the file is executed from the command line, but doesn’t have to run for someone to use the file.

[Learn more about the main method](https://stackoverflow.com/questions/22492162/understanding-the-main-method-of-python).

To do so, we use some Python-specific syntax at the end of the file:

if \_\_name\_\_ == "\_\_main\_\_":  
    main()

Then, we move all the game logic into a main function that we can move to the top of the file:

\# ttt.pydef main():  
    board = \[\["\_" for \_ in range(3)\] for \_ in range(3)\]  
    while True:  
        print\_board(board)  
        try:  
            selection = convert\_selection(select\_square())  
            place\_piece(selection, board)  
        except ValueError:  
            print("Sorry, please select a number 1-9")def convert\_selection(selection):  
    selection -= 1  
    return (selection // 3, selection % 3)def place\_piece(selection, board):  
    board\[selection\[0\]\]\[selection\[1\]\] = "X"def print\_board(board):  
    for row in board:  
        print(row)def select\_square():  
    selection = int(input("Select a square: "))  
    if not 1 <= selection <= 9:  
        raise ValueError  
    return selectionif \_\_name\_\_ == "\_\_main\_\_":  
    main()

Wow, this is shaping up to look like a real application!

# Mini-step #5: Switching players

We’re so close to being able to play tic-tac-toe! But you’ll notice one big omission.

There’s only one player right now. Only ‘X’ can make moves!

We need to alternate between ‘X’ and ‘O’ on every iteration of the `while` loop.

**Can you figure out a way to do that? It’ll likely involve creating a new variable or two.**

…

…

…

_Don’t forget to update the place\_piece function._

…

…

…

Here’s my solution:

def main():  
    board = \[\["\_" for \_ in range(3)\] for \_ in range(3)\]  
    is\_x = True  
    while True:  
        player = "X" if is\_x else "O"  
        print\_board(board)  
        try:  
            selection = convert\_selection(select\_square())  
            place\_piece(selection, player, board)  
        except ValueError:  
            print("Sorry, please select a number 1-9")  
        is\_x = not is\_x...def place\_piece(selection, player, board):  
    board\[selection\[0\]\]\[selection\[1\]\] = player

I created two new variables `is_x` and `player`. `is_x` is a boolean that flip/flops every turn (i.e. — `is_x = not is_x` ). Then `player`is either X or O depending on the value of `is_x` .

But if you think about it, that’s redundant. It’s two variables that say basically the same thing, just one is a string and the other is a boolean.

**Redundancy = time to refactor!**

def main():  
    board = \[\["\_" for \_ in range(3)\] for \_ in range(3)\]  
    is\_x = True  
    while True:  
        print\_board(board)  
        try:  
            selection = convert\_selection(select\_square())  
            place\_piece(selection, is\_x, board)  
        except ValueError:  
            print("Sorry, please select a number 1-9")  
        is\_x = not is\_x...def place\_piece(selection, is\_x, board):  
    board\[selection\[0\]\]\[selection\[1\]\] = "X" if is\_x else "O"

Try it out!

$ python ttt.py  
Select a square: 5  
\['\_', '\_', '\_'\]  
\['\_', 'X', '\_'\]  
\['\_', '\_', '\_'\]  
Select a square: 1  
\['O', '\_', '\_'\]  
\['\_', 'X', '\_'\]  
\['\_', '\_', '\_'\]  
Select a square: 9  
\['O', '\_', '\_'\]  
\['\_', 'X', '\_'\]  
\['\_', '\_', 'X'\]  
Select a square: 2  
\['O', 'O', '\_'\]  
\['\_', 'X', '\_'\]  
\['\_', '\_', 'X'\]  
Select a square: 3  
\['O', 'O', 'X'\]  
\['\_', 'X', '\_'\]  
\['\_', '\_', 'X'\]  
Select a square: 6  
\['O', 'O', 'X'\]  
\['\_', 'X', 'O'\]  
\['\_', '\_', 'X'\]  
Select a square: 7  
\['O', 'O', 'X'\]  
\['\_', 'X', 'O'\]  
\['X', '\_', 'X'\]  
Select a square:

It works! We’re in business!

# Wrapping up

We got a lot accomplished today. We can now successfully play tic-tac-toe on our application.

It still doesn’t know anything about wins or draws. We need to monitor when the game has ended.

Also, if you play with the current version, X can overwrite a move of O and vice versa. We need a way to make moves permanent.

But we’re now well on our way and it’s exciting!

[Here’s today’s code](https://github.com/bennett39/ttt-medium/tree/day3).

See you next time!

## About Bennett

I’m a software developer in New York City. I do web stuff in Python and JavaScript.

Want to be updated when I release a new post? I have an [email list](http://eepurl.com/goJwcT) you can subscribe to. I have nothing to sell, only free content to share with the community, no time wasters. I’d love to have you there.

Check out the complete list of [all posts in this tic-tac-toe series](https://medium.com/@BennettGarner/the-tic-tac-toe-series-master-list-a4a908f015f9).