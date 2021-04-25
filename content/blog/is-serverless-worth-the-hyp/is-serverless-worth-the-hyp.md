---
title: Getting Started with AWS Lambda - Is Serverless Worth the Hype?
date: 2021-02-18T22:05:05.707Z
description: Let’s see if serverless is really better with an AWS Lambda example
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9bcb1842678b&operation=register&redirect=https%3A%2F%2Fbetterprogramming.pub%2Fis-serverless-worth-the-hype-9bcb1842678b&source=post_actions_header--------------------------bookmark_preview-----------)

![A big red X on top of servers](https://miro.medium.com/max/600/1*TEo7AmE8yPcjigscqzE-_w.png?q=20)



Down with the servers (except not really)! — Photo from Wikimedia Commons.

# Serverless Has a Bad Reputation

“Serverless” is a hot buzzword, but it often gets an eye roll from the developers I know — especially when mentioned in connection with microservices.

Advocates of serverless say it scales seamlessly to meet demand while not wasting resources if not in use… if you set it up correctly.

On the other hand, many developers see serverless architecture as overly complicated — especially when it comes to rearchitecting existing applications and setting up deployment strategies.

So, who is right? As a developer, you’ll hear a lot of opinions, but my best advice is this: If you’re interested in a new technology, try it out.

# Giving Serverless a Shot

Luckily for us, the most popular serverless provider (AWS) lets you try their serverless product (Lambda) for free.

![AWS Lambda sales page](https://miro.medium.com/max/600/1*8lAoQ9AvjrARmFivaEaiJw.png?q=20)



What does the sales page have to say?

Taking a look at the sales copy gives us a high-level overview of the biggest promised benefits of serverless. If we build our application right, we should get:

1.  An application that scales with demand.
2.  An application that is cheap to operate when demand is low.
3.  An infrastructure that we don’t have to think about.

Keep those three points in mind, as those are the criteria we’ll be using to judge serverless in the end.

# The Elephant in the Room: No, Serverless Is Not Server-less

It’s really not a great name — or at least it’s a misleading name.

Of course, your application still runs on some combination of processors, RAM, and hard drives somewhere.

Serverless architecture simply abstracts away a lot of the finer details.

This abstraction is just a continuation of a fundamental trend in computer science. The history of the field includes all kinds of examples of how [abstraction makes working with computers easier](https://levelup.gitconnected.com/want-to-understand-computer-science-study-abstraction-cb785a19bbc5).

# So What Is a Lambda?

Here’s the easiest way to think of it: A lambda is a function.

That function spins up, processes information, usually returns a response, and then goes away.

The real innovation here is you can spin up thousands or millions of instances of the same function to process high levels of traffic or throughput for your application.

Where things get tricky is managing the ways that the function can get triggered (e.g. API, metric triggers, notification systems, message queues, etc.).

Additionally, making sure the lambda has access and permissions on other services (e.g. database, cache, email queuing, file storage, etc.) across AWS can be a challenge. In order to do useful work, the lambda needs to be able to access these services, but that often means managing rulesets and access policies for every bit of code you write.

# Try It Out!

OK, so we want to test out Lambda. Ultimately, we want to see how serverless works and if it’s easy to get started.

Here are the high-level steps we’ll need to take:

1.  Create a lambda.
2.  Define the triggers for that lambda.
3.  Write some code to call and use the response of the lambda.

If it seems simple, that’s because it actually is pretty easy to get started. Let’s take a look.

# 1\. Create a Lambda

Head into AWS (create an account if you don’t have one) and navigate to the AWS Lambda page.

![Lambda page](https://miro.medium.com/max/600/1*r8RnNZDnLrSTLxgTe-XzVw.png?q=20)



My Lambda page. You might see a sales page. If so, just click the button to continue.

Once we’ve finished creating our lambda and we start to use it, cool metrics will show up here about how much it’s being called.

We don’t have any lambdas yet, so click the “Create function” button.

![Page with “Create function” button](https://miro.medium.com/max/600/1*VfiZaO9IZMvcrSYPi9Te0A.png?q=20)



If you want, browse through the blueprints in the “Use a blueprint” section. They’ll help acquaint you with what kinds of tasks you can accomplish with lambdas.

For now, choose “Author from scratch.”

I’ll be coding my lambda in Python because it’s the language I like and use most often, but one of the benefits of lambda is you can write your code in a variety of languages.

Click “Create function.”

## Writing a lambda

Welcome to the lambda creation page! We’re already pretty close to having a completed lambda.

![Lambda creation page](https://miro.medium.com/max/600/1*QgkbjLyQhUYU22UCXfz8Yg.png?q=20)



We’ll return to the “Designer” section in a moment when we want to add triggers for the event. For now, you can minimize that section and look at the “Function code” section.

![Function code section](https://miro.medium.com/max/600/1*EokcpA9Bg4cDktKyvu3GxA.png?q=20)



As I mentioned, lambdas are just basic functions (for our purposes today) that get called via a certain trigger. Click on `lambda_function.py` and you’ll see the default function that AWS has already pre-loaded for us.

![Default function](https://miro.medium.com/max/600/1*v6Rg2xf8dzDajjjUUe2ZyQ.png?q=20)



A few things to notice here:

*   The `lambda_handler` function takes two arguments — the `event` that triggered it and any `context` that goes along with this lambda instance.
*   We’re going to need to add some logic to tell the handler what work needs to be done.
*   Then we `return` a dictionary that looks like a response you might expect to see via an API.

With those basic rules, we can write pretty complex logic.

## Testing our lambda

For now, let’s just run the sample code from Amazon. Click “Test” and you’ll see the screen to create a test event:

![Creating a test event](https://miro.medium.com/max/600/1*Dt-f1ARsCGZcMjkxIp9b5Q.png?q=20)



We’ll just leave the default values for now, but you can create your own test events for your function to make sure it handles all the cases you’d expect.

Events can come from many places and take different formats, so testing your lambdas is super important.

Click “Create” and then click “Test” to see the results of our lambda!

{  
  "statusCode": 200,  
  "body": "\\"Hello from Lambda!\\""  
}Function Logs  
START RequestId: aa45ea9e-8726-49a9-b12b-556e44bd2646 Version: $LATEST  
END RequestId: aa45ea9e-8726-49a9-b12b-556e44bd2646  
REPORT RequestId: aa45ea9e-8726-49a9-b12b-556e44bd2646 Duration: 0.91 ms Billed Duration: 1 ms Memory Size: 128 MB Max Memory Used: 50 MBRequest ID  
aa45ea9e-8726-49a9-b12b-556e44bd2646

Notice that we get a response back with the results of our successful task execution.

We also get a short report about the lambda with billing information.

## Lambda billing

AWS charges for serverless computing by the number of requests and the number of seconds it takes your task to complete.

[From AWS](https://aws.amazon.com/lambda/pricing/):

> “With AWS Lambda, you pay only for what you use. You are charged based on the number of requests for your functions and the duration, the time it takes for your code to execute.
> 
> Lambda counts a request each time it starts executing in response to an event notification or invoke call, including test invokes from the console.
> 
> The AWS Lambda free usage tier includes 1M free requests per month and 400,000 GB-seconds of compute time per month.”

So, as long as we don’t make 1 million requests or those requests don’t take too long, we’ll be well within the free usage tier.

The benefit of serverless here is we’re not paying for idle time. We only pay for computing resources when they’re needed, down to the millisecond.

With that said, if you write inefficient lambda code that takes a while to execute, you’ll deplete your free tier much faster. In fact, lambdas are restricted to small tasks that take less than 15 minutes to execute.

## Make our code more interesting: the sandwich builder!

“Hello World” is boring. Let’s make something truly innovative and useful (/sarcasm).

How about an API that suggests random cold cut sandwiches and even allows you to specify parts of the sandwich?

Here’s what I have in mind:

Any time you make a change to the lambda, you have to click “Deploy.” So, make sure you click that before you test!

And if we test it out… we get a random sandwich!

Response  
{  
  "statusCode": 200,  
  "body": "\\"tempeh & gruyere with tomato\\""  
}Function Logs  
START RequestId: 5694048c-0edc-4bc8-893a-a4e19dd4e1ee Version: $LATEST  
END RequestId: 5694048c-0edc-4bc8-893a-a4e19dd4e1ee  
REPORT RequestId: 5694048c-0edc-4bc8-893a-a4e19dd4e1ee Duration: 1.00 ms Billed Duration: 2 ms Memory Size: 128 MB Max Memory Used: 51 MB Init Duration: 109.90 msRequest ID  
5694048c-0edc-4bc8-893a-a4e19dd4e1ee

Now let’s edit our test case and try specifying our own meat for the sandwich:

![Editing test case](https://miro.medium.com/max/600/1*yoHUA4kHbSJtizGGVouyZQ.png?q=20)



Run the test:

Response  
{  
  "statusCode": 200,  
  "body": "\\"capicola & provolone with onions\\""  
}Function Logs  
START RequestId: 77a2eeeb-26c8-4e70-af53-5d3d6f8956ea Version: $LATEST  
END RequestId: 77a2eeeb-26c8-4e70-af53-5d3d6f8956ea  
REPORT RequestId: 77a2eeeb-26c8-4e70-af53-5d3d6f8956ea Duration: 0.95 ms Billed Duration: 1 ms Memory Size: 128 MB Max Memory Used: 51 MBRequest ID  
77a2eeeb-26c8-4e70-af53-5d3d6f8956ea

Yum! That sounds like a good sandwich!

# 2\. Expose an API for Our Lambda

So, we have a _super-useful_ sandwich-building lambda and now we want to share this beautiful innovation with the world.

To do so, let’s expose an API for our lambda. We’ll need to specify a way for the API to trigger the lambda and then receive its response.

Open the “Designer” again:

![Lambda creation page](https://miro.medium.com/max/600/1*QgkbjLyQhUYU22UCXfz8Yg.png?q=20)



Click “Add trigger.” And we’ll use AWS’s internal tool, API Gateway:

![Using API Gateway](https://miro.medium.com/max/600/1*Let72sVBtw0Cm96314JiDw.png?q=20)



For the purposes of this tutorial, I’m creating an open API, meaning anyone can access it.

When you’re actually using lambdas, you’ll want to add some security and authentication to your API, but that would make this tutorial way too long.

Once you add the API, you’ll see it show up as one of the triggers for your lambda:

![Lambda triggers](https://miro.medium.com/max/600/1*MQ3ivnI6QpB74UA20Q1k9w.png?q=20)



And if you click on the API Gateway trigger, you’ll see some information about it below:

![API Gateway trigger info](https://miro.medium.com/max/600/1*FJkuk1fNERiR9YIJf-eo6w.png?q=20)



If we visit that API endpoint, we see the raw response!

![Raw response](https://miro.medium.com/max/600/1*1Ld1h8fFUbMJ6hgjNsOckA.png?q=20)



## Making the response more like an actual API

Normally, if you hit an API endpoint, you’d expect to get a JSON object back — not just a string.

So, let’s update the return value of our lambda:

return {  
        'statusCode': 200,  
        'body': json.dumps(dict(sandwich=sandwich))  
    }

I’ve just wrapped the response in a dictionary so that when we ping the API, we’ll get a JSON object.

Any time you make a change to the lambda, you have to click “Deploy.” So, make sure you click that before you test!

Try it out in Postman, and we get valid JSON back!

![Getting valid JSON back](https://miro.medium.com/max/600/1*qb8VB0wn7_N9pjCgZq_HBg.png?q=20)



# 3\. Use Our API

Woohoo, we’ve built something! Let’s try to make it work in the context of a real website!

Let’s create a page with a button that allows you to ping the API:

Save it as `index.html` and then open that file with your browser. Make sure to update `apiUrl` with your value.

Here’s our sandwich generator site!

![Sandwich generator site](https://miro.medium.com/max/600/1*RmPbxibqjobc4dHZXEZ33w.png?q=20)



But if you click the button, nothing happens!

If we check the console, we’ll see that we have a dreaded CORS error. Oh no!

Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at [https://\*\*\*.execute-api.us-east-2.amazonaws.com/default/lambda\_tutorial](https://npbhu6zm3f.execute-api.us-east-2.amazonaws.com/default/lambda_tutorial). (Reason: CORS header ‘Access-Control-Allow-Origin’ missing).

## Fixing the CORS error

Unfortunately, because we developed `index.html` locally, the `file://` prefix isn’t playing nicely with Amazon’s CORS policy.

To fix CORS errors, we need to deploy the site somewhere or run it on a server.

The easiest way I can think to get it working is to upload `index.html` to a Git repository.

![Uploading to a Git repo](https://miro.medium.com/max/600/1*VT6b8JgW57gtfwhQDjGDCg.png?q=20)



Then create a new Netlify or GitHub Pages app from that repo with an empty build command and empty publish directory:

![Creating a new Netlify app](https://miro.medium.com/max/600/1*maGA1xjKoBN7j_0Kx8T8SA.png?q=20)



Then, we’ll grab a sweet domain in the Netlify settings and we’re live at [https://sandwich-generator.netlify.app/](https://sandwich-generator.netlify.app/).

Yay! Go make a sandwich — seriously.

# A Note to All the Haxors

I know the API is currently open and smart people can figure out its URL and how to access it.

Feel free to use it, but if you make a ton of requests and this post starts to get expensive for me, then I’ll have to take it down or add authorization.

Please don’t create more work for me just because you’re being cute and pinging my API a million times. I’m calling on human decency here.

I’ve also added rate limiting to the API, so if you were thinking of doing something mean, just save yourself and the rest of us the trouble.

# What Did We Learn About Serverless?

Remember, we had three criteria for judging serverless. Let’s see what we learned about each of them.

## 1\. An application that scales with demand

We didn’t really stress test our sandwich generator, but it’s easy to see that it should scale well on AWS’s infrastructure.

API Gateway can handle and balance millions of requests with low latency. In turn, each instance of the lambda can spin up its own response to each of those requests.

Of course, you could build a sandwich generator in pure JavaScript in the browser. We didn’t need a lambda to build this specific application.

Where lambdas start to get powerful is when you link them to data storage (RDS, Dynamo, S3) and different workflows (triggers and destinations).

Depending on your choice of other infrastructure, it may not scale up and down easily with demand. A lambda is just a link in a larger chain, and ultimately the scalability of your application depends on the entire chain.

_Score: Serverless scalability is good for certain types of applications if configured correctly._

## 2\. Cheap to operate when demand is low

Scaling up to meet surges in traffic is important, but so is operating efficiently when not needed. Ultimately, that’s the biggest benefit of lambda.

With AWS’s current free-tier pricing, lambda is free below 1 million requests or a certain threshold of compute time.

Similarly, the API Gateway free tier includes 1 million HTTP API calls and 750,000 connection minutes per month for up to 12 months.

As before, your other infrastructure in your application may or may not be so cheap.

_Score: Lambda itself is very cheap. Amazon wants you to use it! But the cost of your overall application again depends on what you’re building. Serverless can be cheaper, but in some cases, it might be better to pay for dedicated resources._

## 3\. Infrastructure that we don’t have to think about

For me, this is where serverless starts to break down. Configuration is a big pain, and we didn’t even build anything too complex.

Once you start to use other products with your serverless functions, you’ll have to manage connections, rulesets, and event triggers. Each part of your infrastructure has to be configured to scale with demand but also reject large-scale attacks from bad actors.

I also think about vendor lock-in in this context. In order to do anything interesting with serverless, we have to use other services from AWS. Even for our simple example, we needed to sign up for API Gateway.

It’s true, once you have serverless configured, the challenges of scaling take care of themselves since AWS can handle tons of throughput. Once you have serverless set up, it can be nice.

But to get set up, you’ll need an understanding of AWS and how all the pieces fit together. It’s not trivial to get started. And adding to your application often means changing a lot of your configuration.

_Score: Far from being infrastructure we don’t have to think about, serverless introduces new overhead and DevOps tasks for developers to get their code working in production. If you don’t need that code to change often, then the DevOps investment might be worth it._

# Getting Started With Serverless

The application we built today was super basic, but there are companies using serverless lambdas to run their entire applications.

Indeed, the promise of serverless microservices could be the future of the internet that allows for fast, distributed processing of billions of requests around the world.

Perhaps I’m biased against lambdas or intimidated by the DevOps requirements. The only way for you to find out if serverless is for you is to build something! Hopefully, this article has given you a good start.

# Free Resources

Want my best content on web development and becoming a better programmer?

[Join my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).

[

## Free Email Series for Medium Readers | Bennett Garner

### Join over 500 other developers currently in my email series.

mailchi.mp







](https://mailchi.mp/2e671faffc04/bennett-medium)