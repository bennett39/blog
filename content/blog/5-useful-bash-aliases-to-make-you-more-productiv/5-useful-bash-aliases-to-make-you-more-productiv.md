---
title: 5 Useful Bash Aliases to Make You More Productive
date: 2021-03-26T12:26:21.013Z
description: Take advantage of aliases to master the command line
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F12c04b550479&operation=register&redirect=https%3A%2F%2Fjavascript.plainenglish.io%2F5-useful-bash-aliases-to-make-you-more-productive-12c04b550479&source=post_actions_header--------------------------bookmark_preview-----------)

![](https://miro.medium.com/max/600/1*0xwHLwFv3YO6HbYNs7G_kw.jpeg?q=20)



Photo by [Jan Canty](https://unsplash.com/@jancanty?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/coy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

The best developers I know all maintain a list of Bash aliases to make their lives easier. It’s one of the signs of a good developer — how much can you automate the tasks you do repeatedly?

Bash aliases are super easy to create, too! It’s low-hanging fruit to reduce your time, typing, and mental energy while coding. In this post, I’ll explain why every developer should have a list of aliases, how to do it, and the ten I’ve found most useful.

# Why should I use Bash aliases?

Bash aliases are a major key to my productivity as a developer. There are three reasons why Bash aliases help so much:

1.  **They’re faster**. This may be obvious, but typing less saves time. That time compounds if you alias a command that you use often. So, shaving 1–2 seconds off a command you use multiple times per day can result in hours of time saved over the course of a year.
2.  **They’re easier to type**. As a developer, your hands and wrists are an important asset. It’s a huge deal to reduce the amount of work they have to do over the years.
3.  **They’re easier to remember**. This is the biggest reason for me. I have a hard time remembering the syntax for commands, especially commands that have non-intuitive arguments. With aliases, I set the command once and just use the alias forever.

These may seem like small reasons, but together they can make a big difference. Once you have aliases for several common commands, the time, effort, and energy savings begin to compound.

Convinced? Read on!

# How do I create a Bash alias?

The syntax for creating an alias is `alias foo="< command >"`. You can try it right now on the command line!

$ alias foo="echo 'I pity the foo'"  
$ foo  
I pity the foo

Here, I created an alias for a Mr. T quote. (If you haven’t seen the Rocky movies, then what are you doing?!?)

Unfortunately, if I open a new terminal window or restart my shell, the `foo` command disappears! Creating aliases directly means they only live within your currently active session.

To have an alias set predictably on every session, you’ll want to edit `~/.bashrc` (create that file if you haven’t already).

Adding a line to `~/.bashrc` makes the `foo` command available all the time, across my sessions:

Now reload your shell with `exec bash` and you should now have the foo command on any new session!

# My Top 10 Bash Aliases

Here are the aliases I think everyone should include in their `~/.bashrc`

## 1\. Find a file recursively

alias f='find . |grep '

If I’m in a complex directory and looking for the file `rocky.js`, I can recursively find the file name with `f rocky`.

## 2\. Find an old command

Sometimes you know the command you need is in your history somewhere, but where?

alias h='history|grep '

If I can’t remember the syntax or arguments of a command, I often search my history for the last time I ran it using `h <command_name>`

## 3\. Commit to Git

I commit code to Git many times per day, so cutting down on typing really saves me time.

alias gc='git commit -m'

When I want to commit code it’s just `gc "<commit message>"`

I also have aliases for `git push` and `git pull origin`.

## 4\. Pretty listing

I use `ls` all the time to list the contents of a directory. At this point, I’ve forgotten that my version of `ls` is an alias, but the output is much easier to read.

alias ls='ls -CHG --color'

This command enables colors, columns, symlinks, and other nice features of `ls`.

When I need more detail, I use my other `ls` aliases (BONUS aliases!):

alias la='ls -ahG'  
alias ll='ls -ahlG'

Here’s the output in a sample directory:

![](https://miro.medium.com/max/600/1*rur8DcOv4m_lbjukSrmC6g.png?q=20)



## 5\. Making directories

When I’m creating new directories, I want to:

*   Be able to create parent directories at the same time (i.e. give a full path to my target new directory)
*   Be notified when I create a directory so I can beware of typos

This alias for `mkdir` does all that:

alias mkdir="mkdir -pv"

# That’s just the beginning!

We did it! That’s 5 essential Bash aliases to get you started, but they can become the foundation of many more.

Pay attention as you’re coding to the commands you use most often. Are you typing more than you need to? That might be a good candidate for your next alias.

Aliases can be a game-changer on the command line, so I’m happy if this article introduced you to some new ones!

## Want to level up?

I’ll send you my best content on web development and becoming a better programmer, for free.

[Join 500 other developers in my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).

[

## Free Email Series for Medium Readers | Bennett Garner

### Join over 500 other developers currently in my email series.

mailchi.mp







](https://mailchi.mp/2e671faffc04/bennett-medium)