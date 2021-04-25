---
title: Machine learning - A beginner’s guide to teaching artificial intelligence
date: 2017-12-05T00:50:28.678Z
description: 2017 is the year of hype for artificial intelligence and machine learning. Countless articles this year have extolled AI’s virtues…
---



](/machine-learning-a-beginners-guide-to-teaching-artificial-intelligence-40de2551ee0?source=post_page-----40de2551ee0--------------------------------)

2017 is the year of hype for artificial intelligence and machine learning. Countless articles this year have extolled AI’s virtues, explored the automation of menial tasks, and prophesied the end of work altogether. While we’ve all heard about AI’s potential after this year of endless press coverage, few have a basic understanding of how the technology works.

I’m not a computer scientist or AI expert. I’m still very much a student of AI, and I hope writing a brief primer aimed at non-technologists will help me (and maybe you?) understand how artificial intelligence learns a little better.

# How I explain machine learning

Machine learning is giving a computer the ability to learn without being explicitly programmed.

So, let’s take a step back to understand explicit programming. Explicit programming is how most of the software you’re familiar with works. Traditionally, software has defined goals, straightforward inputs, and clear outputs. While explicit programming can get very complicated, it is essentially a set of rules. For every action or input, the software follows a set of rules to generate an output.

# The problem with explicit programming

Explicit programming has served us well. It’s what powers the personal computer and the internet. It runs nearly all the software we use daily. Explicit programming is what we think of when we think of coding and computer science.

However, explicit programming has limitations, and those limitations are largely two-fold. First, since explicit programming is all about inputs, rules, and outputs, it relies on a software developer to write rules that apply to every possible input. In situations where there are hundreds or thousands of variable inputs, it becomes impractical to explicitly program rules for all the possible variables.

Second, explicit programming is static and bad at prediction. In most cases, once a software developer has written a piece of code, that code won’t change. It doesn’t have the ability to update itself in response to new variables. This static nature makes explicit programming bad at prediction, since it lacks the ability to fine tune its predictions when it gains new data.

“Tuning predictions in response to new data” is a very nerdy way of saying that explicitly programmed software can’t learn. But since the 1960s, computer scientists have been working on changing that.

# The golden age of machine learning

Helping a computer to learn requires two main things:

1.  A large dataset so the computer can make hundreds or thousands of mistakes while tuning its predictions
2.  Lots of processing power to assemble and examine the data and run hundreds or thousands of tests

We’re entering the golden age of machine learning because those two things are now readily available. Storage and processing power have increased exponentially over the decades in a phenomenon known as [Moore’s Law](https://en.wikipedia.org/wiki/Moore%27s_law).

With these two hardware challenges overcome, machine learning now relies on writing algorithms that tell a computer what to look for in a data set, without explicitly coding how to find the answer. The computer then uses trial and error, breeding the successful trials and adding random variation to try new things. Over the course of many tests and variations, the computer learns how to succeed at the task at hand.

It might be easiest to understand how the algorithm works visually. This video of a computer learning to play Super Mario is a great introduction:

# How is machine learning used today?

Machine learning has tons of applications, some of them are visible while many of them are hidden.

*   Machine learning helps Amazon decide which products to recommend to you based on your past purchases
*   Facebook uses a branch of machine learning known as computer vision to suggest which friends you should tag in your photos
*   Machine learning helps Siri and Alexa with voice recognition through trial and error
*   Gmail relies on machine learning to identify and filter spam
*   Netflix uses machine learning to predict which shows you’ll like based on the ones you’ve watched in the past

Basically, machine learning is useful anywhere a computer needs to make a prediction or a judgment. It can apply to fraud detection, bioinformatics, advertising, and financial modeling.

# Exercise caution: Machine learning has flaws

Machine learning is only as good as the algorithm it uses and the data set it gets trained on. Because machine learning is so complex, its code is notoriously difficult to debug and write correctly. When a machine learning algorithm fails, it can be difficult to pinpoint exactly why it failed.

Another problem is machine learning inherits the biases that are present in your data set. If your data is imprecise, skewed, or incomplete, the algorithm will still try to make sense of it. Conclusions drawn from too small of a data set are often outright wrong. Insufficient data is one reason why machine learning projects fail to deliver.

More insidious, however, are machine learning projects that inherit racial, class, age, or other biases from the data collectors or algorithm creators. Machine learning is only as good as the data it is trained on, and creating ethical datasets is a subject for an entire blog post in itself.

# The promise of AI

Over the coming years, AI will certainly make our lives easier by automating menial tasks. It’s possible these tasks could get as complex as self-driving cars and automated checkout at retail stores. While the jobs lost to automation is an important concern, another key is making sure everyone has equal access to automation and is treated fairly in a new automated world. Unfortunately, we haven’t found a way for a machine learning algorithm to teach ethics or morality quite yet.