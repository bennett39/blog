---
title: Vue + Django - Using .vue Files and the Vue CLI
date: 2021-03-08T16:04:37.427Z
description: Build a full-stack website running on Django
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd6dd8c9145eb&operation=register&redirect=https%3A%2F%2Fbetterprogramming.pub%2Fvue-django-using-vue-files-and-the-vue-cli-d6dd8c9145eb&source=post_actions_header--------------------------bookmark_preview-----------)

![Different-colored folders](https://miro.medium.com/max/600/0*3x4gHfpDbWrI_aI-?q=20)



Photo by [Andrew Pons](https://unsplash.com/@imandrewpons?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral).

Starting to use Vue.js within a Django project is pretty straightforward.

If you just want to build a few simple Vue components and drop them into your Django app, you can do it the simple way — inline. I’ve written a whole Vue + Django tutorial demonstrating that approach:

[

## Vue + Django: Getting Started

### Adding Vue to your Django app is pretty easy!

levelup.gitconnected.com



](https://levelup.gitconnected.com/vue-django-getting-started-88d3f4c2ba62)

But inline JavaScript/CSS is limited, and that approach will only get you so far. I’ve learned this firsthand by building apps with Vue and Django.

# Django Can Play Nice With the Vue CLI and Webpack

In order to get the most out of Vue, you’ll probably want to use the Vue CLI and `.vue` files. These tools enable you to create easily reusable components and build JavaScript dependencies into your project with webpack. But they also require you to compile and serve dynamic JavaScript assets, complicating deployment.

Luckily, Django has great tooling for using and deploying static files that we can pair with the Vue CLI to deploy fully featured Vue apps pretty easily. It takes a little bit of configuration, but here’s how I got my application to work.

# Experienced Devs: Want To Skip the Reading and See the Code?

Happy to help:

[

## bennett39/vue-cli-tutorial

### Contribute to bennett39/vue-cli-tutorial development by creating an account on GitHub.

github.com



](https://github.com/bennett39/vue-cli-tutorial)

# What You Need To Know: How the Vue CLI Works

The [Vue CLI](https://cli.vuejs.org) is a great tool for starting fully featured Vue projects. It helps you bootstrap all the configuration needed for a complex Vue application.

The CLI uses [webpack](https://webpack.js.org/) behind the scenes to compile JavaScript assets into a single fully built app with all the dependencies available.

*   When you use yarn (or npm) to run `yarn serve`, you’re telling webpack to compile the necessary assets and then start a web server that listens for changes in those files and re-compiles the result.
*   When you use yarn (or npm) to run `yarn build`, webpack will compile an optimized version of all the assets and write the production code to an output directory (usually something like `dist/`).

Webpack compiles your dependencies and builds them for you so that you can have multi-file, complex JavaScript applications. The key thing to realize here is that webpack can actually write the compiled version of the app anywhere we tell it to.

# Making the Vue CLI Work With Django

To get the Vue CLI (and `.vue` files) working inside Django, we just need to tell webpack to write its output somewhere that Django knows about.

If we write our JavaScript assets to a directory that Django can detect with its `[STATICFILES_DIRS](https://docs.djangoproject.com/en/3.1/ref/settings/#staticfiles-dirs)` [setting](https://docs.djangoproject.com/en/3.1/ref/settings/#staticfiles-dirs), then Django will serve those files like any other.

Since Django can detect changes in static files itself, we can use `python manage.py runserver` at the same time as `yarn serve` to dynamically pick up any changes within our web app during development.

When it comes time to deploy, we simply need to `yarn build` the Vue app and then run `python manage.py collectstatic` to make sure all the production files are available to Django.

# Wire It Up

If it seems too easy to be true, that’s because it actually is relatively simple. There are six steps to using the Vue CLI with Django:

1.  Start a Vue CLI project inside Django.
2.  Use the Vue app /components inside a Django template.
3.  Set up Django static files.
4.  Configure Vue to build assets to Django’s static files.
5.  In development, run Django with `manage.py` and Vue with `yarn serve`.
6.  For deployments, build Vue and then use Django’s `collectstatic`.

Sound easy enough? Let’s get into it.

# 1\. Start a Vue CLI Project Inside Your Django Project

I assume you already have a Django project and it looks something like this:

.  
|-myproject  
| |-asgi.py  
| |-\_\_init\_\_.py  
| |-settings.py  
| |-urls.py  
| |-wsgi.py  
|-myapp  
| |-migrations  
| | |-\_\_init\_\_.py  
| |-models.py  
| |-\_\_init\_\_.py  
| |-apps.py  
| |-admin.py  
| |-tests.py  
| |-views.py  
|-manage.py

If you don’t have a Django project yet, here’s a quick video I made on how to start one (just with a different project name):

Step-by-step how to get started with Django

To add Vue, you’ll want to [install the Vue CLI](https://cli.vuejs.org/guide/installation.html) and [create a new Vue project](https://cli.vuejs.org/guide/creating-a-project.html#vue-create) in the base directory of your project (where `manage.py` is):

vue create vueapp

Then pick your configuration settings:

![Configuration settings](https://miro.medium.com/max/600/0*7iygi7jdiC8ijaAs?q=20)



We’ll go with Vue 2 for now since Vue 3 isn’t fully released and supported yet.

Next:

cd vueapp/  
yarn serve

To check your installation, go to [http://localhost:8080/](http://localhost:8080/):

![Vue app](https://miro.medium.com/max/600/0*8t-AD57oivMvGSxc?q=20)



## How does it work?

OK, let’s figure out how it works under the hood. Take a look at `vueapp/src/main.js`:

Notice that Vue is creating/mounting a Vue instance anywhere it finds an element with the ID `#app`. This means that if we can get Vue to load with our Django templates, we should only need `<div id=”app”>` to get Vue to initialize on a page.

Then we can use Vue components inside Django templates!

# 2\. Create a Test Django Template

You may already have a Django template where you want Vue to load. If so, great. You can use that.

For simplicity and clarity, I’m going to create a new template (and view/URL) in `myapp/`, the Django app I created at the beginning.

There’s the template. Now let’s make a few more updates so it has a URL and view for it:

That should do it. Now navigate to [http://127.0.0.1:8000/vue-test](http://127.0.0.1:8000/vue-test) and you’ll see our test template is loading, but not the Vue part of it yet!

![Test template](https://miro.medium.com/max/600/0*EXTOBP_MOpQ5Abu5?q=20)



Let’s get Vue to mount to the `#app` element on that page!

# 3\. Set Up Django’s Static Files

The first thing we need is a place for the Vue files to live after they get built. We’ll need that to be a location that Django knows about.

If you don’t have static files set up in your app at all, you can [follow the Django documentation](https://docs.djangoproject.com/en/3.1/howto/static-files/) to make sure your settings are right to get started. They should be correct if you’re starting from scratch along with this tutorial.

In our case, the Vue files aren’t necessarily tied to a particular Django app, so we’ll want to create a generic location for static files at our project root.

1.  At the project root (where `manage.py` is), create a new folder named `static/` that will hold all the files we build from Vue.
2.  In `myproject/settings.py`, we need to update the `staticfiles` settings so that Django knows about this new directory. These settings are usually at the very bottom of the file.

Notice that we made `STATIC_ROOT` point to a new nonexistent folder in our project: `var/static_root/`. Django will create and manage that folder for us. When it comes time to deploy, that folder will be important.

When you’re done making changes, test it out!

$ python manage.py collectstatic131 static files copied to ‘/Users/bennettgarner/Repos/vue-cli-tutorial/myproject/var/static\_root’.

# 4\. Point Vue Project’s Build Directory in Config

OK, Django is now collecting and serving our static files, and we have a place to put the Vue files once they’re compiled. We just need to tell Vue where to put them.

We can override Vue’s default configuration by creating a file: `vueapp/vue.config.js`

What’s going on here?

*   `publicPath` tells Vue where to look for its other files once it’s on the internet. Basically, what URL my other files will live at. Django puts them all behind `STATIC_URL` and the path to our output directory.
*   `outputDir` tells Vue where to write its files in the file system. This is the big one, and here I’ve pointed it to `static/` (which we just added to `STATICFILES_DIRS`) plus a namespaced path to make sure Vue files don’t get mixed up with others.
*   By default, Vue hashes file names so that we’re sure we have the most recent version (not a cached old version of the file). However, that messes up Django’s ability to predict and track file names across changes. Instead, we can use `[ManifestStaticfilesStorage](https://docs.djangoproject.com/en/3.1/ref/contrib/staticfiles/#django.contrib.staticfiles.storage.ManifestStaticFilesStorage)` with Django to accomplish the same task.
*   The runtime compiler is turned on because we’re doing tricky stuff with webpack and I want the full build. You may be able to remove this setting if your project works without it. It will make your deployment 30% lighter in weight. [Read about the runtime compiler](https://vuejs.org/v2/guide/installation.html#Runtime-Compiler-vs-Runtime-only) in the Vue docs.
*   We write to disk in development mode so that Django can pick up those files, instead of the compilation happening on the fly inside of Vue.

Try it out!

1.  `cd vueapp/`
2.  `yarn serve`
3.  Cancel the server with `ctl+c`.
4.  `ls ../static/src/vue/dist/`

You’ll see that all our Vue files got built to the static directory in our Django app!

# 5\. Run Django in Development With manage.py

Now we just need to add the static Vue files to the template we made earlier. Edit `myapp/templates/myapp/vue-test.html`:

We want the Vue files to load after the rest of the page is already loaded, so that’s why they’re at the bottom.

Test it out!

1.  In project root: `python manage.py runserver`
2.  Open a new terminal tab and go to `vueapp/` to run `yarn serve`.
3.  Go to [http://127.0.0.1:8000/vue-test](http://127.0.0.1:8000/vue-test).

![Testing Vue](https://miro.medium.com/max/600/0*VEXZhYgEN7Wqexpj?q=20)



We did it! That’s a `.vue` file component rendering correctly inside a Django template!

# 6\. Build Vue + collectstatic for Deployments

When it comes time for deployments, you’ll want to:

1.  Enable `[ManifestStaticfilesStorage](https://docs.djangoproject.com/en/3.1/ref/contrib/staticfiles/#django.contrib.staticfiles.storage.ManifestStaticFilesStorage)` to help with versioning and cache busting.
2.  Push your code up as is to GitHub/GitLab or whatever you use.
3.  As part of the deploy commands, `yarn build` and `python manage.py collectstatic`.

# Vue CLI and .vue Files on Django

With a little bit of configuration, Vue and Django can play nice together!

You can now take the sample app and make edits to the components as you would with any other Vue project. Django will pay attention to the files Vue is writing and then serve them as static assets.

## Final reminders

Don’t forget: Vue looks for an `#app` element on the page, so you’ll need that anywhere you intend to use Vue.

In addition, you’ll need the script tags on any page where you want to use Vue. The best practice here is to create a partial [base template in Django](https://docs.djangoproject.com/en/3.1/ref/templates/language/#template-inheritance) with all this info that you can then extend for your individual pages.

And that’s it! Congrats, you have a full-stack website running on Django!

# Want to level up?

Access my best content on web development and becoming a better programmer, for free.

[Join 500 other developers in my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).

[

## Free Email Series for Medium Readers | Bennett Garner

### Join over 500 other developers currently in my email series.

mailchi.mp







](https://mailchi.mp/2e671faffc04/bennett-medium)