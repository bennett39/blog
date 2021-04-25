---
title: Deploying Django to Heroku - Connecting Heroku Postgres
date: 2019-01-25T17:00:14.831Z
description: Here’s everything you need to know to get your database working on Heroku.
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffcc960d290d1&operation=register&redirect=https%3A%2F%2Fblog.usejournal.com%2Fdeploying-django-to-heroku-connecting-heroku-postgres-fcc960d290d1&source=post_actions_header--------------------------bookmark_preview-----------)

Here’s everything you need to know to get your database working on Heroku.

![](https://miro.medium.com/max/600/1*uiZTYxYbMIajgS52sJu53w.png?q=20)



Deploying a Django app successfully to Heroku is far from self-explanatory.

In a different post, I outlined how to get [barebones Django running as a brand new Heroku app](https://medium.com/@BennettGarner/deploying-django-to-heroku-procfile-static-root-other-pitfalls-e7ab8b2ba33b).

But Django isn’t very useful unless it has a database to talk to, and Heroku doesn’t work with Django’s default SQLite database.

Luckily, Heroku Postgres databases are free or cheap for hobby developers. In this guide, I’ll show you how to set it up.

_(Are you an experienced dev just looking for some source code?_ [_Happy to oblige_](https://github.com/bennett39/django-heroku-example)_.)_

# Local vs. Remote

Let me make it clear what we’re doing here.

We have a Django app that works locally on our computer. If we run:

python manage.py runserver

Then, we’ll see a working Django app at [http://localhost:8000](http://localhost:8000)

That local version of the Django app is using `db.sqlite3` as its database.

However, when we visit the Heroku version, YourProjectName[.herokuapp.co](http://projectname.herokuapp.com)m, Heroku will need to use a Postgres database instead.

What we want to do by the end of this guide is get our app running with SQLite whenever we’re working on it locally, and with Postgres whenever it’s in production.

(Alternatively, you can set up Postgres locally as well, but that’s a subject for another guide.)

# DATABASE\_URL

If you followed the steps in [my last guide](https://medium.com/@BennettGarner/deploying-django-to-heroku-procfile-static-root-other-pitfalls-e7ab8b2ba33b), Heroku already created an instance of Postgres for our Django app. You just didn’t know it.

In your project’s directory, using the Heroku CLI, run:

heroku config

If Heroku created a Postgres instance that’s linked to this app, you’ll see it:

\=== nameless-tundra-24362 Config Vars  
DATABASE\_URL: postgres://\[DATABASE\_INFO\_HERE\]

I’ve removed the database info from the code snippet above because this URL is literally your database’s location and access credentials all in one.

Anyone with this URL can access your database, so be careful with it.

You’ll notice that Heroku saves it as an environment variable called `DATABASE_URL` . This URL can and does change, so you should never hard code it. Instead, we’ll use the variable `DATABASE_URL` in our application.

If your `DATABASE_URL` variable isn’t set, chances are Heroku doesn’t have a Postgres instance linked to your app. You may have to create it manually.

# Steps to Using Postgres Remotely & SQLite Locally

There are a few key changes we need to make to our application to get everything running.

The key thing we’ll be doing here is setting `DATABASE_URL` to the Heroku-provided variable when we’re on Heroku. When we’re working locally, we’ll use a local file — `.env` — to set `DATABASE_URL` to point to SQLite.

That way, any time we use the `DATABASE_URL` variable it will point to the correct database based on the environment.

## Install python-dotenv

Inside your project’s root directory (alongside `manage.py`), run:

pip install python-dotenv

This installs python-dotenv and also a related module called dj-database-url.

Add the new modules to your `requirements.txt`:

pip freeze > requirements.txt

## Create .env

We’ll use a file called `.env` to tell Django to use SQLite when running locally.

To create `.env` and have it point Django to your SQLite database:

echo 'DATABASE\_URL=sqlite:///db.sqlite3' > .env

Now, we don’t want `.env` to make it to Heroku, because `.env` is the part of our app that points to SQLite and Heroku doesn’t like SQLite.

So, we need git to ignore `.env` when pushing to Heroku. Run:

echo '.env' >> .gitignore

## Update settings.py

Now that we have a `.env` file, we need to tell Django to use it. Hence why we downloaded the `python-dotenv` plugin earlier.

In your project’s `settings.py` make the following changes to get Django to use `.env`.

**1\. Import new modules at the top of the file:**

import dj\_database\_url  
import dotenv

**2\. Load environment variables from** `**.env**` **if it exists:**

\# This line should already exist in your settings.py  
BASE\_DIR = os.path.dirname(os.path.dirname(os.path.abspath(\_\_file\_\_)))\# This is new:  
dotenv\_file = os.path.join(BASE\_DIR, ".env")  
if os.path.isfile(dotenv\_file):  
    dotenv.load\_dotenv(dotenv\_file)

Since `.env` won’t exist on Heroku, `dotenv.load_dotenv(dotenv_file)` will never get called on Heroku and Heroku will proceed to try to find its own database — Postgres.

**3\. Add WhiteNoise to** `**MIDDLEWARE**`**:**

WhiteNoise helps Django manage static files (images, scripts, etc) to load your site faster. It’s already installed by default and easy to add while we’re editing `settings.py`:

MIDDLEWARE = \[  
    'django.middleware.security.SecurityMiddleware',  
    'whitenoise.middleware.WhiteNoiseMiddleware',  
    ...  
\]

Django security middleware should already be the first thing on the list. Never load any middleware before Django security.

Add the WhiteNoise middleware right below Django security.

Check out the [WhiteNoise docs](http://whitenoise.evans.io/en/stable/django.html#django-middleware) for more information.

**4\. Change** `**DATABASES**`

Currently your `settings.py` has this:

DATABASES = {      
    'default': {          
        'ENGINE': 'django.db.backends.sqlite3',          
        'NAME': os.path.join(BASE\_DIR, 'db.sqlite3'),      
    }  
}

Change it to this:

DATABASES = {}  
DATABASES\['default'\] = dj\_database\_url.config(conn\_max\_age=600)

The idea here is to clear the `DATABASES` variable and then set the `'default'` key using the `dj_database_url` module. This module uses Heroku’s `DATABASE_URL` variable if it’s on Heroku, or it uses the `DATABASE_URL` we set in the `.env` file if we’re working locally.

**5\. Add WhiteNoise to** `**STATICFILES_STORAGE**`

Django has a variable that lets it know where to store and retrieve static files. Let’s point it to WhiteNoise.

Add:

STATICFILES\_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

I recommend adding the above in the same place as your `STATIC_URL` and `STATIC_ROOT` variables.

**6\. sslmode issue workaround**

If you ran the Django application as specified above, you might get an error when working locally because the `dj_database_url` module wants to log in with SSL. Heroku Postgres _requires_ SSL, but SQLite doesn’t need or expect it.

So, we’ll use a hacky workaround to get `dj_database_url` to forget about SSL at the last second.

As the very last line in your `settings.py`, add:

\# This should already be in your settings.py  
django\_heroku.settings(locals())\# This is new  
options = DATABASES\['default'\].get('OPTIONS', {})  
options.pop('sslmode', None)

# Make Sure You Got Everything

Check your app against this [barebones source code](https://github.com/bennett39/django-heroku-example).

# Test It Out!

## Did we break anything about the old local build?

python manage.py runserver

It works!

![](https://miro.medium.com/max/600/1*ergys03jgWfLDd7wOFSLnQ.png?q=20)



## Can we connect to the local SQLite database and migrate?

python manage.py migrate

Then create a superuser for the local database admin:

python manage.py createsuperuser

Check it out:

python manage.py runserver

And navigate to [http://localhost:8000/admin/](http://localhost:8000/admin/) and login with your newly created superuser credentials.

## Did we break anything on the remote Heroku build?

Try pushing our working local app to Heroku:

git add .  
git commit -am "Implement database connection"  
git push heroku master

Will it build?

Counting objects: 13, done.  
Delta compression using up to 4 threads.  
Compressing objects: 100% (11/11), done.  
Writing objects: 100% (13/13), 5.29 KiB | 0 bytes/s, done.  
Total 13 (delta 6), reused 0 (delta 0)  
remote: Compressing source files... done.  
remote: Building source:  
remote:   
remote: -----> Python app detected  
remote: -----> Installing requirements with pip  
remote:        Collecting python-dotenv==0.10.1 (from -r /tmp/build\_647b7c85726703d0ffc33248e829a90f/requirements.txt (line 6))  
remote:          Downloading [https://files.pythonhosted.org/packages/8c/14/501508b016e7b1ad0eb91bba581e66ad9bfc7c66fcacbb580eaf9bc38458/python\_dotenv-0.10.1-py2.py3-none-any.whl](https://files.pythonhosted.org/packages/8c/14/501508b016e7b1ad0eb91bba581e66ad9bfc7c66fcacbb580eaf9bc38458/python_dotenv-0.10.1-py2.py3-none-any.whl)  
remote:        Installing collected packages: python-dotenv  
remote:        Successfully installed python-dotenv-0.10.1  
remote:   
remote: -----> $ python manage.py collectstatic --noinput  
remote:        /app/.heroku/python/lib/python3.6/site-packages/psycopg2/\_\_init\_\_.py:144: UserWarning: The psycopg2 wheel package will be renamed from release 2.8; in order to keep installing from binary please use "pip install psycopg2-binary" instead. For details see: <[http://initd.org/psycopg/docs/install.html#binary-install-from-pypi](http://initd.org/psycopg/docs/install.html#binary-install-from-pypi)\>  
remote:          """)  
remote:        119 static files copied to '/tmp/build\_647b7c85726703d0ffc33248e829a90f/staticfiles', 362 post-processed.  
remote:   
remote: -----> Discovering process types  
remote:        Procfile declares types -> web  
remote:   
remote: -----> Compressing...  
remote:        Done: 54.4M  
remote: -----> Launching...  
remote:        Released v7  
remote:        [https://nameless-tundra-24362.herokuapp.com/](https://nameless-tundra-24362.herokuapp.com/) deployed to Heroku  
remote:   
remote: Verifying deploy... done.  
To [https://git.heroku.com/nameless-tundra-24362.git](https://git.heroku.com/nameless-tundra-24362.git)  
   c29842c..957955a  master -> master

Success!

Check the URL just to make sure: [https://nameless-tundra-24362.herokuapp.com/](https://nameless-tundra-24362.herokuapp.com/)

## Can we connect to the remote Heroku Postgres instance and migrate?

heroku run bash

This lets us into the shell for our project on Heroku. In the Heroku shell:

python manage.py migrate

Just like we did for the local version.

(In the future, when you make migrations locally, you can simply push those migrations along with the rest of your app. In Heroku’s shell you should only ever have to `migrate` and never `makemigrations`.)

Create the super user in the Postgres version of the database. In the Heroku shell, still:

python manage.py createsuperuser

Now navigate to yourprojectname.herokuapp.com/admin/ and log in to confirm everything is working!

# Congratulations!

You’ve got a fully functional Django app running on Heroku with a Postgres database.

Now, you’re free to add views, urls, models, and all the other good Django stuff to your heart’s content.

_Questions, issues, concerns, problems, recommendations, death threats? Let me know in the comments below._

## Free Resources

Want my best content on web development and becoming a better programmer?

[Join my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).