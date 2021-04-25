---
title: Tic-tac-toe series #5 - Adding version control with Git
date: 2019-05-01T16:33:44.286Z
description: We’re building a tic-tac-toe game to illustrate the value of incremental learning when it comes to software development.
---



](/tic-tac-toe-series-5-adding-version-control-with-git-5c0607995fb3?source=post_page-----5c0607995fb3--------------------------------)

We’re building a tic-tac-toe game to illustrate the value of incremental learning when it comes to software development.

([See all the posts in this series](https://medium.com/@BennettGarner/the-tic-tac-toe-series-master-list-a4a908f015f9).)

So far, we’ve made a lot of progress! We now have a working game that you can play on the command line!

As any developer knows, there’s always more you can do to improve your application. Today, we’ll learn some common workflow stuff that won’t change the application, but it will dramatically help us develop it.

# Where we are so far

To date, our game lives only locally and can accept inputs from two players.

When one player wins or there are no free spaces left on the board, the game prints a message and quits:

![](https://miro.medium.com/max/600/6/1*pEQQ-SIUaNqGMV8IUR_Grg.png)

In the next phase of development, we’re going to add more features like an AI to play against.

While the application seems simple now, it could get quite complex very quickly.

# We’re gonna make mistakes

As things get more complicated, the likelihood that we’ll make mistakes increases. There’s a good chance we’re going to code ourselves into a corner at some point or make poor decisions along the way.

Since we know we’ll make mistakes, it would be nice to have a way to easily undo those errors and try something new.

Better yet, it would be awesome to have multiple copies of our application where we could work on new features on one copy while the main copy stays playable and usable.

# Copies = a bad idea

We could easily accomplish this goal of multiple versions of our application by actually creating copies of the file on our computer.

You’ve probably seen something like this if you’ve worked in a corporate environment for any amount of time:

![](https://miro.medium.com/max/600/1*NVxUvbsmhvf0JZfZoiDEVQ.jpeg?q=20)



If we keep appending suffixes to the file name, then we end up with multiple versions, sure. But that’s hardly an optimal solution.

It ends up quite cluttered and unclear which version we actually want to work on. What’s the most recent update? Which version is stable and which is under development?

# Enter Git

Git is a tool for version control of your projects. It’s one of the most important tools in software development today.

![](https://miro.medium.com/max/600/1*N6oVTvcFjYdSLgkR3CZgvg.png?q=20)



First developed by Linus Torvalds (yes, the same guy who created Linux) in 2005, Git allows you to maintain multiple versions of the same files on your computer without needing to rename the files.

Instead, Git maintains a history of the revisions you’ve made to your files, allowing you to roll back changes in time.

Git also enables the creation of “branches” where you can work on a file under development in parallel to the stable version of the file. While you do, Git maintains separate revision histories for the various branches.

You can then switch between branches depending on your needs.

*   Need the stable, authoritative version of your application? Switch to the “master” branch.
*   Want to work on a new feature that might break the application? Switch to a feature development branch.
*   Ready to incorporate your new feature into the stable, authoritative version of the app? Merge the revisions from “development” into “master.”

It’s that simple. Let’s set it up!

# Mini-step #1: Install git

This tic-tac-toe game is still an exercise in incremental learning and small experiments. So, I won’t tell you how to use git. I’ll just point you in the right direction on some experiments you might try with git.

The first mini-experiment is intalling git on your local computer.

Lucky for me, this isn’t a tutorial, so I won’t explain the intricacies of installing git on Windows, Mac, and Linux.

Do a little Googling to find out how to add git to your computer.

**Try it now!**

…

…

…

Did you install it?

If you installed it correctly, you should be able to do something like:

$ git --version  
git version 2.7.4

# Mini-step #2: Initialize a git repository

Now, git doesn’t start tracking all the files on our computer automatically. You wouldn’t want it to!

Instead, we need to tell git which directory it should track for us. To do that, we need to initialize a git repository (a version-controlled directory) in the same directory as our tic-tac-toe game.

**Try to figure it out on your own before looking at my answer!**

…

…

…

(Hint: Did you know you can type `man git` to see git’s documentation? That might help…)

…

…

…

To start a new repository:

$ git init  
Initialized empty Git repository in /home/bennett/Repos/testrepo/.git/

# Mini-step #3: Track some files

Even though we’ve initialized a git repository, git won’t start tracking files in that directory until we explicitly tell it to.

Telling git about a file involves two steps:

1.  Add the file to git’s tracked files
2.  Commit any changes you’ve made (or make an initial commit of new files)

In our case, we need to tell git that `ttt.py` exists and we want git to track changes to it. Then, we need to commit the initial version of the file to git.

When we make a commit, git requires us to add a little message about the changes we’ve made. For now, it’s okay to just say “Initial commit.” Writing good commit messages is a subject for a [whole different post](https://chris.beams.io/posts/git-commit/).

**Try it on your own: Figure out how to add a file to git and how to commit changes to that file. Use Google and** `**man git**` **to help!**

…

…

…

$ git add ttt.py$ git status  
On branch masterInitial commitChanges to be committed:  
  (use "git rm --cached <file>..." to unstage)new file:   ttt.py$ git commit -m "Initial commit"  
\[master (root-commit) 1aae357\] Initial commit  
 1 file changed, 0 insertions(+), 0 deletions(-)  
 create mode 100644 ttt.py

Every time we make a change that we want git to remember, we’ll need to `git add` and `git commit` . This is a key pattern of software development.

If this is your first time using git, welcome to version control! You’re now tracking a file.

# Mini-step #4: Experiment with making changes

Let’s get some practice using git. Remember, the best way to learn anything new is incrementally. So, let’s just do some tiny experiments.

Try this:

1.  Create a new file `git_test.txt`
2.  Add and commit the file to the git repo
3.  Open `git_test.txt` and type a test message, then save the file
4.  Add and commit the changes you just made to git
5.  See if you can print a log of your commits to the repository thus far

**Go test it out! Mini-experiments like this are the best way to really understand and speak git’s language.**

…

…

…

$ touch git\_test.txt$ git add git\_test.txt$ git status  
On branch master  
Changes to be committed:  
  (use "git reset HEAD <file>..." to unstage)new file:   git\_test.txt$ git commit -m "Add git\_test"  
\[master 148b921\] Add git\_test  
 1 file changed, 0 insertions(+), 0 deletions(-)  
 create mode 100644 git\_test.txt$ echo "Oh, what a beautiful morning!" > git\_test.txt $ git status  
On branch master  
Changes not staged for commit:  
  (use "git add <file>..." to update what will be committed)  
  (use "git checkout -- <file>..." to discard changes in working directory)modified:   git\_test.txtno changes added to commit (use "git add" and/or "git commit -a")$ git add git\_test.txt $ git status  
On branch master  
Changes to be committed:  
  (use "git reset HEAD <file>..." to unstage)modified:   git\_test.txtbennett:testrepo$ git commit -m "Add message to git\_test"  
\[master 8857a09\] Add message to git\_test  
 1 file changed, 1 insertion(+)$ git log  
commit 8857a09c28848300ceaf4179409c97d3c826d8f5  
Author: Bennett Garner <[34491412+bennett39@users.noreply.github.com](mailto:34491412+bennett39@users.noreply.github.com)\>  
Date:   Wed May 1 12:01:20 2019 -0400Add message to git\_testcommit 148b92138a5dc7e800a3d2a3e01aa4757e1fdb18  
Author: Bennett Garner <[34491412+bennett39@users.noreply.github.com](mailto:34491412+bennett39@users.noreply.github.com)\>  
Date:   Wed May 1 12:00:17 2019 -0400Add git\_testcommit 1aae3573ff6b568af563178f50b79f03a38322de  
Author: Bennett Garner <[34491412+bennett39@users.noreply.github.com](mailto:34491412+bennett39@users.noreply.github.com)\>  
Date:   Wed May 1 11:52:00 2019 -0400Initial commit

# Mini-step #5: Undoing changes

Let’s imagine you want to undo the changes you just made. Maybe you realized that creating `git_test.txt` was a mistake.

Can you figure out how to rollback changes in git?

Try:

1.  Go back to when `git_test.txt` was an empty file (rollback the message)
2.  Go back to before you created `git_test.txt` (back to the initial commit)

…

…

…

$ ls  
git\_test.txt  ttt.py$ git log --oneline  
8857a09 Add message to git\_test  
148b921 Add git\_test  
1aae357 Initial commit$ git reset --hard 148b921  
HEAD is now at 148b921 Add git\_test$ git log --oneline  
148b921 Add git\_test  
1aae357 Initial commit$ git reset --hard 1aae357  
HEAD is now at 1aae357 Initial commit$ git log --oneline  
1aae357 Initial commit$ ls  
ttt.py

By the end, you can see that `git_test.txt` is actually gone from the folder on my machine! Git knew to remove the file when I rolled back to the initial commit.

# Mini-step #6: Create branches

When you first initialize a repository, you’re automatically on the master version of your repository.

$ git branch  
\* master

However, git makes it easy to create as many different versions of your repository as you want. Each version can exist for building new features, experimenting, different configurations, etc.

See if you can figure out how to:

1.  Create a new branch called `development`
2.  Switch to the `development` branch
3.  Add and commit a file called `branch_test.txt` to the `development` branch
4.  Switch back to the `master` branch — is the new file there?
5.  Switch back to `development` — is the new file there?

**Play around with these steps and create even more branches to switch between. Now, you’re beginning to see the magic of git.**

…

…

…

(Hint: you can use `man` here as well! Try `man git branch` for specific help on branches.)

…

…

…

$ git branch  
\* master$ git branch development$ git branch  
  development  
\* master$ git checkout development  
Switched to branch 'development'$ git branch  
\* development  
  master$ touch branch\_test.txt$ git add branch\_test.txt $ git commit -m "Add branch\_test"  
\[development e2e4e4c\] Add branch\_test  
 1 file changed, 0 insertions(+), 0 deletions(-)  
 create mode 100644 branch\_test.txt$ git status  
On branch development  
nothing to commit, working directory clean$ ls  
branch\_test.txt  ttt.py$ git checkout master  
Switched to branch 'master'$ git status  
On branch master  
nothing to commit, working directory clean$ ls  
ttt.py$ git checkout development  
Switched to branch 'development'$ ls  
branch\_test.txt  ttt.py

# Mini-step #7: Merge branches

When you’ve made a change on a branch, and then you want to incorporate that change into the master version, you’ll need to conduct a merge.

Try:

1.  Switch to the `master` branch
2.  Merge in the changes from the `development` branch
3.  Delete the `development` branch — we don’t need it anymore, now that it’s merged

…

…

…

$ git branch  
\* development  
  master$ git checkout master  
Switched to branch 'master'$ git merge development  
Updating 1aae357..e2e4e4c  
Fast-forward  
 branch\_test.txt | 0  
 1 file changed, 0 insertions(+), 0 deletions(-)  
 create mode 100644 branch\_test.txt$ ls  
branch\_test.txt  ttt.py$ git branch -d development  
Deleted branch development (was e2e4e4c).  
  
$ git branch  
\* master

# Wrapping up

This post is getting long, so I’ll wrap up for now.

Needless to say, there are entire articles on how git works and the things you can do with it!

Most development teams at major companies use git to work on dozens of new features at a time, merging changes into the master branch as each feature is complete and fully tested.

Learning git is a critical skill for any modern software developer. It’s also incredibly useful even for your personal projects.

We’ll be using git to track all our changes to the tic-tac-toe game. In fact, I’ve already been using git without explicitly telling you. That’s how I’m able to track and share [different versions of the code each day.](https://github.com/bennett39/ttt-medium/branches)

See you next time!

## About Bennett

I’m a software developer in New York City. I do web stuff in Python and JavaScript.

Want to be updated when I release a new post? I have an [email list](http://eepurl.com/goJwcT) you can subscribe to. I have nothing to sell, only free content to share with the community, no time wasters. I’d love to have you there.

Check out the complete list of [all posts in this tic-tac-toe series](https://medium.com/@BennettGarner/the-tic-tac-toe-series-master-list-a4a908f015f9).