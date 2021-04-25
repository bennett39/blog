---
title: Celery Tutorial - A Must-Learn Technology for Python Developers
date: 2020-04-19T12:54:44.451Z
description: Workers to process tasks in the background are essential & powerful tools in any developer’s toolkit.
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F578f5d63fab3&operation=register&redirect=https%3A%2F%2Fmedium.com%2Fswlh%2Fpython-developers-celery-is-a-must-learn-technology-heres-how-to-get-started-578f5d63fab3&source=post_actions_header--------------------------bookmark_preview-----------)

![](https://miro.medium.com/max/600/1*2kVzaUMFISXxe7E0pQRJPA.png?q=20)



Celery is a must-learn technology for Python developers

When you work on data-intensive applications, long-running tasks can seriously slow down your users.

Modern users expect pages to load instantaneously, but data-heavy tasks may take many seconds or even minutes to complete. How can we make sure users have a fast experience while still completing complicated tasks?

# Enter Workers + Message Queues

If we want users to experience fast load times in our application, we’ll need to offload some of the work from our web server.

## Workers

![](https://miro.medium.com/max/600/1*k7QKEzN-pSuuuPbcBCjYmQ.jpeg?q=20)



Photo by [sol](https://unsplash.com/@solimonster?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

One way we do this is with asynchronicity. While the webserver loads the next page, a second server is doing the computations that we need in the background.

We call these background, task-based servers “workers.” While you typically only have one or a handful of web servers responding to user requests, you can have many worker servers that process tasks in the background.

These workers can then make changes in the database, update the UI via webhooks or callbacks, add items to the cache, process files, send emails, queue future tasks, and more! All while our main web server remains free to respond to user requests.

## Message Queues

![](https://miro.medium.com/max/600/1*vKEhTnrZ_Sal-05uoD6rOw.jpeg?q=20)



Photo by [Melanie Pongratz](https://unsplash.com/@melanie_sophie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

We tell these workers what to do via a message queue. Put simply, a queue is a first-in, first-out data structure. When we store messages in a queue the first one we place in the queue will be the first to be processed. All tasks will be started in the order we add them.

When a worker becomes available, it takes the first task from the front of the queue and begins processing. If we have many workers, each one takes a task in order.

The queue ensures that each worker only gets one task at a time and that each task is only being processed by one worker.

# What Is Celery? Worker Management for Python Tasks

Celery allows Python applications to quickly implement task queues for many workers.

It takes care of the hard part of receiving tasks and assigning them appropriately to workers.

You use Celery to accomplish a few main goals:

1.  Define independent tasks that your workers can do as a Python function
2.  Listen to a message broker (we’ll use [Redis](https://levelup.gitconnected.com/getting-started-with-redis-its-easier-than-you-think-23d3729230fa) in this example) to get new task requests
3.  Assign those requests to workers to complete the task
4.  Monitor the progress and status of tasks and workers

# Overview of Celery + Django

In this example, we’ll use Celery inside a Django application to background long-running tasks.

Since we want Celery to have access to our database, models, and logic, we’ll define the worker tasks inside of our Django application.

However, these tasks will not run on our main Django webserver. Instead, Celery will manage separate servers that can run the tasks simultaneously in the background.

Since we need that queue to be accessible to both the Django webserver (to add new tasks) and the worker servers (to pick up queued tasks), we’ll use an extra server that works as a message broker.

That message broker server will use Redis — an in-memory data store — to maintain the queue of tasks.

![](https://miro.medium.com/max/600/1*oojFWFYyxKt9a2nJ_TvAqw.png?q=20)



Django adds tasks to Redis; Redis feeds tasks to Celery

To recap: Django creates a task (Python function) and tells Celery to add it to the queue. Celery puts that task into Redis (freeing Django to continue working on other things). On a separate server, Celery runs workers that can pick up tasks. Those workers listen to Redis. When the new task arrives, one worker picks it up and processes it, logging the result back to Celery.

# Celery Tutorial in a Django Application Using Redis

Hopefully, by now, you can see why Celery is so useful. It helps us quickly create and manage a system for asynchronous, horizontally-scaled infrastructure.

We can continue to add workers as the number of tasks increases, and each worker will remove tasks from the queue in order — allowing us to process many tasks simultaneously.

So, how does it actually work in practice? Here are the steps:

## Requirements

*   [Install Redis](https://redis.io/topics/quickstart) & start it up locally at port 6379
*   `pip install django`
*   `pip install celery`
*   `pip install redis`

# Visual learner?

No worries, I made a video of these steps!

You can also follow along below…

## 1\. Set Up Django

Let’s create a new Django project to test out Celery:

django-admin startproject celery\_tutorial

Set up the models:

cd celery\_tutorial/  
python manage.py migrate

Make sure it works:

python manage.py runserver

Visit [http://localhost:8000/](http://localhost:8000/)

## 2\. Add Celery config to Django

From the folder where `manage.py` is:

cd celery\_tutorial/

You should now be in the folder where `settings.py` is.

We need to set up Celery with some config options. Create a new file called `celery.py` :

from \_\_future\_\_ import absolute\_import, unicode\_literals  
  
import os  
  
from celery import Celery  
  
\# set the default Django settings module for the 'celery' program.  
os.environ.setdefault('DJANGO\_SETTINGS\_MODULE', 'celery\_tutorial.settings')  
  
app = Celery('celery\_tutorial')  
  
\# Using a string here means the worker doesn't have to serialize  
\# the configuration object to child processes.  
\# - namespace='CELERY' means all celery-related configuration keys  
\#   should have a \`CELERY\_\` prefix.  
app.config\_from\_object('django.conf:settings', namespace='CELERY')  
  
\# Load task modules from all registered Django app configs.  
app.autodiscover\_tasks()

This file creates a Celery app using the Django settings from our project. The last line tells Celery to try to automatically discover a file called `tasks.py` in all of our Django apps.

Save that file.

We also want Celery to start automatically whenever Django starts. So, update `__init__.py` in the same folder as `settings.py` and `celery.py` :

from \_\_future\_\_ import absolute\_import, unicode\_literals  
  
\# This will make sure the app is always imported when  
\# Django starts so that shared\_task will use this app.  
from .celery import app as celery\_app  
  
\_\_all\_\_ = ('celery\_app',)

Finally, we need to tell Celery how to find Redis. So, open `settings.py` and add this line:

CELERY\_BROKER\_URL = 'redis://localhost:6379'

## 3\. Create Your First Task

If you have an existing Django project, you can now create a file called `tasks.py` inside any app. Celery will automatically detect that file and look for worker tasks you define there.

For simplicity, though, we’re going to create our first task in `celery_tutorial/celery.py` , so re-open that file and add this to the bottom:

@app.task(bind=True)  
def debug\_task(self):  
    print('Request: {0!r}'.format(self.request))

This simple task just prints all the metadata about the request when the task is received.

It’s not a super useful task, but it will show us that Celery is working properly and receiving requests.

## 4\. Let’s Queue Our First Task!

Okay, just to recap. We…

*   Created a Celery `app` instance that manages all tasks in our application
*   Started Redis and gave Celery the address to Redis as our message broker
*   Created our first task so the worker knows what to do when it receives the task request

Now, the only thing left to do is queue up a task and start the worker to process it.

Queuing the task is easy using Django’s `shell` :

python manage.py shell

Let’s import the task and queue it up:

\>>> from celery\_tutorial.celery import debug\_task  
\>>> debug\_task.delay()  
<AsyncResult: fe261700-2160-4d6d-9d77-ea064a8a3727>

We use `.delay()` to tell Celery to add the task to the queue.

We got back a successful `AsyncResult` — that task is now waiting in Redis for a worker to pick it up!

## 5\. Start a Worker to Process the Task

There’s a task waiting in the Redis queue. Let’s start up a worker to go get and process the task.

Remember the task was just to print the request information, so this worker won’t take long.

All we have to do is run Celery from the command line with the path to our config file. Make sure you’re in the base directory (the one with `manage.py`) and run:

celery -A celery\_tutorial.celery worker --loglevel=info

You should see Celery start up, receive the task, print the answer, and update the task status to “SUCCESS”:

\-------------- celery@Bennetts-MacBook-Pro.local v4.4.2 (cliffs)  
\--- \*\*\*\*\* -----   
\-- \*\*\*\*\*\*\* ---- macOS-10.15.3-x86\_64-i386-64bit 2020-04-18 20:41:52  
\- \*\*\* --- \* ---   
\- \*\* ---------- \[config\]  
\- \*\* ---------- .> app:         celery\_tutorial:0x1107b5a90  
\- \*\* ---------- .> transport:   redis://localhost:6379//  
\- \*\* ---------- .> results:     disabled://  
\- \*\*\* --- \* --- .> concurrency: 8 (prefork)  
\-- \*\*\*\*\*\*\* ---- .> task events: OFF (enable -E to monitor tasks in this worker)  
\--- \*\*\*\*\* -----   
 -------------- \[queues\]  
                .> celery           exchange=celery(direct) key=celery  
                  
  
\[tasks\]  
  . celery\_tutorial.celery.debug\_task  
  
\[INFO/MainProcess\] Connected to redis://localhost:6379//  
\[INFO/MainProcess\] mingle: searching for neighbors  
\[2INFO/MainProcess\] mingle: all alone  
  
\[INFO/MainProcess\] Received task: celery\_tutorial.celery.debug\_task\[fe261700-2160-4d6d-9d77-ea064a8a3727\] \[WARNING/ForkPoolWorker-8\] Request: <Context: {'lang': 'py', 'task': 'celery\_tutorial.celery.debug\_task', 'id': 'fe261700-2160-4d6d-9d77-ea064a8a3727', 'shadow': None, 'eta': None, 'expires': None, 'group': None, 'retries': 0, 'timelimit': \[None, None\], 'root\_id': 'fe261700-2160-4d6d-9d77-ea064a8a3727', 'parent\_id': None, 'argsrepr': '()', 'kwargsrepr': '{}', 'origin': 'gen3931@Bennetts-MacBook-Pro.local', 'reply\_to': 'f8232d33-d7ee-3912-814c-6d531e3e9259', 'correlation\_id': 'fe261700-2160-4d6d-9d77-ea064a8a3727', 'hostname': 'celery@Bennetts-MacBook-Pro.local', 'delivery\_info': {'exchange': '', 'routing\_key': 'celery', 'priority': 0, 'redelivered': None}, 'args': \[\], 'kwargs': {}, 'is\_eager': False, 'callbacks': None, 'errbacks': None, 'chain': None, 'chord': None, 'called\_directly': False, '\_protected': 1}>\[INFO/ForkPoolWorker-8\] Task celery\_tutorial.celery.debug\_task\[fe261700-2160-4d6d-9d77-ea064a8a3727\] succeeded in 0.0015866540000000207s: None

Woohoo! We’re now using Celery — just that easy.

# How You Might Deploy It

Well, it’s working locally, but how would it work in production?

Basically, no matter what cloud infrastructure you’re using, you’ll need at least 3 servers:

1.  Django web server
2.  Redis message broker
3.  Celery worker server

The cool thing about Celery is its scalability. So you can add many Celery servers, and they’ll discover one another and coordinate, using Redis as the communication channel.

This allows for a very high throughput of tasks. As you add more tasks to the queue (e.g. from more users), you can add more worker servers to scale with demand.

Individual worker tasks can also trigger new tasks or send signals about their status to other parts of the application. This means that decoupled, microservice-based applications can use Celery to coordinate and trigger tasks across services.

# Just Scratching the Surface

Celery is an incredibly powerful tool. Most major companies that use Python on the backend are also using Celery for asynchronous tasks that run in the background.

As a Python developer, I don’t hear enough people talking about Celery and its importance. I’m a huge fan of its simplicity and scalability.

If you’re a Python backend developer, Celery is a must-learn tool.

# Free Resources

Want my best content on web development and becoming a better programmer?

[Join my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).

[

## Free Email Series for Medium Readers | Bennett Garner

### Join over 500 other developers currently in my email series.

mailchi.mp







](https://mailchi.mp/2e671faffc04/bennett-medium)