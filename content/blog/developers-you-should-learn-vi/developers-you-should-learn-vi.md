---
title: Developers - You Should Learn Vim
date: 2021-04-20T17:06:31.464Z
description: The most productive developers I know all have something in common. They know how to use Vim.
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9f05b2016307&operation=register&redirect=https%3A%2F%2Flevelup.gitconnected.com%2Fdevelopers-you-should-learn-vim-9f05b2016307&source=post_actions_header--------------------------bookmark_preview-----------)

The most productive developers I know all have something in common. They know how to use Vim.

![](https://miro.medium.com/max/600/1*_vYlN6CNpdtcf530sU_biQ.png?q=20)



Welcome to Vim

I’m not telling you to switch to using Vim all the time (although you could). But in this article, I’ll argue why learning a 30-year-old text editor is important if you want to get good at software development.

# The Case for Vim

Vim is:

*   **Incredibly fast to use (once you learn it)** — Vim commands all come from the keyboard, and they allow you to edit entire words, lines, or paragraphs of text with individual commands. Editing in bulk makes Vim shockingly fast to use, once you internalize the commands.
*   **Incredibly performant (no more waiting for files to open)** — If you’ve ever waited for your IDE to start up when you’re just trying to simply edit one file, you know what I mean. Vim opens files instantly and editing is super fast. No waiting. Vim’s lightweight footprint is part of why it’s pre-installed on so many operating systems and servers.
*   **Available everywhere (most importantly: on machines you SSH to)** — This is the reason why the best developers I know use Vim. They often prefer to code in an IDE locally, but when they’re on a remote server or working on an unfamiliar machine, Vim is nearly always pre-installed to edit text quickly.

The combination of speed and availability makes Vim pretty hard to beat.

# Why Doesn’t Everybody Learn Vim?

Simply put: learning Vim is hard.

Don’t let that scare you. You can learn the basics of Vim in just a few minutes using `$ vimtutor` or an online teaching tool like [Open Vim](https://www.openvim.com/).

![](https://miro.medium.com/max/600/1*TGxvgjQeKBdbh5l6oHeJwQ.png?q=20)



But it will take you a while to internalize Vim’s commands and capabilities. For the first few weeks of using Vim, you’ll feel like you’re going slower! It’s tough to remember all the commands at first.

After about a month of using Vim key bindings, though, I promise you’ll be more productive than you were in your old editor.

Several months in, you’ll wonder how you ever lived without Vim.

## Want my best content on web development and becoming a better programmer?

[

## Free Email Series for Medium Readers | Bennett Garner

### Join over 500 other developers currently in my email series. Including: What technical skills you need to learn, and…

mailchi.mp







](https://mailchi.mp/2e671faffc04/bennett-medium)

# The Tricky Part: Vim Has Modes

When you type in a normal text editor or IDE, you manipulate the characters next to the cursor by typing new characters or deleting existing ones. When you want to move your cursor in an IDE, you click or use the arrow keys.

Vim isn’t like that.

Vim is a modal text editor, meaning it has multiple different “modes” that you can interact with the text on the page.

![](https://miro.medium.com/max/600/1*Br0UgDBYqhZ72n9MKqhdig.png?q=20)



*   In “insert” mode, Vim operates much like your standard IDE, adding and deleting characters.

![](https://miro.medium.com/max/600/1*_ozN1Ap9Sxkua9Qoz4FrQw.png?q=20)



*   But in “normal” mode, where you spend most of your time, you can use the keyboard to move, copy, manipulate, and delete text in groups.

![](https://miro.medium.com/max/600/1*7sbPpl6KptQAObRAvWVRWQ.png?q=20)



*   In “visual” mode, Vim lets you highlight and update entire portions of text.

Switching between modes along with using the keyboard for everything (no mouse) is what makes Vim so fast to use.

# Edit Faster in Groups

In a normal IDE, if you want to delete an entire paragraph you highlight the paragraph with your mouse and then press `delete`.

In Vim, to delete the same paragraph, just type `dap` in normal mode.

While the difference may seem trivial, being able to type all your commands means you can edit much faster. You don’t have to switch between the keyboard and the mouse. No more hunting, clicking, & dragging. Just 3 keystrokes.

These improvements in efficiency add up over time. Using Vim, you can make big changes to a file very quickly.

For example, want to copy a function to a different place in the file?

In an IDE:

1.  Highlight the function with the mouse
2.  `cmd+x` (to cut)
3.  Click to the new location
4.  `cmd+v` (paste)

In Vim:

1.  `dap` (delete & copy the paragraph)
2.  `3}` (move to the new location — could also be `7{` , `4j` , `12k` , `G` — there are lots of ways to move around a file in Vim)
3.  `p` (paste)

That may not seem very impressive. But you can type `dap3}p` before you even finish step #1 of the IDE version.

Composing efficient commands in Vim is what takes your productivity to the next level.

# Repeat Commands for Efficiency

Vim starts to get really powerful once you learn how to repeat commands.

The `.` key in Vim repeats your last command. Learning to use `.` effectively is one of the keys to getting really good and fast at Vim.

Using `.`, I can make the same change to multiple lines or instances of a piece of text. For example:

*   Want to add semicolons to the end of many lines?

1.  `A;<esc>` — add a semicolon to the end of this line
2.  `j` — move to the next line
3.  `.` — add the semicolon again (repeats #1)

*   Want to replace some instances of “foo” with “bar”?

1.  `/foo` — find “foo” in the document
2.  `cw` — change the word
3.  `bar<esc>` — type “bar”
4.  `n` — find the next instance of “foo”
5.  `.` — repeat the command `cwbar<esc>`

That’s pretty cool and makes repetitive changes much faster.

# Macros: Vim is a Programmable Editor

But what if my changes require multiple commands? The `.` command only supports repeating the last command and sometimes I want to repeat a change that required multiple edits.

Enter macros. Simply by pressing `qa` I can begin to record the commands I’m composing. Pressing `q` again stops the recording, and I can now use `@a` to repeat that macro anywhere.

Macros persist across Vim sessions so they’re reusable. In my example above, I picked `a` for where to store my macro, but you can pick any key. `qb` , `qc` , `qz` all work as valid ways to start a macro.

Once you get to the level of using macros in Vim, you start to feel like you have a superpower! You can predictably and repeatedly alter complex text. With macros at your fingertips, working in Vim becomes even more productive.

But be careful! With great power comes great responsibility, and macros are no exception. Just like with coding, recording a macro requires precision to make sure it works predictably in all cases. Be attentive when you’re creating your macros. They could backfire if you create them too loosely.

# Plugins: Extensible + Fast

Vim supports plugins to add more functionality.

It would be impossible for me to list all the possible Vim plugins you might use. But needless to say, plugins can vastly improve your editing experience and give you insights that you’d usually find in an IDE.

Some of the most popular plugins include:

*   [Vim sensible](https://github.com/tpope/vim-sensible) — Basic opinionated settings
*   [lightline](https://github.com/itchyny/lightline.vim) — A more visible status line
*   [supertab](https://github.com/ervandew/supertab) — Better tab completion
*   [Nerd Tree](https://github.com/preservim/nerdtree) — File system explorer
*   [Nerd Commenter](https://github.com/preservim/nerdcommenter) — Support line commenting in various languages
*   [ctrl+p](https://github.com/kien/ctrlp.vim) — Fuzzy file search
*   [Vim gitgutter](https://github.com/airblade/vim-gitgutter) — Shows the git diff beside the line numbers

If default Vim is missing a feature you love, chances are there’s a plugin for it!

# Vimrc: Ultimate Customizability

Beyond plugins, you can tweak anything else you can think of in your `vimrc` so that your editor is tailored to your preferences.

Wanna see my vimrc?

[

## bennett39/config

### Idea storage, resource bookmarks, discussion forum - bennett39/config

github.com



](https://github.com/bennett39/config/blob/master/vim/vimrc)

# Editing Efficiently Matters

So what?

Okay, so maybe Vim is a great text editor. Is it really so good that you should put in the time to learn it?

Ultimately, you’ll have to answer that for yourself.

*   If your job involves SSH-ing into remote machines, learning Vim is almost certainly worth it to have an editor that works everywhere
*   If you mostly develop locally, but don’t like your IDE, you might like the concept of “[Unix as IDE](https://blog.sanctum.geek.nz/series/unix-as-ide/)”
*   If you really like your IDE, there are always ways to add Vim keybindings into your IDE that will take your efficiency to the next level

For me, as a backend developer, I spend a decent amount of time SSH-ing into remote machines. I also happen to like the Unix as IDE philosophy, so I love Vim. (Is it obvious? I wrote a whole post about it.)

For you, think of Vim as a tool in your development toolbelt. Getting good at editing text isn’t a trivial thing for a developer. Over the course of your career, you’ll type hundreds of thousands of lines of code.

Editing should be fast. Thinking about the code and how it works should be the hard part. When it comes time to update the code on your screen, your fingers should be able to edit as fast as you can think.

In the end, that’s what I love about Vim: editing code at the speed of thought.

# About Bennett

I’m a web developer building things with Python and JavaScript.

_Want my best content on web development and becoming a better programmer?_

_I share my favorite advice with my email list — no spam, nothing sales-y, just useful content._

[Join 500 other developers currently in my email series](https://mailchi.mp/2e671faffc04/bennett-medium).

[

## Free Email Series for Medium Readers | Bennett Garner

### Join over 500 other developers currently in my email series. Including: What technical skills you need to learn, and…

mailchi.mp







](https://mailchi.mp/2e671faffc04/bennett-medium)