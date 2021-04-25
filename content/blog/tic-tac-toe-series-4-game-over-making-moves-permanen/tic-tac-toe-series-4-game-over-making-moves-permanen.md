---
title: Tic-tac-toe series #4 - Game over & making moves permanent
date: 2019-04-29T15:42:20.137Z
description: I’m writing a series of posts where we build an increasingly-complex tic-tac-toe game from scratch, one step at a time.
---



](/tic-tac-toe-series-4-game-over-making-moves-permanent-23bca3b40ce0?source=post_page-----23bca3b40ce0--------------------------------)

![](https://miro.medium.com/max/600/8/1*cVqljhyEyo6P2aa8UWIyhw.png)

I’m writing a series of posts where we build an increasingly-complex tic-tac-toe game from scratch, one step at a time.

Right now, it’s pretty rudimentary. But eventually, we’ll use this tic-tac-toe game to incrementally explore all kinds of concepts in software development.

Want to follow along? Here are [all the posts in the series](https://medium.com/@BennettGarner/the-tic-tac-toe-series-master-list-a4a908f015f9).

# We got problems

We ended the last post with a working application to play tic-tac-toe. Players can make moves and see their pieces displayed on the board.

[Here’s the code](https://github.com/bennett39/ttt-medium/blob/day3/ttt.py) from the end of the last post, for a refresher.

However, that application had some problems we need to address.

1.  The game currently allows players to overwrite one another’s moves. We need a way to make moves permanent.
2.  The app also keeps going infinitely, even after the game has been won or the board is full. We need it to terminate when there’s a win or a draw.

Those are our tasks today. Let’s get to it!

As always, we’ll use an incremental approach, breaking the problems down into miniature steps.

# Mini-step #1: No overwriting others’ moves

A player shouldn’t be able to make a move where a piece already exists on the board.

Or, in other words, you should only be able to make a move into an empty square.

Luckily, we already have a `place_piece` function defined. We can add in a line or two to check whether that square is empty before we place a piece there.

**Try it out on your own before looking at my solution!**

…

…

…

def place\_piece(selection, is\_x, board):  
    if board\[selection\[0\]\]\[selection\[1\]\] == "\_":  
        board\[selection\[0\]\]\[selection\[1\]\] = "X" if is\_x else "O"

We check that the square is empty before we place a piece there. That way, players can’t overwrite one another.

Of course, this creates new problems as we’ll see in a second. But first, let’s clean up our solution.

## Bonus: refactor

See the repetition of `board[selection[0]][selection[1]]` ?

That’s hard to read and it’s not intuitive what’s happening. Imagine if you didn’t know `selection` is a tuple.

Let’s make it more explicit and less repetitive:

def place\_piece(selection, is\_x, board):  
    i, j = selection  
    if board\[i\]\[j\] == "\_":  
        board\[i\]\[j\] = "X" if is\_x else "O"

# Mini-step #2: Raise an error

Right now, if you select a square that already has a piece in it, nothing happens. You don’t see an error message, and you just lose your turn.

Let’s make it so that the `place_piece` function raises a `ValueError` whenever a player selects an occupied square.

**Try it yourself!**

…

…

…

def place\_piece(selection, is\_x, board):  
    i, j = selection  
    if board\[i\]\[j\] == "\_":  
        board\[i\]\[j\] = "X" if is\_x else "O"  
    else:  
        raise ValueError

Now, if we look at the `main` function where `place_piece` gets called, we see that we already have it inside a `try` block:

try:  
    selection = convert\_selection(select\_square())  
    place\_piece(selection, is\_x, board)  
except ValueError:  
    print("Sorry, please select a number 1-9")

So, if we raise a ValueError in `place_piece` it will get handled along with the errors we already addressed in `select_quare` .

One thing we should change, though, is the error message. We need to be more descriptive now that we’re adding a new ValueError.

except ValueError:              
    print("Sorry, please select a square 1-9 that is unoccupied.")

# Mini-step #3: Turn forfeiture

Right now, if you make a mistake playing our tic-tac-toe game, it sucks to suck. You lose your turn.

Even if it’s a small typo or getting one of the numbers wrong.

Let’s fix that. See if you can find a way to not skip the player’s turn if they make a mistake.

…

…

…

(Hint: a “mistake” from the player is always handled in the `except` block. Can you change the control flow so that the player doesn’t lose their turn if the `except` block is entered?)

…

…

…

except ValueError:              
    print("Sorry, please select a square 1-9 that is unoccupied.")  
    continue

Just by adding `continue` to the `except` block, we allow the player another shot at their turn.

We’re such generous game designers! So tolerant of players’ errors ;)

# Mini-step #4: Game over

We’ve stopped players from overwriting one another’s moves.

Now, we need a way to stop the game.

For now, let’s just focus on stopping the game when there are no more empty squares. That should be easy.

Let’s create a new boolean `game_over` and a new function `is_draw(board)` . Check at the end of each turn and when the entire board is full, set `game_over` to True and stop the application.

**Try it on your own first!**

…

…

…

def main():  
    board = \[\["\_" for \_ in range(3)\] for \_ in range(3)\]  
    is\_x = True  
    game\_over = False  
    while not game\_over:  
        print\_board(board)  
        try:  
            selection = convert\_selection(select\_square())  
            place\_piece(selection, is\_x, board)  
        except ValueError:  
            print("Sorry, please select a square 1-9 that is unoccupied.")  
            continue  
        game\_over = is\_draw(board)  
        is\_x = not is\_x...def is\_draw(board):  
    for row in board:  
        for val in row:  
            if val == "\_":  
                return False  
    return True

I initialize `game_over` to False and then update it at the end of every turn. I also changed the criteria of the `while` loop to stop looping when `game_over` is True.

Meanwhile, the `is_draw` function simply looks at the entire board to see if there are any empty spaces. If so, it returns False. If it looks through all spaces without finding an empty one, it returns True.

Our application won’t run to infinity now!

Let’s add a line to let the user know why the application quit:

def is\_draw(board):  
    for row in board:  
        for val in row:  
            if val == "\_":  
                return False  
    print("Draw! No more moves!")  
    return True

# Mini-step #5: Check for a winner

The game should also be over if somebody wins, and our application should print out the winner.

In tic-tac-toe, there are 8 possible ways you can win (3 horizontal, 3 vertical, & 2 diagonal).

See if you can write a function `is_win(board)` that returns False if there’s no winner. If there is a winner, it should print the winner and return True.

…

…

…

Here’s my solution:

def is\_win(board):  
    winner = None  
    for i in range(3):  
        # horizontal  
        if board\[i\]\[0\] == board\[i\]\[1\] == board\[i\]\[2\] and board\[i\]\[0\] != "\_":  
            winner = board\[i\]\[0\]  
            break  
        # vertical  
        if board\[0\]\[i\] == board\[1\]\[i\] == board\[2\]\[i\] and board\[0\]\[i\] != "\_":  
            winner = board\[0\]\[i\]  
            break  
    # diagonal  
    if board\[1\]\[1\] != "\_":  
        if (board\[0\]\[0\] == board\[1\]\[1\] == board\[2\]\[2\]  
            or board\[0\]\[2\] == board\[1\]\[1\] == board\[2\]\[0\]):  
            winner = board\[1\]\[1\]  
    if winner is not None:  
        print\_board(board)  
        print(f"{winner} is the winner!")  
        return True  
    return False

I’ve hard coded the checks here because I don’t think we’ll expand the application to playing connect four or connect five or a bigger board in any way.

I’m okay with hard coding the checks because using relative checks would be an overreach. This a good example of the YAGNI principle. You Ain’t Gonna Need It, so don’t spend time building it.

Note: I also added a call to `print_board` at the end of the function. It’s a nice touch for the user to see the final state of the board.

I also added `print_board` to the end of my `is_draw` function while I was at it.

# Wrapping up

Our application now allows you to play tic-tac-toe, finds a winner when there is one, declares a draw when all the spaces are filled, and generally is a complete tic-tac-toe game!

That didn’t take long at all to implement.

In the next post, we’ll add testing and version control to our tic-tac-toe game to make sure it’s doing what we expect it to.

That’ll prove important when we create an AI for players to compete against, as we’ll need to test the AI.

[Here’s today’s code](https://github.com/bennett39/ttt-medium/tree/day4).

See you next time!

## About Bennett

I’m a software developer in New York City. I do web stuff in Python and JavaScript.

Want to be updated when I release a new post? I have an [email list](http://eepurl.com/goJwcT) you can subscribe to. I have nothing to sell, only free content to share with the community, no time wasters. I’d love to have you there.

Check out the complete list of [all posts in this tic-tac-toe series](https://medium.com/@BennettGarner/the-tic-tac-toe-series-master-list-a4a908f015f9).