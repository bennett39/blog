---
title: Deploying Django to Heroku - Procfile, STATIC_ROOT, & Other Pitfalls
date: 2019-01-24T22:58:50.028Z
description: Heroku’s help wasn’t helpful enough, so I wrote this guide to get you (and me) though your first deployment.
---



](/deploying-django-to-heroku-procfile-static-root-other-pitfalls-e7ab8b2ba33b?source=post_page-----e7ab8b2ba33b--------------------------------)

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe7ab8b2ba33b&operation=register&redirect=https%3A%2F%2Fbennettgarner.medium.com%2Fdeploying-django-to-heroku-procfile-static-root-other-pitfalls-e7ab8b2ba33b&source=post_actions_header--------------------------bookmark_preview-----------)

Heroku’s help wasn’t helpful enough, so I wrote this guide to get you (and me) though your first deployment.

![](https://miro.medium.com/max/600/1*w-iGJIS3DjfC6ZPkux5MQQ.png?q=20)



_Update 2/11/20: It’s been a while since I wrote this post, and there’s now a better way:_ [_https://devcenter.heroku.com/articles/heroku-postgresql#connecting-with-django_](https://devcenter.heroku.com/articles/heroku-postgresql#connecting-with-django)

_That said, this post might still be useful to some of you who are new to Heroku!_

Heroku is a great option for deploying your apps. It’s especially attractive if you want to use a relational database, because PostgreSQL is free on Heroku if you’re a hobby developer. On other cloud providers like Google Cloud or AWS, you have to pay for your databases.

That said, it took me a little while to get my barebones Django application running on Heroku. It was especially difficult because Heroku’s docs aren’t comprehensive. I had to stitch this workflow together with help from Stack Overflow.

Here’s what you need to do to go from zero to deployed on Heroku with a Django application from the command line.

> Are you an experienced dev just looking for some source code? I’m happy to oblige: [https://github.com/bennett39/django-heroku-example/tree/barebones](https://github.com/bennett39/django-heroku-example/tree/barebones)

# Prereqs

I’ll assume you’re starting with an empty directory and nothing built yet. I’ll also assume you’re on Linux.

Mac people, you’ll have to do some translating of these commands, but the workflow is the same. Windows people, I’m sorry.

Here’s what you need to have installed:

## Pyenv (not required but makes this whole thing easier)

curl https://pyenv.run | bashpyenv update

## Pyenv virtualenv (plugin for Pyenv)

git clone [https://github.com/pyenv/pyenv-virtualenv.git](https://github.com/pyenv/pyenv-virtualenv.git) $(pyenv root)/plugins/pyenv-virtualenvecho 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash\_profileexec "$SHELL"

## Python 3.6.8 (Heroku’s default Python runtime)

pyenv install 3.6.8

## Pip

sudo apt-get pippip install --upgrade pip

We’ll install other stuff as well, but these will allow us to create a virtual environment that’s clean and compartmentalized. That makes managing the dependencies Heroku sees easier.

# Create Your Virtual Environment

Find a good place to save your project locally. Feel free to use whatever name you want, but for this example I’ll use the name `myproject`

mkdir myprojectcd myproject

We’re going to make this `myproject/` directory a special virtual environment with its own Python source and clean dependency tree.

In `myproject/` run:

pyenv virtualenv 3.6.8 myproject

The command above creates a new virtual environment, using Python 3.6.8.

The name of that new virtualenv is “myproject” — the same as the name of the directory.

Now, we need to set the `myproject/` directory to use the myproject virtualenv:

pyenv local myproject

To confirm your setup, run the following in the `myproject/` directory:

$ python --version  
Python 3.6.8$ pyenv version  
myproject (set by /home/user/path/to/file/.python-version)

# Add Django

Now that we have a clean virtual environment, we can install the modules we’ll need to build our app.

In this example, we’ll launch a Django application, so let’s get Django:

pip install django

Verify the version of Django you have is 2.1 or later:

$ python -m django --version  
2.1.5

If so, we’re ready to create our Django project locally!

Using Django’s typical naming scheme, I’m going to call our project `mysite` . You can use whatever name you want.

django-admin startproject mysite

After running that command, you’ll see a new folder named `mysite/` in your current directory.

Navigate into it and check out its contents:

$ cd mysite/ && ls -a  
./  ../  db.sqlite3  manage.py\*  mysite/

If that’s what you see, then you’ve successfully created your first Django project. To test it out, try:

$ python manage.py runserverPerforming system checks...System check identified no issues (0 silenced).You have 15 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.  
Run 'python manage.py migrate' to apply them.January 24, 2019 - 20:15:54  
Django version 2.1.5, using settings 'mysite.settings'  
Starting development server at [http://127.0.0.1:8000/](http://127.0.0.1:8000/)  
Quit the server with CONTROL-C.

Don’t worry about the “15 unapplied migrations” for right now. You can fix those later on once you have the database running on Heroku.

For now, follow the link to [http://127.0.0.1:8000/](http://127.0.0.1:8000/) (aka [http://localhost:8000](http://localhost:8000))

You’ll be greeted by the friendly Django startup screen:

![](https://miro.medium.com/max/600/1*XKU0Hwie50LGEde-cjNl-g.png?q=20)



This isn’t a tutorial about how to build an application with Django, so we won’t do anything more with Django. That startup screen is enough.

> **Now, our goal is to get that same startup screen to appear at ProjectName.heroku.com**

# Setting Up Heroku

There are a few steps before you can start moving the Django app to Heroku.

## 1\. Sign Up on Heroku

You’ll need a Heroku account to create a Heroku app, so visit:

[

## Heroku | Sign up

### Sign up for a free Heroku developer account and get started building your apps on Heroku.

signup.heroku.com







](https://signup.heroku.com/)

## 2\. Get Git, if you don’t have it

sudo apt-get git

First time using Git? Google some getting started guides and [follow these instructions](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup) to get set up.

Learning Git is outside the scope of this guide.

## 3\. Get the Heroku Command Line Interface

curl https://cli-assets.heroku.com/install.sh | sh

The Heroku CLI integrates with Git to allow you to track and push your local changes to your application on Heroku.

## 4\. Initialize a Heroku Application from the Command Line

Navigate to `myproject/mysite/` . Verify you’re in the right place:

$ ls  
db.sqlite3  manage.py\*  mysite/

You should be in the same directory as `manage.py` .

Now, create a new Git repository for this project:

git init

And then create a Heroku application linked to the Git repository:

$ heroku create  
Creating app... done, ⬢ nameless-tundra-24362  
https://nameless-tundra-24362.herokuapp.com/ | https://git.heroku.com/nameless-tundra-24362.git

Heroku will give your application a creative name like `nameless-tundra-24632`. If you want to specify a name, you can with `heroku create ProjectNameHere` , but it needs to be a unique name.

# Getting Ready to Deploy

Before we can push our Django app to Heroku, Heroku needs a little more information on how to run the app.

Specifically, Heroku needs 6 changes to our out-of-the-box Django app:

1.  Gunicorn
2.  `Procfile`
3.  django-heroku
4.  `STATIC_ROOT` / `PROJECT_ROOT` in `settings.py`
5.  `requirements.txt`
6.  `runtime.txt`

## 1\. Gunicorn

Gunicorn is an open-source web server for Python. It allows Heroku to deploy our application across various “workers.”

How Gunicorn works is a topic for another post, but needless to say Heroku needs it so we’ll give it to them.

In your project’s directory, run:

pip install gunicorn

This adds Gunicorn to your virtual environment so your Django project can use it.

## 2\. Procfile

A Procfile is something unique to Heroku. It’s a file in your project’s root directory that tells Heroku how your app should start and run.

In our case, we’ll use our Procfile to tell Heroku to run a Gunicorn server.

The good news is Django comes with out-of-the-box support for Gunicorn servers, because they both follow the conventions of [WSGI](https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface).

In the Procfile, we’ll tell Heroku to start a Gunicorn server and then point that server to our Django project’s default WSGI interface.

In `myproject/mysite/` , run the following command to create the Procfile:

echo "web: gunicorn mysite.wsgi --log-file -" > Procfile

If you decided to name your Django project something other than `mysite` , then you’ll need to replace `mysite.wsgi` with `your_project_name.wsgi`.

## 3\. Django-heroku

Django is a pretty popular framework, so Heroku has created a module called [django-heroku](https://github.com/heroku/django-heroku) that helps with settings, testing, and logging automatically.

To install it, make sure you’re in `myproject/mysite/` then:

pip install django-heroku

With the module successfully installed, we can now add it to our Django project’s `settings.py` . Open `myproject/mysite/mysite/settings.py` .

At the top of `settings.py` import the module. Then, at the very bottom, call it:

import django\_heroku...\# All of your settings here...django\_heroku.settings(locals())

Save `settings.py` , but don’t close it. We have more changes to make.

## 4\. STATIC\_ROOT & PROJECT\_ROOT

Search your `settings.py` for an environment variable called `STATIC_URL`.

Next to that setting, we’ll also need to give Heroku more context about where static files (images, scripts, etc) are stored.

We’ll add two new variables, `STATIC_ROOT` and `PROJECT_ROOT` , in addition to `STATIC_URL`. That section of `settings.py` should now look like this:

PROJECT\_ROOT = os.path.dirname(os.path.abspath(\_\_file\_\_))  
STATIC\_URL = '/static/'  
STATIC\_ROOT = os.path.join(PROJECT\_ROOT, 'static')

## 5\. requirements.txt

We’re almost ready to deploy our Django project!

Before we can, we need to tell Heroku about all the packages we’ve installed:

*   Django
*   django-heroku
*   Gunicorn

In addition, those packages have installed their own dependencies. You may not have realized it, but our little application has now installed 7 different modules under the hood that allow it to run.

How do we keep track of all the modules we’ve installed?

Easy! We created a clean virtual environment when we started the project, and we’ve used pip to install everything we need. So, pip knows what we’ve installed and can create a handy list for us.

Make sure you’re in the `myproject/mysite` directory, then:

pip freeze > requirements.txt

Check out `requirements.txt` to make sure it looks right. Depending when you read this tutorial, this list could change:

dj-database-url==0.5.0                                                                                                                                                  
Django==2.1.5                                                                                                                                                         django-heroku==0.3.1                                                                                                                                                    
gunicorn==19.9.0                                                                                                                                                        
psycopg2==2.7.7                                                                                                                                                         
pytz==2018.9                                                                                                                                                            
whitenoise==4.1.2

As you build your Django project, chances are you’ll find some other module you need. If so, don’t forget to `pip freeze > requirements.txt` whenever you deploy those changes.

## 6\. runtime.txt

The final thing we need to do before deploying is tell Heroku what version of Python to use.

Heroku defaults to Python 3.6.8 right now, so if you didn’t specify you’d probably be okay.

That said, it’s best practice to be specific with the versions you’re using in your stack.

Run the following in your project’s root directory to create your `runtime.txt` file:

echo "python-3.6.8" > runtime.txt

# Gitignore

Heroku (and many other cloud service providers) uses Git to deploy versions of websites and rollback any problems.

We could deploy our application right now, but let’s clean up one thing before we do.

Navigate to `myproject/mysite/` and type `ls -a`.

If you set up Git correctly earlier in this guide, you should see a file listed called `.git`

If not, find where you typed `git init` earlier and in that folder remove the `.git` file with `rm .git`. Then go back to `myproject/mysite/` and run `git init` there.

Once you’re in the right directory and alongside your `.git` file, create a new file:

touch .gitignore

Now open `.gitignore` with your favorite text editor and paste in the following information about files Git should ignore:

\### Django ###  
\*.log  
\*.pot  
\*.pyc  
\_\_pycache\_\_/  
local\_settings.py

Now, you won’t bog down the deployment process with your log and cache files. We’ll keep Heroku nice and clean.

In the future, in [Part 2 of this guide](https://medium.com/@BennettGarner/deploying-django-to-heroku-connecting-heroku-postgres-fcc960d290d1), we’ll add `db.sqlite` to this list because we’ll be using a different database on Heroku.

# Let’s Launch!!! Django on Heroku FTW.

Cross your fingers.

Hopefully we covered everything and this will go flawlessly.

Here’s what your `myproject/mysite/` should look like: [source code](https://github.com/bennett39/django-heroku-example/tree/barebones).

## 1\. Add your project’s files to Git

git add .

As long as you’re in the `myproject/mysite/` directory, this will add everything we’ve worked on — the entire Django project.

## 2\. Commit the files to Git

git commit -am "Initialize Django project"

This will commit all the files you just added.

## 3\. Push the files to the Heroku repository

The Heroku CLI added a remote repository to your git. Let’s push the files there:

git push heroku master

You should see the Heroku CLI respond with messages about your project’s build:

Counting objects: 3, done.  
Delta compression using up to 4 threads.  
Compressing objects: 100% (3/3), done.  
Writing objects: 100% (3/3), 459 bytes | 0 bytes/s, done.  
Total 3 (delta 1), reused 0 (delta 0)  
remote: Compressing source files... done.  
remote: Building source:  
remote:   
remote: -----> Python app detected  
remote: -----> Installing requirements with pip  
remote:   
remote: -----> $ python manage.py collectstatic --noinput  
remote:        119 static files copied to '/tmp/build\_54c6d4edbd5faba2e2e1bb1dbd158e89/staticfiles', 375 post-processed.  
remote:   
remote: -----> Discovering process types  
remote:        Procfile declares types -> web  
remote:   
remote: -----> Compressing...  
remote:        Done: 54.4M  
remote: -----> Launching...  
remote:        Released v6  
remote:        [https://nameless-tundra-24362.herokuapp.com/](https://nameless-tundra-24362.herokuapp.com/) deployed to Heroku  
remote:   
remote: Verifying deploy... done.  
To [https://git.heroku.com/nameless-tundra-24362.git](https://git.heroku.com/nameless-tundra-24362.git)  
   eb3dc42..c29842c  master -> master

If the build worked, open the link provided: [https://nameless-tundra-24362.herokuapp.com/](https://nameless-tundra-24362.herokuapp.com/)

![](https://miro.medium.com/max/600/1*XKU0Hwie50LGEde-cjNl-g.png?q=20)



IT WORKS!!! WOOHOO!!!

# Having Problems?

It’s likely you’ll find some helpful resources in the Heroku CLI’s error messages.

If the CLI builds the site, but clicking the link results in an error, then checkout `heroku logs --tail` for more information about the errors you’re running into.

If you’re really stuck, comment below and I and other Djangoers will try to help out.

# Ok, so now what?

We have the Django startup page working on Heroku, but there’s still a major issue.

By default, Django uses SQLite as a database. It’s a lightweight, open-source, awesome option for small to medium sized websites.

But SQLite doesn’t work great on distributed cloud computing infrastructure like Heroku.

So, we’ll need to set up a different database to use with Django on Heroku. Creating a Heroku PostgreSQL instance and connecting it to Django is the subject of a [whole different post](https://medium.com/@BennettGarner/deploying-django-to-heroku-connecting-heroku-postgres-fcc960d290d1).

_Thanks for reading!_

_Did I miss anything or mess anything up? Feel free to let me know in the comments below._

## Free Resources

Want my best content on web development and becoming a better programmer?

[Join my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).