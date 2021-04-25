---
title: Consider Yourself a Developer? You Should Solve the Project Euler Problems
date: 2018-12-11T16:32:17.222Z
description: I’m going to make a bold claim - every developer (and aspiring developer) should solve the first 50 Project Euler problems.
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fed8d13397c9c&operation=register&redirect=https%3A%2F%2Fblog.usejournal.com%2Fconsider-yourself-a-developer-you-should-solve-the-project-euler-problems-ed8d13397c9c&source=post_actions_header--------------------------bookmark_preview-----------)

![](https://miro.medium.com/max/600/1*0NtkTQD8trRauRfEU2Nmhg.png?q=20)



I’m going to make a bold claim: every developer (and aspiring developer) should solve the first 50 [Project Euler problems](https://projecteuler.net/archives).

The benefits of working on the Project Euler problems are too great to ignore:

*   They’ll expose whether you understand the basic operations of the language you’re using, and they can be solved in any programming language
*   You’ll learn how to write solid, standard, vanilla code. No libraries needed, so you won’t have crutches to rely on
*   You’ll have to think about how you store and access data in memory
*   You’ll need to optimize processing speed and think hard about algorithms

In this quick (<5 minute read) article, I’ll make my case for why you should solve the Project Euler problems in every programming language you want to learn.

# Start with the fundamentals

![](https://miro.medium.com/max/600/1*pcJLGFtzjuKj2gduQO5ONA.jpeg?q=20)



Vince Lombardi with Bart Starr — Public Domain

Vince Lombardi, the legendary American football coach, would start every pre-season training with a (now famous) speech:

“Gentlemen,” he’d say to a room of professional football players, “this is a football.” He’d go on to explain the fundamentals of playing and winning at the game — running, passing, tackling, and kicking.

Lombardi’s ruthless focus on the fundamentals made him the winningest coach in the history of football, with more championships (5) than any other coach in history.

The same principles apply to software development.

Any coding challenge will test your ability to use variables, conditional statements, loops, [data structures](https://medium.com/@BennettGarner/what-the-graph-a-beginners-simple-intro-to-graphs-in-computer-science-3808d542a0e5), and other common features of programming languages.

Project Euler, however, is an especially rigorous test of your understanding and command of the fundamentals. You’ll have to combine the fundamentals in different permutations in order to solve the problems.

And the fundamentals are all you’ll need. If you’ve been coding web apps, games, or other software, chances are you’ve become [used to using certain libraries](https://medium.com/@BennettGarner/new-to-react-you-need-to-understand-these-key-concepts-before-anything-else-2247efc1eaac) to accomplish specific, predictable tasks.

Project Euler takes away those libraries and purely tests your ability to reason about simple logical problems and write simple code that addresses those problems.

# Training for something that’s hard to teach: Optimization Intuition

There’s a reason why every Computer Science undergraduate program includes a course in Algorithms & Data Structures, and why so many companies test algorithmic thinking in their hiring process.

At its core, writing good code is about [**applying the best solution**](https://medium.com/@BennettGarner/the-pragmatic-programmer-is-essential-reading-for-software-developers-443940b8ef9f) **to the logical problem at hand**.

The challenge is to know when to spend time optimizing and when to go with the simple solution.

## Sometimes the straightforward solution is the best solution.

The solution that is the most readable to other humans and intuitively makes sense is often the best.

The first few Project Euler problems will teach you this lesson. They involve simple computations with (relatively) small upper bounds. Computers are fast, and there’s no need to create work for yourself over-optimizing the first few solutions.

## Other times, you’ll need to optimize in order to see real performance gains

What a theoretical Algorithms class in university can’t teach you is what it feels like to wait for a slow algorithm to compute the answer to a problem.

A great developer has intuition about efficiency. Over time, experienced devs develop a sixth sense for sub-optimal implementations of solutions to problems. You develop that intuition from experience, and the later Project Euler problems are great teachers of what efficiency intuition feels like.

If an algorithm is going to get used often or on large inputs, spending time on optimization is worth it for the time savings down the line. In other cases though, it’s not worth the extra time to optimize a solution that will only be used on a small-scale.

Judging that optimization trade-off is the intuition that experienced devs have, and it’s a skill that Project Euler can begin to teach.

# You’ll become more fluent and confident in your language of choice

Solving even a handful of Project Euler problems will work wonders on your confidence and fluency in a given language. It’s also a great way to pick up a new language or refresh a language you used to know.

For instance, I’ve been [coding in Python](https://medium.com/@BennettGarner/why-i-code-in-python-a1e4012eb859) pretty exclusively for the past few months, and it has been a while since I wrote anything in C — a language I knew before Python.

I started implementing the first few [Project Euler problems in C](https://github.com/bennett39/euler) to refresh my skills. It has worked wonders to bring back my skills and remind me of the syntax. Similarly, I used Project Euler to learn the basics of Java, since Java’s syntax is similar to but different in key ways from C.

# Great chance to practice documentation and testing

If you want to add something impressive to your portfolio, start a GitHub repository where you keep your solutions to the Project Euler problems.

> (_NB: multiple comments have mentioned that sharing your Project Euler solutions is against the competition’s rules. If you’re competing on current Project Euler challenges, join the official forum and discuss code there._
> 
> _However, for the archived problems that have been solved hundreds of thousands of times, there’s a lot to gain from adding Project Euler code to your portfolio, like hundreds of other developers have before you._
> 
> _Just make sure you don’t look at other people’s solutions when developing your own! That spoils the fun and means you won’t learn as much!)_

When you complete a solution, check your variable and function names to make sure they’re intuitive, remove any code you don’t need, and format everything beautifully and consistently.

Next, add comments about why you made the decisions you did in your code. [Learning to write good comments is a skill](https://medium.com/@BennettGarner/youre-commenting-your-code-too-much-and-other-controversial-thoughts-on-documentation-1ee617ed46af). A good rule of thumb: any good developer can figure out WHAT your code does, so your comments should explain WHY it does it that way.

![](https://miro.medium.com/max/600/1*GPkJfAn4SyEwyxClJb7jhg.png?q=20)



Another great opportunity with Project Euler is to learn unit testing. Add a minimal testing framework to your projects and write tests before you write the solution. This is a great way to get started with Test Driven Development.

# Project Euler, ftw!

If you solve the first 50–100 Project Euler problems you will:

*   Have a solid command of programming fundamentals
*   Be confident and fluent in the syntax of your chosen language
*   Learn about algorithm optimization, and — more importantly — optimization intuition
*   Gain experience writing clean code with good documentation
*   Have an opportunity to practice test-driven development
*   Have something impressive to show on your GitHub and portfolio

I don’t know of any other set of problems that’s as well suited to teach and demonstrate your command of programming fundamentals. Project Euler is a must-do for any developer or aspiring developer.

## Free Resources

Want my best content on web development and becoming a better programmer?

[Join my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).