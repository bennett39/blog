---
title: How to Create a Django Project in 5 Minutes
date: 2021-03-08T12:47:58.720Z
description: It’s incredibly easy to start developing in Django
---



](/how-to-create-a-django-project-in-5-minutes-914c0de15f63?source=post_page-----914c0de15f63--------------------------------)

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F914c0de15f63&operation=register&redirect=https%3A%2F%2Fbennettgarner.medium.com%2Fhow-to-create-a-django-project-in-5-minutes-914c0de15f63&source=post_actions_header--------------------------bookmark_preview-----------)

![](https://miro.medium.com/max/600/1*TEiKh2mi8aExWUFy_9PHxw.png?q=20)



Getting started with Django is super fast and easy!

Known as “the web framework for perfectionists with deadlines,” Django powers some of the [biggest websites in the world](https://hackernoon.com/10-popular-websites-built-with-django-906cc310aa0a).

I write about Django a lot, and this post is my ultimate resource for getting started with Django from scratch.

# Steps to Get Started with Django

1.  Create a virtual environment (optional, but highly recommended)
2.  Install Django
3.  Create a project

# For the Visual Learners

Here’s a video of me doing these steps for a project:

# 1.1 Virtual Environment

First, consider creating a new virtual environment for your project so you can manage your dependencies separately.

I use [pyenv](https://github.com/pyenv/pyenv) and [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) for my environments:

$ pyenv virtualenv django-tutorialLooking in links: /tmp/tmpjizkdypnRequirement already satisfied: setuptools in /home/bennett/.pyenv/versions/3.6.8/envs/django-tutorial/lib/python3.6/site-packages (40.6.2)Requirement already satisfied: pip in /home/bennett/.pyenv/versions/3.6.8/envs/django-tutorial/lib/python3.6/site-packages (18.1)$ pyenv local django-tutorial

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
Performing system checks...System check identified no issues (0 silenced).You have 17 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.  
Run 'python manage.py migrate' to apply them.May 17, 2019 - 16:09:28  
Django version 2.2.1, using settings 'mysite.settings'  
Starting development server at [http://127.0.0.1:8000/](http://127.0.0.1:8000/)  
Quit the server with CONTROL-C.

Go to [localhost:8000](http://127.0.01:8000) and you should see the Django welcome screen!

![](https://miro.medium.com/max/600/0*DAaFvuY-RF9zDNK5.png?q=20)



Hey! Django works!

# 1.3 Create app

We could build our application with the folder structure the way it is right now. However, best practice is to separate your Django project into separate apps when you build something new.

So, let’s create a new app for our logic:

$ python manage.py startapp myapp  
$ ls  
db.sqlite3  manage.py\*  myapp/  mysite/

# 1.4 Register the myapp app with the mysite project

We need to tell Django to recognize this new app that we just created. The steps we do later won’t work if Django doesn’t know about myapp.

So, we edit `mysite/settings.py` :

INSTALLED\_APPS = \[  
    'myapp.apps.MyappConfig',  
    ... # Leave all the other INSTALLED\_APPS  
\]

# 1.5 Migrate the database

Remember how I said Django allows you to define database models using Python?

Whenever we create or make changes to a model, we need to tell Django to migrate those changes to the database. The Django ORM then writes all the SQL `CREATE TABLE` commands for us.

It turns out that Django comes with a few models already built in. We need to migrate those built in models to our database.

(For those of you thinking, “We didn’t create a database!” You’re right. But Django will create a simple SQLite database for us if we don’t specify differently. And SQLite is awesome!)

So, let’s migrate those initial models:

$ python manage.py migrate  
  
Operations to perform:  
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

# 1.6 Create Super User

One more thing before we move on.

We’re about to create some models. It would be nice if we had access to Django’s pretty admin interface when we want to review the data in our database.

To do so, we’ll need login credentials. So, let’s make ourselves the owners and administrators of this project. THE ALL-POWERFUL SUPERUSER!!!

$ python manage.py createsuperuserUsername (leave blank to use 'bennett'):   
Email address: [hello@bennettgarner.com](mailto:hello@bennettgarner.com)  
Password:   
Password (again):   
Superuser created successfully.

Let’s verify that it works. Start up the Django server:

$ python manage.py runserver

And then navigate to [localhost:8000/admin](http://localhost:8000/admin)

![](https://miro.medium.com/max/600/0*_ivuTqPFzqjG84Di.png?q=20)



Oooo, Django Admin!!! Pretty.

Log in with your superuser credentials, and you should see the admin dashboard:

![](https://miro.medium.com/max/600/0*NXXmCjmK63yL6qV7.png?q=20)



Look at those lovely User and Group models that we just migrated!

# We did it!

It’s really that easy to get started with Django!

You now have the basic file structure to build anything. Add views and models to your app to start creating features.

Django makes it easy to build complex sites that solve problems for people. If you’re new to Django, I’m excited to welcome you into this ecosystem. Here are some cool next steps you can take:

[

## Build your first REST API with Django REST Framework

### Building a REST API in Django is so super easy. In this tutorial, we’ll walk through the steps to get your first API up…

medium.com



](https://medium.com/swlh/build-your-first-rest-api-with-django-rest-framework-e394e39a482c)

[

## Celery Tutorial: A Must-Learn Technology for Python Developers

### Workers to process tasks in the background are essential & powerful tools in any developer’s toolkit.

medium.com



](https://medium.com/swlh/python-developers-celery-is-a-must-learn-technology-heres-how-to-get-started-578f5d63fab3)

[

## React on Django: Getting started

### Here’s my quickstart guide for getting up and running with React on a Django backend.

blog.usejournal.com



](https://blog.usejournal.com/react-on-django-getting-started-f30de8d23504)

Have fun coding, and welcome once again to the world of Django!

## Free Resources

Want my best content on web development and becoming a better programmer?

[Join my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).

[

## Free Email Series for Medium Readers | Bennett Garner

### Join over 500 other developers currently in my email series.

mailchi.mp







](https://mailchi.mp/2e671faffc04/bennett-medium)