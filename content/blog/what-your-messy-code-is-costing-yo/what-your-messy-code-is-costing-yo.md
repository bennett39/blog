---
title: What Your Messy Code Is Costing You
date: 2019-01-14T16:11:10.132Z
description: Right now, naming that variable or building that helper function doesn’t seem critical… but messiness will catch up with you over the long…
---



](/what-your-messy-code-is-costing-you-3317e419df3a?source=post_page-----3317e419df3a--------------------------------)

Right now, naming that variable or building that helper function doesn’t seem critical… but messiness will catch up with you over the long haul.

![](https://miro.medium.com/max/600/0/1*VTBnsH3k7q7nSb5EpNnkZg.png)

# The Cost of a Mess

I’m currently reading Robert C. Martin’s _Clean Code_. It’s a book that has become a touchstone in software development because it focuses on a problem so many developers have encountered: messy code bases and how they get that way.

If you’re a working developer or a student of [writing good software](https://blog.usejournal.com/the-pragmatic-programmer-is-essential-reading-for-software-developers-443940b8ef9f), this book is an important read. In this short article, I’ll share the basic tenets of clean coding, and why you should care.

In the first chapter, Martin tells the story of a software project:

*   The project starts off great, and the developers on the project are highly productive while the code base is small and they can understand what the whole program does
*   Over time, however, new stuff gets added and new people join the team. It becomes difficult for the established developers to remember what code does what and good luck to the new devs learning the code base
*   Soon, every change to the code creates two new issues somewhere else. Productivity grinds to a slog as you’re constantly putting out fires and resolving bugs
*   As the mess builds, the productivity of the team continues to decline, approaching zero
*   Eventually, the developers working on the code demand a redesign and a team is put together to build a replacement
*   However, the rest of the devs on staff still have to maintain the old code base, and management won’t replace the old with the new until the new one has all the features customers have come to expect from the old application. This race between new and old can go on for years!

Chances are if you’re a developer with any experience, you’ll have encountered at least some part of this software story. This is, unfortunately, the status quo for many applications.

# It’s Not One Person’s Fault

It’s easy to point fingers at management: “They should commit more resources to the redesign!”

It’s also tempting to blame the devs from the past: “What were they thinking?! I would have never written this code this way!”

However, reality is always more complicated. Those managers and developers were working hard, under deadlines, to get their jobs done and projects completed. Nobody sabotaged the project on purpose, and it’s not one person’s fault that the code looks the way it does.

Messy code happens collectively. So, too, does clean code. When there’s a culture of cleanliness on your dev team, respect for the demands of writing good code from management, and a personal belief and dedication to writing the cleanest code possible from each individual developer.

# But It’s Everyone’s Responsibility

If you ask Robert C. Martin how code gets messy, he doesn’t pull any punches:

> “We complain that the requirements changed in ways that thwart the original design. We bemoan the schedules that were too tight to do things right. We blather about stupid managers and intolerant customers and useless marketing types and telephone sanitizers. But the fault, dear Dilbert, is not in our stars, but in ourselves. We are unprofessional”

The key point here is if you want to point fingers at someone for messy code, look in the mirror first. Take massive ownership over the project you’re working on and its current state.

As a developer, your job is the code! You’re the one who must stick up for it. Speak your mind when the requirements change or when the schedule gets too tight.

Again, Martin has sage advice:

> “Most managers want the truth, even when they don’t act like it. Most managers want good code, even when they are obsessing about the schedule. They may defend the schedule and requirements with passion; but that’s their job. It’s your job to defend the code with equal passion.”

# The Deadline & the Mess

All developers feel pressured to make messes in order to meet the deadline. It’s how a lot of messy code gets written in the first place.

However, as soon as you make a mess, you’ve already slowed yourself down. When you make messes you can’t go fast over the long run, and being fast over the long run is what makes for a successful project.

Professional coders who have studied their craft know that making a mess is never faster. In many cases, the mess you just made to meet your deadline will actually force you to miss it.

The only way to make your current deadline and the deadlines to come is to keep the code clean.

# How Do I Fix Messy Code?

It’s one thing to be able to recognize messy code. It’s another thing entirely to see ways you could refactor it more cleanly.

Recognition vs creation of clean code is a bit like art. Most of us can appreciate a beautiful painting when we see it. That doesn’t mean we could paint it ourselves.

Writing clean code is a skill that you’ll need to hone and [practice over many projects](https://blog.usejournal.com/consider-yourself-a-developer-you-should-solve-the-project-euler-problems-ed8d13397c9c) and working on many teams. You won’t be perfect at first — in fact, there’s no such thing as perfect. Instead, writing clean code is a constant journey as a programmer.

That said, there are resources for you online and in print to help you on your journey as a clean coder. Martin’s Clean Code is a great place to start.

# One Simple Rule to Get You Started

Many books have been written about the path to clean code. I can’t possibly cover all the techniques and philosophies in this one post.

I will, however, share one simple rule that will put you on the path.

Remember that your code will likely get read by someone else (maybe even future you), so practice the first rule of clean code:

“Leave any code you’ve touched cleaner than you found it.”

It might be a small change — a variable name or breaking one function into smaller ones — but if everyone constantly made these improvements it’d work like compound interest. You’d have code that gets cleaner and tighter with every developer who touched it.

That’s the opposite of what usually happens and a worthy goal for any dev team.