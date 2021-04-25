---
title: Refactoring - 5 Steps to Improve Existing Code
date: 2020-07-14T12:42:28.011Z
description: As a developer, most of your job is updating existing code. These are the heuristics I use when refactoring…
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F70e9e5af4ce3&operation=register&redirect=https%3A%2F%2Fjavascript.plainenglish.io%2Fhow-to-refactor-existing-code-70e9e5af4ce3&source=post_actions_header--------------------------bookmark_preview-----------)

![](https://miro.medium.com/max/600/1*x6Oj5uI7u9KhI_av7kW7WQ.png?q=20)



The hardest & most frequent thing you’ll do as a developer

As a developer, most of your job is updating existing code. However, there aren’t many guides or tutorials to teach you how to successfully refactor and what to look for.

In this post, I’ll share a simple set of guidelines that have served me well when I have to refactor.

# Why Refactor?

It’s amazing how quickly a code base can become unintelligible. It’s not that the original writers were bad coders. The requirements just changed, and things got added on the fly.

The decisions seemed reasonable back then. Rule #1 of refactoring: blaming someone else won’t help anything.

So why refactor?

*   Because it makes code easier to understand, and code that’s easier to understand is easier for the next developer to update
*   Often, it makes your code execute faster
*   It forces you to really understand how the code works & why it’s doing that work, making you a better developer
*   In the future, refactored code is more flexible. Reusing logic, consts, & data structures makes code easier to extend.

# When to Refactor?

I see engineering teams get stuck in a loop of refactor after refactor. How do I decide what’s worth my time to fix?

*   Always assume you won’t be the next person to touch this code (or future-you will forget how the code works). Right now while you understand it is the best time to clarify it.
*   That doesn’t mean you have to re-write how it fundamentally works (though you might). Many times, cleaning a few things up and renaming other things makes a big difference.
*   If you’ve been assigned a specific task/ticket, don’t refactor code that’s outside the scope of your ticket. Limit the scope of your refactors.
*   You should always strive to leave the code better than you found it.

When should you look for code smells and ways to make the code simpler? Always.

When should you undertake a massive refactor that will touch many parts of the code base? Only when you have been explicitly assigned it and have support from your whole team.

# My 5 Favorite Refactoring Heuristics

> “A heuristic is any approach to problem solving or self-discovery that employs a practical method that is not guaranteed to be optimal, perfect, or rational, but is nevertheless sufficient for reaching an immediate, short-term goal” — [Wikipedia](https://en.wikipedia.org/wiki/Heuristic)

While they won’t cover every possible refactoring opportunity, these 5 heuristics help me a lot to simplify code.

# 1\. Have Tests

When you refactor something, the first step is to [check that there are tests](https://en.wikipedia.org/wiki/Test-driven_development) for this functionality.

Changing existing code means you could break something, so before we make any changes we want to know the expected responses the original code was producing.

If you don’t have tests, the first step in refactoring is writing them.

# 2\. Reduce If/Else

Any time you add an if-statement to your code, you increase its complexity. The next person who reads it will have to keep in mind all the different possible code paths they’ve encountered.

Often the same result can be achieved by moving work into [functions that return early](https://stackoverflow.com/a/1804276).

Elements of [functional programming](https://maryrosecook.com/blog/post/a-practical-introduction-to-functional-programming) will also help [reduce conditional logic](https://medium.com/swlh/stop-using-if-else-statements-f4d2323e6e4).

Ideally, your code shouldn’t be nested (indented) more than a few layers deep.

# 3\. Use Variable Names to Clarify

If your variable names are unclear, wrong, or too generic, that makes the code harder to read.

This is an easy win, usually. Just find and replace the bad variable names with good ones.

Also, turn complex conditional logic into variables that store booleans. If you’re calling functions only to see if they have a value, then save those as variables with clear names:

has\_thing = self.logic.find\_the\_thing(lookup, other\_stuff)

Always be looking for ways to clarify & simplify variable names. Naming is one of the hardest parts of coding.

# 4\. Functions Should Do Only One Thing

When you write functions & refactor other people’s functions, keep this in mind. They should be as dumb as possible.

I wrote this function yesterday:

def get\_company\_override\_rate(self, date):  
    “”” Find the currency conversion rate for this time period “””  
    date = arrow.get(date)  
    rate = self.conversion\_rates.get(date.format(‘YYYY-MM’))         
    return rate

I’m not saying this function is the pinnacle of simplicity. There’s more that could be done to make it simpler. But it’s short and immediately clear what it does.

Python programmers will notice that this function returns `None` if the date isn’t in the dictionary. The code does that to Reduce If/Else (see #2 above) when a bunch of these small functions get chained together to do more complex logic.

Keep your functions simple and let them call one another.

# 5\. Store Useful Info at the Class / State Level

If you see a function that takes a lot of arguments, then chances are it needs some class or state object.

Using `this.state` or `self.attribute` are common patterns, though the syntax changes depending on your language.

When you store data on the class instance, you can reference that data across different methods on the class. Then, initializing the class calls standard logic to populate those values.

You get a more consistent result that you can reuse in many places without repeating yourself.

# Follow Your Nose

These are just guidelines I’ve found helpful. Ultimately, the code you work on will be a lot different from mine, so you’ll just need to develop intuition about what code needs to change.

If you smell something fishy in the code, follow your instincts. Almost certainly, something could be improved.

Want to go deeper on refactoring? Definitely read Martin Fowler’s book — [Refactoring: Improving the Design of Existing Code](https://www.amazon.com/Refactoring-Improving-Design-Existing-Code/dp/0201485672)

Have more refactoring heuristics you use? I’d love to hear about them — use the comments or hello@bennettgarner.com

## About Bennett

I’m a software developer in New York City. I do web stuff in Python and JavaScript.

Like what you’ve read here? I have an [email list](https://mailchi.mp/2e671faffc04/bennett-medium) you can subscribe to. Infrequent emails, only valuable content, no time wasters. I’d love to have you there.