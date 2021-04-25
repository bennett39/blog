---
title: The Dangers of Copy-Pasting Code - My Debugging Story
date: 2019-04-30T17:45:50.450Z
description: Tutorials and Stack Overflow code are super useful, until you stop understanding what they’re doing.
---



](/the-dangers-of-copy-pasting-code-my-debugging-story-dd994208baab?source=post_page-----dd994208baab--------------------------------)

![](https://miro.medium.com/max/600/4/1*wRubwigdh3X9LmWemQecCA.png)

The workflow of a typical dev — search Stack Overflow for the answer…

Tutorials and Stack Overflow code are super useful, until you stop understanding what they’re doing.

This happened to me this past week, and I spent an agonizing few hours debugging a relatively simple issue in my Django application.

Since I had copy-pasted from some authoritative sounding tutorials online, I didn’t understand what was happening when I ran into an error.

This quick post is my word of warning: If you’re copy-pasting code you don’t understand, then you’re in for a bad time.

# Copying en masse

In my application, I wanted to implement basic user registration and sign-in, but enable customization of the forms and add email activation on registration.

The solution was to use a combination of Django’s built-in auth along with an open-source tool called django-authentication.

I glanced at the documentation for both and then started hacking toward the desired outcome. And I got really far with my own intuition and code samples online.

This applies to nearly any coding problem, not just Django. You’ll likely get most of the way there with tutorials and code samples, but those are dangerous waters if you’re copying too much or code that’s too complex.

# Django NoReverseMatch: Reverse for ‘password\_reset\_confirm’ — my bug from hell

Unfortunately, I was copying from multiple sources, and eventually those sources conflicted to produce a bug:

![](https://miro.medium.com/max/600/4/1*W2v36OFMRJqzr8BoiQ3cjg.png)

Now, the bug in question doesn’t really matter for the sake of this post. It’s a concrete example we can talk about. But this post is about lessons I learned about debugging in general. They can apply to any bug.

By this point, I had copied so many templates, routes, and views from various sources, that I didn’t even know where to begin debugging.

# Google can’t solve everything

Of course, I turned to Google to find an answer. Maybe somebody had experienced this exact issue on Stack Overflow?

Well, yes. A NoReverseMatch is a pretty common issue in Django, it turns out. But none of the code snippets I found applied to my situation.

I spent hours testing out other people’s code, copy-pasting to see if I could get something to work.

All to no avail.

# Go back to basics

As I attempted to debug, I realized I didn’t exactly know what the error even meant or what was producing it.

So, I did something I should have done at the beginning — read the documentation on the error itself.

It turns out a NoReverseMatch error can come from several places in Django. Reading the docs pointed me in the right direction to start looking at the underlying issues.

Also, this [Stack Overflow post about NoReverseMatch](https://stackoverflow.com/a/38390178/10558777) was a big help once I humbled myself enough to accept its advice.

When you’re debugging, strive to understand the error, and then troubleshooting becomes much more clear and systematic.

# Build a tiny test

I originally assumed my issue was with the way Django received parameters or routed the request.

So, I built a [mini route test to check](https://medium.com/@BennettGarner/learning-python-start-small-29d15881f780) how parameters were getting passed through my routes and views.

This was a key lesson for me. Build something tiny and incremental that allows you to rule out one possible cause of the bug.

In so doing, I learned that the problem wasn’t with my URLs at all. In fact, everything seemed to be passing perfectly to the Django server.

# Shifting focus

When debugging, ruling out possible causes is the best way to get to the root of an issue.

Debugging software is a lot like fixing a car.

At first, you don’t know anything about the problem and what’s causing it. You may have intuition, but you’ll only gain real knowledge with a test.

So, you do miniature tests of all the various systems to find where things are breaking down. In a car, you might check that the battery is charged, the fuel lines are clean, the spark plugs aren’t clogged, the starter turns the engine, etc. Gradually eliminate reasons until you find out why the car won’t start.

Similarly, once I found that the URLs were working just fine in my Django app, I could look farther downstream for my issue.

# I found it!

As with most bugs, mine came from a single line.

<form method="post" action="{% url 'password\_reset\_confirm' %}">

This is a mundane and common-enough line. It uses Django templating to set the url for a form in my `password_reset_confirm.html` template.

Except — this form should just submit to its own route via POST.

The `action` part of the form element is a self-reference to the same page. However, Django routed me to that page using some arguments provided in the URL, and those arguments aren’t present when I send a POST request.

If you’re not following along with this description, don’t worry. I’ll just say (in case someone else finds this page because they have the same issue) that the solution was to get rid of the reference to `password_reset_confirm` in the template:

<form method="post">

That’s it! Such a dumb bug took me forever to resolve.

# That bug was an opportunity

I realized that my copy-pasting spree led to a lot of other issues in my code.

Now that I actually understood what was going on in my application flow, I was able to refactor my templates and URL routing.

This is a critical point!

While it was great that I found the bug, it was arguably more important that I learned broader lessons about how my application worked under the hood.

Applying those broader lessons throughout my application was a major win, because it allowed me to get rid of code I wasn’t using and clean up and standardize my usage.

# Lessons learned

Online resources are amazing — and copy-pasting can be incredibly effective when coding. Don’t get me wrong!

But as soon as you lose sight of how your code is working or how much you’ve changed your application, then you’re in trouble.

Do you have a similar debugging war story? Share it in the comments below!

## About Bennett

I’m a software developer in New York City. I do web stuff in Python and JavaScript.

Want to be updated when I release a new post? I have an [email list](http://eepurl.com/goJwcT) you can subscribe to. I have nothing to sell, only free content to share with the community, no time wasters. I’d love to have you there.