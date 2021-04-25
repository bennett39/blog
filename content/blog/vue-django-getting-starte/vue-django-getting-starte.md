---
title: Vue + Django - Getting Started
date: 2020-07-28T11:19:12.888Z
description: Adding Vue to your Django app is pretty easy!
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F88d3f4c2ba62&operation=register&redirect=https%3A%2F%2Flevelup.gitconnected.com%2Fvue-django-getting-started-88d3f4c2ba62&source=post_actions_header--------------------------bookmark_preview-----------)

![](https://miro.medium.com/max/600/1*0Rk_6tH6IeSu8RjGEMRxYg.png?q=20)



These two technologies work great together.

Adding Vue to an existing Django app is super easy, and a great way to enable more complex front-end features. In this post, we’ll see how to do it the easy way.

# Why Add Vue?

We’ll get to the code in a minute, but first it’s worth asking — “Why add Vue to your Django project?”

If you just want to add some effects or dynamically render something on the page, vanilla JavaScript is probably a better bet. Just add a `script` tag to your page and try to do the manipulations from there.

However, if you have a whole section of the page that needs to share data and re-render often when the user interacts with it, then you probably have a use case for a JavaScript framework, and Vue is a great choice!

Vue allows you to:

*   Dynamically render text onto the page (like Django templates, but interactive!)
*   Update the CSS, classes, & attributes of various elements on the page
*   Define “components,” whole sections of a web page, that render dynamically and are reusable
*   Build/restore the state of components based on properties (“props”) that you pass from the server-side or fetch via a [REST API](https://medium.com/swlh/build-your-first-rest-api-with-django-rest-framework-e394e39a482c)

[

## Build your first REST API with Django REST Framework

### Building a REST API in Django is so super easy. In this tutorial, we’ll walk through the steps to get your first API up…

medium.com



](https://medium.com/swlh/build-your-first-rest-api-with-django-rest-framework-e394e39a482c)

Vue is really cool and fairly easy to learn. It’s similar to [learning React](/new-to-react-you-need-to-understand-these-key-concepts-before-anything-else-2247efc1eaac), but with a less steep learning curve and easier to set up.

[

## New to React? You Need to Understand These Key Concepts Before Anything Else

### That online tutorial you just finished didn’t teach you the “big picture” of React. Don’t make the same mistakes I…

levelup.gitconnected.com



](/new-to-react-you-need-to-understand-these-key-concepts-before-anything-else-2247efc1eaac)

This isn’t a Vue tutorial, so to learn Vue you’ll want to visit the [official guide](https://vuejs.org/v2/guide/) (which is really well done and easy to follow).

[

## Introduction - Vue.js

### Vue.js - The Progressive JavaScript Framework

vuejs.org



](https://vuejs.org/v2/guide/)

Let’s see how to add Vue to Django!

# Create a New Django App Before We Start

Before we can add Vue to Django, we need a Django project! I’ll use this project for all the examples that follow.

If you have an existing Django app, you don’t need to copy this. But you should read it just so you know what my app structure looks like.

Visual learner? No worries — I made a video of these steps:

# 1.1 Virtual Environment

I use [pyenv](https://github.com/pyenv/pyenv) and [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) for my environments:

$ pyenv virtualenv vue-appLooking in links: /var/folders/tz/tjybwp513hd5zvdh166kbwnw0000gn/T/tmp2ler\_knwRequirement already satisfied: setuptools in /Users/bennettgarner/.pyenv/versions/3.8.0/envs/vue-app/lib/python3.8/site-packages (41.2.0)Requirement already satisfied: pip in /Users/bennettgarner/.pyenv/versions/3.8.0/envs/vue-app/lib/python3.8/site-packages (19.2.3)$ pyenv local vue-app

# 1.2 Install Django

Now, we can install Django:

$ pip install django

Next, let’s start a new Django project:

$ django-admin startproject mysite

If we look at the directory now, we’ll see that Django created a new folder for us:

$ ls  
mysite/

And if we look inside that folder, there’s everything we need to run a Django site:

$ cd mysite/  
$ ls  
manage.py\*  mysite/

Let’s make sure it works. Test run the Django server:

$ python manage.py runserverWatching for file changes with StatReloader  
Performing system checks...System check identified no issues (0 silenced).You have 17 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions. Run 'python manage.py migrate' to apply them.July 24, 2020 - 12:14:09  
Django version 3.0.8, using settings 'mysite.settings'  
Starting development server at http://127.0.0.1:8000/  
Quit the server with CONTROL-C.

Go to http://127.0.0.1:8000 and you should see the Django welcome screen!

![](https://miro.medium.com/max/600/1*t7UmkbtuXTh8Hq1w4Yz2Ew.png?q=20)



# 1.3 Create the Vue app

We could build our application with the folder structure the way it is right now. However, best practice is to separate your Django project into separate apps when you build something new.

So, let’s create a new app for our Vue application (if you already have an existing project, this will be the app you want to use Vue in).

$ python manage.py startapp vue\_app  
$ ls  
db.sqlite3 manage.py  mysite/     vue\_app/

# 1.4 Register the vue\_app app with the mysite project

We need to tell Django to recognize this new app that we just created. The steps we do later won’t work if Django doesn’t know about myapi.

So, we edit `mysite/settings.py` :

INSTALLED\_APPS = \[  
    'vue\_app.apps.VueAppConfig',  
    ... # Leave all the other INSTALLED\_APPS  
\]

# 1.5 Migrate the database

We might need the User and session models that come with Django, so let’s have Django add them to the database.

(For those of you thinking, “We didn’t create a database!” You’re right. But Django will create a simple SQLite database for us if we don’t specify differently. And SQLite is awesome!)

So, let’s migrate those initial models:

$ python manage.py migrateOperations to perform:  
  Apply all migrations: admin, auth, contenttypes, sessions  
Running migrations:  
  Applying contenttypes.0001\_initial... OK  
  Applying auth.0001\_initial... OK  
  Applying admin.0001\_initial... OK  
  Applying admin.0002\_logentry\_remove\_auto\_add... OK  
  Applying admin.0003\_logentry\_add\_action\_flag\_choices... OK  
  Applying contenttypes.0002\_remove\_content\_type\_name... OK  
  Applying auth.0002\_alter\_permission\_name\_max\_length... OK  
  Applying auth.0003\_alter\_user\_email\_max\_length... OK  
  Applying auth.0004\_alter\_user\_username\_opts... OK  
  Applying auth.0005\_alter\_user\_last\_login\_null... OK  
  Applying auth.0006\_require\_contenttypes\_0002... OK  
  Applying auth.0007\_alter\_validators\_add\_error\_messages... OK  
  Applying auth.0008\_alter\_user\_username\_max\_length... OK  
  Applying auth.0009\_alter\_user\_last\_name\_max\_length... OK  
  Applying auth.0010\_alter\_group\_name\_max\_length... OK  
  Applying auth.0011\_update\_proxy\_permissions... OK  
  Applying sessions.0001\_initial... OK

We’ve got Django up and running! Now, let’s add Vue to our project!

# The Easy Way to Add Vue: Include Vue in Your Templates

If you just want to use Vue on a single page in your application, the easiest way is to include it inline in the template.

## Set Up a Template

In `vue_app/` create a `templates/` folder. Then, create `vue_app/templates/vue_app/` . This type of namespacing is common in Django projects and helps to make sure the templating engine serves the right files.

Now, create `test.html` in `vue_app/templates/vue_app/` :

<!-- vue\_app/templates/vue\_app/test.html --><html>  
  <head>  
    <title>Testing Vue</title>  
  </head>  
  <body>  
    <h1>Testing, Testing</h1>  
  </body>  
</html>

As you can see, we don’t have Vue in the template at all yet, but we’ll add it soon.

First, let’s define a URL and a view to render the template.

Open `vue_app/views.py` (it should already exist):

\# vue\_app/views.pyfrom django.shortcuts import renderdef test\_vue(request):  
    return render(request, 'vue\_app/test.html')

Next, we need a URL to point to that view. Go back to the folder where `manage.py` is (I called it `mysite/` ). Then, open `mysite/mysite/urls.py`. We’ll just define this URL globally, since it’s the only one in our test app for now.

\# mysite/mysite/urls.pyfrom django.contrib import admin  
from django.urls import pathfrom vue\_app import views as vue\_views  # This line is newurlpatterns = \[  
    path('admin/', admin.site.urls),  
    path('test', vue\_views.test\_vue),  # This line is new  
\]

If you `python manage.py runserver` now and go to [http://127.0.0.1:8000/test](http://127.0.0.1:8000/test) you should see our test page.

![](https://miro.medium.com/max/600/1*chmM6p0QIQvk_Ugz2rtkoQ.png?q=20)



No Vue yet…

## Finally, we can add Vue!

The easiest way to add Vue to our template is a straight import. If you check out the [official Vue guide](https://vuejs.org/v2/guide/), they have links to a CDN where you can quickly import Vue into your web page:

<!-- development version, includes helpful console warnings -->  
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>OR<!-- production version, optimized for size and speed -->  
<script src="https://cdn.jsdelivr.net/npm/vue"></script>

Setting up your Django app to use different settings in dev vs. production is outside the scope of this article. There are tons of reasons why you’ll want to have different settings locally, most importantly `DEBUG = True` in `mysite/settings.py` . It’s worth learning how to do that.

Once you do, you can import the script something like this:

<html>  
  <head>  
    {% if env\_name == 'production' %}  
      <script src="[https://cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue)"></script>  
    {% else %}  
      <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>  
    {% endif %}  
    <title>Testing Vue</title>  
  </head>  
  <body>  
    <h1>Testing, Testing</h1>  
  </body>  
</html>

That’s it! You’ve added Vue to your template. Let’s try it out!

# Writing your first bit of Vue on Django

Now, we’re ready to write some Vue code!

<html>  
  <head>  
    <!-- development version, includes helpful console warnings -->  
    <script src="[https://cdn.jsdelivr.net/npm/vue/dist/vue.js](https://cdn.jsdelivr.net/npm/vue/dist/vue.js)"></script>  
    <title>Testing Vue</title>  
  </head> <body>  
    <div id="vue-app">  
      <h1>\[\[ myTitle \]\]</h1>  
    </div> <script type="text/javascript">  
      let app = new Vue({  
        el: "#vue-app",  
        delimiters: \['\[\[', '\]\]'\],  
        data: {  
          myTitle: 'Hello Vue!',  
        },  
      });  
    </script>  
  </body>  
</html>

![](https://miro.medium.com/max/600/1*3wTlLQOj_6rQG4xg15YHmg.png?q=20)



It’s working!

Couple things to notice here:

1.  Like all Vue apps, we define a `<div>` where Vue can operate. Think of it like a “window” into our web page where Vue can render things
2.  Since it’s Vue’s window, Vue controls everything inside that `<div>` . If you tried to add script tags or produce side effects inside that window, Vue would strip that out so that it can run reliably. As such, you should make your Vue window as small as possible and compose the page in Vue gradually.
3.  We set custom delimiters on this Vue app. By default Vue uses `{{ }}` for variables in their apps, but that collides with Django templating syntax. So, we changed the delimiters in Vue.
4.  The good news is, once you get used to Vue, you won’t need those delimiters very often. You can often accomplish the same thing without the delimiters:

// Same example as above, but w/ more advanced Vue & no delimiters<div id="vue-app">  
  <span v-html="myTitle"></span>  
</div><script type="text/javascript">  
  let app = new Vue({  
    el: "#vue-app",  
    data: {  
      myTitle: '<h1>Hello Vue!</h1>',  
    },  
  });  
</script>

Importing from a CDN is by far the easiest way to add Vue to Django, and you’ll actually be able to get pretty far with it before you need to adopt a different strategy…

# Cleaning up — static assets

If you continue down this path and try to build a Vue app, you’re going to end up with a lot of huge `<script>` tags in your HTML as you define components and logic.

It’s better to collect and [serve these scripts as static assets](https://docs.djangoproject.com/en/3.0/howto/static-files/) instead of including them directly in the templates themselves. Then, you can also share components across web pages.

Setting up [static assets for Django](https://docs.djangoproject.com/en/3.0/howto/static-files/) is the subject of entire tutorials, so I’ll leave that out here. Once you have it set up, though, you should be able to keep all your Vue files in a folder like `static/src/` .

However, at a certain point you’ll reach a breaking point where those files become pretty big and they can’t easily import one another.

Eventually, you’ll want to make the leap to a build system for Vue.

# Adding Webpack to Use Vue CLI & Vue Templates

If you want to use all the features of the Vue CLI and .vue files, then this approach won’t work. You’ll need to compile assets with Webpack.

That involves a bit more configuration, but don’t worry! It’s not too hard to do. I’ve written another post with all the details:

[

## Vue + Django: Using .vue Files and the Vue CLI

### Build a full-stack website running on Django

betterprogramming.pub



](https://betterprogramming.pub/vue-django-using-vue-files-and-the-vue-cli-d6dd8c9145eb)

# Vue on Django — We did it!

That wasn’t so hard! Now you can focus on building cool things with Vue on top of an API driven by Django.

Tons of cool modern web apps you use every day use this combination of technologies, so welcome to developing with these two great tools. You’ll be able to build almost anything you can imagine between a Python backend and a JavaScript frontend.

Questions? Comments? Concerns with my approach? Happy to hear them out in the comments below or at [vue@bennettgarner.com](mailto:vue@bennettgarner.com)

# Free Resources

Want my best content on web development and becoming a better programmer?

[Join my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).

[

## Free Email Series for Medium Readers | Bennett Garner

### Join over 500 other developers currently in my email series.

mailchi.mp







](https://mailchi.mp/2e671faffc04/bennett-medium)