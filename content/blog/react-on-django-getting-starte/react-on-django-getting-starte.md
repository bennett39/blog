---
title: React on Django - Getting started
date: 2019-05-15T20:08:14.063Z
description: Here’s my quickstart guide for getting up and running with React on a Django backend.
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff30de8d23504&operation=register&redirect=https%3A%2F%2Fblog.usejournal.com%2Freact-on-django-getting-started-f30de8d23504&source=post_actions_header--------------------------bookmark_preview-----------)

Here’s my quickstart guide for getting up and running with React on a Django backend.

I’m writing this because no tutorial I’ve found does a good job of explaining the big picture for why these technologies fit together the way they do. So, I’ll start with an architecture overview before we dive into the code.

# How Django works to serve files

Django is a web server and framework for delivering web pages to users. When you type a URL into the address bar, it goes through a bunch of routing, but it eventually ends at a server like Django.

Django decides what to do with the URL you typed in. It also can take a look at the headers of your request, including cookies and other tokens, to show you different content depending on who you are. Finally, Django can also handle different request types like GET, POST, PUT, etc. Ultimately, Django sends back some type of response that gets displayed on your browser.

At its simplest, a Django response flow looks like this:

\# urls.py  
from django.urls import path  
from . import viewsurlpatterns = \[  
    path('', views.index),  
    path('hello/', views.hello),  
\]\# views.py  
from django.http import HttpResponsedef index(request):  
    return HttpResponse("You're at the index!")def hello(request):  
    return HttpResponse("Hello there, user.")

In this simple example, when you type _whatever.com/hello_, Django knows to send back the HttpResponse defined in `hello` , because that’s what you’ve specified in your `urlpatterns`. If you just visit the index — whatever.com/ — then you’ll get the HttpResponse defined in `index` .

Forgive me if this is too basic, but understanding that Django is just a way to route requests and decide what response to send is the key to finally demystifying how Django works under the hood.

Django is similar to many other frameworks/servers you may already know about like NodeJS (JavaScript), Rails (Ruby), Flask (Python), Spring Boot (Java), and Laravel (PHP). Indeed, you can use any of these frameworks in combination with React. Django is just nice to work with and the one I prefer.

# How React gets deployed

When you prepare a React application for production deployment, it gets compiled down into a set of static html, css, and javascript files. Django can serve these production files just like it can serve any other web page, so that’s no problem.

In our `views.py` we’ll just need to point at the React files. It’ll end up something like this:

\# views.py  
from django.shortcuts import renderdef index(request):  
    return render(request, "path/to/react/build/index.html")

When Django receives a request for `index`, it will serve up the static production build of React that we’ve created.

# Letting React talk to the database

This is the trickier part. Often, our React application will need to have access and make changes to information in the database. The database models are defined in Django, so we need a way for Django to act as a middleman between React and the database.

To do so, we create interfaces known as serializers. These serializers convert the data from the database into JSON format for React to consume.

For example, when we send a GET request to the serializer for the `User` model in an example application, we might get:

**HTTP 200 OK**  
**Allow:** GET, POST, HEAD, OPTIONS  
**Content-Type:** application/json  
**Vary:** Accept  
  
\[  
    {  
        "url": "[http://127.0.0.1:8000/api/users/2/](http://127.0.0.1:8000/api/users/2/)",  
        "username": "testing",  
        "email": "[test@gmail.com](mailto:bennettgarner+test@gmail.com)",  
        "groups": \[\]  
    },  
    {  
        "url": "[http://127.0.0.1:8000/api/users/1/](http://127.0.0.1:8000/api/users/1/)",  
        "username": "bennett",  
        "email": "[hello@bennettgarner.com](mailto:hello@bennettgarner.com)",  
        "groups": \[\]  
    }  
\]

When React needs to make changes to the database, it creates a JSON-format request and sends it via POST back to the Django serializer.

We could add a new `User` to the above example by sending a POST request in this format:

{  
    "username": "",  
    "email": "",  
    "groups": \[\]  
}

We can do this with any model in our database, not just `User`, even custom models. Serializers are actually pretty easy to implement, as we’ll see.

Serializers are the key to allowing React to talk to the database. The Django REST Framework is an open-source extension of Django that makes this super easy.

# How it fits together

I made a graphic so we’re on the same page about how all this works:

![](https://miro.medium.com/max/600/1*lAMsvtB6afHwTQYCNM1xvw.png?q=20)



How it all fits together.

# To-do list

So, in order to get a React front end working atop a Django back end, what are the steps?

1.  Create a Django project
2.  Add Django REST Framework to the project
3.  Serialize your models so React can consume them
4.  Create a React app that uses/manipulates the data
5.  Build a static version of the React app for Django to serve

# Coming up

We’ll actually get to coding in another tutorial. But I know that if someone had just laid out this roadmap for me at the beginning, I would have been a lot less confused about how everything works together.

## Here are some posts I’ve written about these topics —

You can learn how to build a REST API in Django with this post I wrote:

[

## Build your first REST API with Django REST Framework

### Building a REST API in Django is so super easy. In this tutorial, we’ll walk through the steps to get your first API up…

medium.com



](https://medium.com/swlh/build-your-first-rest-api-with-django-rest-framework-e394e39a482c)

Learn everything you’d want to know about starting React here:

[

## New to React? You Need to Understand These Key Concepts Before Anything Else

### That online tutorial you just finished didn’t teach you the “big picture” of React. Don’t make the same mistakes I…

levelup.gitconnected.com



](https://levelup.gitconnected.com/new-to-react-you-need-to-understand-these-key-concepts-before-anything-else-2247efc1eaac)

# Free Resources

Want my best content on web development and becoming a better programmer?

[Join my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).