---
title: Developers - Your Poorly Named Variables Are Hurting Your Team
date: 2019-01-16T17:03:57.147Z
description: If you’re a developer that uses the first variable name that comes to mind, you’re probably making your code hard to read, use, and…
---



](/developers-your-poorly-named-variables-are-hurting-your-team-a8ec31e3cbd5?source=post_page-----a8ec31e3cbd5--------------------------------)

If you’re a developer that always uses the first variable name that comes to mind, you’re probably making your code hard to read, use, and maintain.

![](https://miro.medium.com/max/600/0/1*tFyjCV0Q8jlR3ZAWdQcfYA.png)

# Motivation: Why Naming Matters

As developers, we’re constantly naming things. Variables, functions, classes, and modules all need names. You likely don’t think about it, but a major portion of a developer’s workflow is choosing and using names for abstractions.

For example, I just looked through a ~100 line [twitter bot](https://github.com/bennett39/twitter-bot) application I wrote. Even in this relatively small program, I found 40 unique names for variables, functions, classes, and methods. Of those forty names, twenty-three were names somebody else came up with that I had to learn (Tweepy, tqdm modules). The remaining seventeen were all names I created in just 100 lines of code! (Not all the names are perfect, either — if you notice something I should change, send me a pull request.)

Since names are so ubiquitous in software development, getting good at naming is a critical skill for every developer. It is foundational to what we do, and the code we write will include hundreds or thousands of names for things.

When you name things in a vague or confusing way, the code takes more time and effort to read. Other developers who work on your code (or even your future self) will scratch their heads wondering what that method does and why you chose to name a variable `a1`.

When you name things clearly, on the other hand, the code becomes readable. When a developer needs to add new code its obvious what each variable stores and what each function does from the names themselves. Good naming is good coding. It’s fast to write, understandable to read, and easy to talk about.

Hopefully I’ve convinced you that naming is important. In this guide, I’ll share the best practices for naming things. Of course, there are opinions and variations on these rules, so my word is not law. It is, however, a good starting point and backed up by reading books like Clean Code and The Pragmatic Programmer.

# **Write It Out**

For some reason, many developers are scared of longer names for things. So, they abbreviate or create pseudonyms for long words. Something like this isn’t too uncommon:

int d; // Distance in feet

Of course, `d`is shorter and smaller on the screen, but is it really easier to use than a variable named `distanceInFeet`? Every use of `d`takes a mental effort to remember what `d`stands for.

This gets even more taxing when the application grows to require some measurement of time. Now, developers will have to do a double take to remember if \`d\` stands for “distance” or “days”.

The only exception I can think of to the rule against abbreviations and single character names is the convention of using `i` / `j` / `k`as iterators in for loops. All developers know these iterators, so using them isn’t a mental burden in the same way.

If you’re using any decent code editing tool like Sublime, Atom, Vim, Emacs, or Visual Studio, then code completion should be built in or you can add it as a plugin. This means that typing time and misspellings aren’t really a valid objection to longer names. Once you’ve defined the variable or function, your editor will remember the name for you and type it flawlessly with a few keystrokes.

# Avoid Redundancy & Bloat

Just because a name is long doesn’t make it good. While you shouldn’t be scared to use longer names, use that extra space only for a good purpose. Don’t bloat your names or add unneeded fillers.

Something like

list\_of\_users = \[‘alice’, ‘bob’, ‘charlie’\]

is 2/3 redundant. Any developer can tell that’s a list just by looking at it. No need to include “list” in the variable name, then. Instead, just calling the variable `users`works well enough.

Similarly, calling a variable `time_amount` isn’t meaningfully different from calling it `time`. If you need two variables for time, use the name to tell what sets them apart. Don’t just make them variations on one another. For example, using `time_1` and `time_2` would be just as bad because it doesn’t give any context. Instead, perhaps `start_time` and `stop_time` are a little better. `start_time_utc` might be even better, depending on the context.

# Focus on Intention

*   The name of a variable/class should be a noun that tells what it stores
*   The name of a function/method should be a verb that tells what will happen when you call it

For example, good variable names (depending on the context) might be `unregisteredUsers`, `itemsOnSale`, or `countOfAccounts`.

Good function names might be `getUserBalance()`, `setItemPrice()`, or `isRegistered()`.

The key to making simple names like this is to actually keep your functions simple. If a function has to do many things, it’s going to be hard to come up with a simple name for it. However, when each function does only one thing, naming and calling functions gets much easier.

The same goes for classes. When they start to get unruly, that’s a smell that you need to separate them out and nest them together in simpler component pieces.

That said, you should spend some time thinking about your names. A name should make it clear what you’ll get when you call it. If I see a variable called `users`, I expect a list. If it’s an integer count of the number of currently logged in users, then the name should be `userCount`, `countOnlineUsers`, or similar.

# Consistency Is Key

Sometimes synonyms get mixed up in your naming, leading to confusion. Was that method named `.addUser()` or `.createUser()`? Why do I `.removeUser()`but I `.deleteAccount`?

Consistency in naming is critical, especially as your application grows. You should use only one word per concept in your naming scheme. For example, decide early on if you’re going to use `get`or `fetch`in your function names. Will it be `set`, `put`, or `update`?

For variables and classes, use the words specific to your problem domain and stick to a certain set. Decide if it’s a `cart`or a `basket`, a `like`or a `favorite`, a `message`or a `chat`.

# The Fundamentals of Naming

This post has only covered the basic principles of good naming. But if you follow the guidelines outlined here, you’ll be writing more readable code than many developers out there.

Your employer and teammates will have their own preferences and conventions about naming, and you should learn from them. However, don’t be shy to stand up for the code and point out bad names when they arise. The longer you leave a bad name, the more likely it is to get baked into the system and the harder it becomes to change.

Did I miss anything? Let me know in the comments below! Also curious to hear about any naming nightmares you might have encountered as developers.