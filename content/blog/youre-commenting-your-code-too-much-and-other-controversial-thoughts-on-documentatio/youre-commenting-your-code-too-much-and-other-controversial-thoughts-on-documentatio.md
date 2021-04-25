---
title: You’re commenting your code too much (and other controversial thoughts on documentation)
date: 2018-12-14T17:51:00.871Z
description: The best code comment is often no comment at all… before you get out your pitchforks, let me explain.
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1ee617ed46af&operation=register&redirect=https%3A%2F%2Flevelup.gitconnected.com%2Fyoure-commenting-your-code-too-much-and-other-controversial-thoughts-on-documentation-1ee617ed46af&source=post_actions_header--------------------------bookmark_preview-----------)

![](https://miro.medium.com/max/600/1*x5F0GbT81p44PQOM-sp7hg.png?q=20)



The best code comment is often no comment at all… before you get out your pitchforks, let me explain.

Over-commented code is often more difficult to understand than code without comments.

Little notes back and forth from all the different maintainers of a project can often get cluttered. You spend more time reading the comments than you do the actual code. And often, you could have understood how the program works without the comments altogether.

Of course, there are exceptions! I’m not saying you should \*never\* write a comment.

I am saying, though, that if you can avoid writing a comment then don’t! Let your code stand on its own!

There are a couple reasons why commenting less is a good idea. In this short article I’ll try to explain what I mean and then you can have it out with me in the comments below. ;)

# **When you should comment**

Before I dive into my anti-comments diatribe, I should say there are absolutely times when writing comments is a good idea!

BUT those times are few and far between.

The classic rule of thumb is: [Code tells you how and comments tell you why](https://blog.codinghorror.com/code-tells-you-how-comments-tell-you-why/).

That’s a nice, useful rule. But it doesn’t completely solve the problem of over-commenting. Anyone who has ever spent time around a toddler knows you can ask “Why?” in a never-ending loop.

You can ask why about anything in your code, but that doesn’t mean you should comment everything. So, you need a more strict rule for when to add comments. My philosophy is:

> Only write a comment when you can’t avoid it.

When another developer reading your code would need a helping hand to know why you’ve solved the problem this way, or what dependency made a certain approach necessary, then add a comment.

But comments aren’t the only way to explain your code. Let’s look at what I mean.

# If your comment explains how — delete it!

The biggest rule of eliminating comments is not to do something like this:

\# Nested for loops to iterate through valuesfor i in outer\_loop:  
    for j in inner\_loop:  
       do something

Any good developer knows what a nested for loop is and how it will work. They can work out what’s going to happen when the code runs. No need to add comments to explain it.

Here’s some code I wrote when I was first learning that illustrates my point:

/\* Use modulo and base-10 division/multiplication to reverse an   
 \* int \*/while (original > 0) {  
    reverse \*= 10;   
    int r = original % 10;  
    reverse += r;  
    original /= 10;  
}

The comment is nice, and relatively harmless on its own, but it’s unnecessary. As the code base grows, little comments like that add up. Add enough of them and you can end up doubling the amount of stuff future maintainers of your code have to read.

The best way to delete a comment like this is to use proper naming on your variables and place the code inside a well-named function like `reverse_int()`

# Necessary work vs value-add work

As a developer, the best kind of work you can do is value-add work. It’s code that delivers functionality to the user. You want to maximize your time doing value-add work.

However, there’s other work that’s necessary. Documentation is one part of that. Documenting your code well makes it maintainable and understandable.

Documentation doesn’t add anything for the user, though. So, minimizing your time writing comments and other documentation should be a goal. Don’t skimp on documentation that future maintainers will need, but also don’t waste time writing documentation nobody will need.

# An example

Here’s some code I’ve written recently with a lot less comments, but better function names and self-explanatory code:

unsigned int sum\_squares(int m) {  
    int i;  
    int squares = 0; for (i = 0; i <= m; i++) {  
        squares += i\*i;  
    } return squares;  
}/\* Quadratic, so could reach billions. Use long long.\*/  
unsigned long long square\_sums(int n) {  
    int j;  
    int sum; for (j = 0; j <= n; j++) {  
        sum += j;  
    }  
      
    return sum\*sum;  
}

(Totally open to corrections to my C code. By no means an expert!)

You can see, though, that there’s only one one-line comment in this code. It explains WHY we need more memory to return `square_sums()`.

Otherwise, good naming makes everything clear. When I use these functions elsewhere in the code, it should be evident what the functions do just by their names.

# Maintaining comments

A point that’s rarely mentioned is that comments have to be maintained, too!

When someone changes your code in the future, they also have to change the explanation for that code in the comments. That’s more time spent on making the change, slowing down the process.

If you use variable and function names as your baseline for documentation, then you only need to refactor the code and you’re done. But if you use abstract names for your functions and variables, then you’ll have to spend time changing both the code \*and\* the comments.

Yet another reason to keep comments to a minimum.

# A caveat

Writing a block comment at the top of a program — or a multi-line docstring at the beginning of a function/class/etc for languages that support docstrings — that explains the expected behavior of the program and the general approach the algorithm takes, is often helpful.

These are really helpful, too, when you go to read a piece of code you’ve never seen before. It humanizes the code and gives you a sense for what the person who wrote it was thinking.

# Comment less, but don’t sacrifice readability

Did I miss something? Am I way out of line on how your coding workflow works and how you use comments?

Tell me in the comments below!

If you liked this article, it’d mean a lot if you clapped and followed me. I write new posts on the regular.

# Free Resources

Want my best content on web development and becoming a better programmer?

[Join my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).