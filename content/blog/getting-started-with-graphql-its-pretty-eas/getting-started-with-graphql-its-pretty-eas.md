---
title: Getting Started with GraphQL - It’s pretty easy!
date: 2019-05-24T17:06:17.373Z
description: Many companies have switched over to GraphQL to build their APIs. There’s good reason. It’s a new way of thinking about fetching data.
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3ea803426298&operation=register&redirect=https%3A%2F%2Flevelup.gitconnected.com%2Fgetting-started-with-graphql-its-pretty-easy-3ea803426298&source=post_actions_header--------------------------bookmark_preview-----------)

Many companies have switched over to GraphQL to build their APIs. There’s good reason — it’s a revolutionary way of thinking about how we fetch data.

![](https://miro.medium.com/max/600/1*yzuJnF3vENKW9BEPTBp65Q.png?q=20)



# GraphQL’s origins & why use it

GraphQL comes from Facebook. Internally, Facebook was looking for a way to make their newsfeed load more reliably on mobile.

Using a traditional REST API structure, the newsfeed was making many calls to multiple API endpoints in order to get all the data it needed. Along the way, the API calls were also overfetching extra data that the newsfeed didn’t need. Additionally, upon receipt, the frontend engineers still had to parse through the data to find the fields they wanted.

Facebook engineers wondered, “What if we could write a query language so that we can specify all the information we need in a single API request?”

GraphQL is the result of that effort. It maps the relationships between objects in your database — creating a [graph](https://medium.com/@BennettGarner/what-the-graph-a-beginners-simple-intro-to-graphs-in-computer-science-3808d542a0e5). Then they designed a query language for traversing that map of relationships. Hence, the name GraphQL.

With the addition of a query language, GraphQL APIs can now accept all incoming requests at a single endpoint. They then fetch and return the data you requested, and only the data you requested. No more overfetching information that you won’t use.

# A specification, not an implementation

Critically, Facebook decided to open source GraphQL as a specification.

That means that it can be implemented in any programming language. As long as the implementation parses queries, schema, etc in the specified way, it will play nice with any other GraphQL application.

Indeed, there are now dozens of implementations of GraphQL in every major programming language.

In this article, we’ll use the reference implementation of GraphQL that’s written in JavaScript, but the same basic principles apply in any language.

You can check out the full [list of GraphQL implementations](https://graphql.github.io/code/) to find your favorite language.

# Basic architecture

Developing a functioning GraphQL API requires two components: a server and a client.

1.  The server handles incoming queries, parses those queries, fetches the data using defined schema, and returns a response usually in JSON.
2.  The client makes it possible for your application to communicate with the server. While you can just send a plain POST request to a GraphQL endpoint, you get much greater functionality if you use a GraphQL client to help send your queries.

Building a GraphQL API can be more intensive than [building a REST API](https://medium.com/@BennettGarner/build-your-first-rest-api-with-django-rest-framework-e394e39a482c). However, the benefits in speed and usability may make up for it in complex or high-performance applications.

# What GraphQL looks like

Our goal for our API is to send a GraphQL query and get back a response. So, let’s see what that might look like.

Remember, GraphQL is its own language. It’s not a difficult language to learn, and for the most part writing queries is very intuitive.

Let’s imagine we have a database that has flight and passenger information.

In GraphQL, we might query a flight like this:

{  
  flight(id: "1234") {  
    origin  
    destination  
  }  
}

That’s GraphQL’s way of saying, “give me the origin and destination of flight 1234”

In response, we’ll receive:

{  
  "data": {  
    "flight": {  
      "origin": "DFW",  
      "destination": "MKE"  
    }  
  }  
}

Notice:

*   We receive back exactly what we asked for — nothing more, nothing less.
*   We also receive the response in the exact same format as the original query we sent.

These are the hallmarks of a GraphQL API. It’s what makes GraphQL so fast and powerful.

That’s not all we can do, though. Let’s say we want to get passenger information on the flight:

{  
  flight(id: "1234") {  
    origin  
    destination  
    passengers {  
      name  
    }  
  }  
}

Now, GraphQL will traverse the graph of relationships between this flight and its passengers. We’ll get a list of passengers in return:

{  
  "data": {  
    "flight": {  
      "origin": "DFW",  
      "destination": "MKE",  
      "passengers": \[  
        {  
          "name": "Luke Skywalker"  
        },  
        {  
          "name": "Han Solo"  
        },  
        {  
          "name": "R2-D2"  
        }  
      \]  
    }  
  }  
}

Cool, so now we can immediately see all the passengers on this flight with a single API call.

Why Han, Luke, and R2 are flying domestic is a bigger question, but I hear Milwaukee is lovely this time of year.

Because GraphQL interprets data as a graph, we can traverse it the other direction, too.

{  
  person(name: "Luke Skywalker") {  
    passport\_number  
    flights {  
      id  
      date  
      origin  
      destination  
   }  
}

And now we can see what flights Luke has booked:

{  
  "data": {  
    "person": {  
      "passport\_number": 78120935,  
      "flights": \[  
        {  
          "id": "1234",  
          "date": "2019-05-24",  
          "origin": "DFW",  
          "destination": "MKE"  
        },  
        {  
          "id": "2621",  
          "date": "2019-07-05",  
          "origin": "MKE",  
          "destination": "DFW"  
        }  
      \]  
    }  
  }  
}

Wow, he’s going to be in Milwaukee for more than a month! I wonder what he’s doing there?

# To-do list

So, what do we need to create a GraphQL API?

1.  Pick a framework to implement your GraphQL server. We’ll use Express.
2.  Define schema so GraphQL knows how to route incoming queries
3.  Create resolver functions that handle queries and tell GraphQL what to return
4.  Construct an endpoint
5.  Write a client-side query that fetches data

Then, you can use the GraphQL queries to power client applications. This tutorial won’t get into all the different ways you can use GraphQL on the frontend, but it integrates well with all modern frontend frameworks.

Ultimately, most uses of GraphQL will involve talking with a database. In this tutorial, we won’t cover adding a database to Express and allowing GraphQL to query and update that database. That’s the subject of an entirely different tutorial.

# 1\. Implement a server

First we need to lay the groundwork for our API.

You’ll need [nodejs and npm installed](https://nodejs.org/en/download/) to follow along with this tutorial from here on out.

Let’s build a barebones Express server. Start by intializing npm:

$ npm initThis utility will walk you through creating a package.json file.  
It only covers the most common items, and tries to guess sensible defaults.See \`npm help json\` for definitive documentation on these fields  
and exactly what they do.Use \`npm install <pkg>\` afterwards to install a package and  
save it as a dependency in the package.json file.Press ^C at any time to quit.  
package name: (graphql-medium)   
version: (1.0.0)   
description:   
entry point: (index.js)   
test command:   
git repository:   
keywords:   
author:   
license: (ISC)   
About to write to /home/bennett/Repos/graphql-medium/package.json:{  
  "name": "graphql-medium",  
  "version": "1.0.0",  
  "description": "",  
  "main": "index.js",  
  "scripts": {  
    "test": "echo \\"Error: no test specified\\" && exit 1"  
  },  
  "author": "",  
  "license": "ISC"  
}Is this OK? (yes)

Just hit enter to skip through the initialization process. You can go back and edit your `package.json` later if you want.

Next, let’s install Express, GraphQL, and Express-GraphQL library:

$ npm install express express-graphql graphqlnpm notice created a lockfile as package-lock.json. You should commit this file.  
npm WARN graphql-medium@1.0.0 No description  
npm WARN graphql-medium@1.0.0 No repository field.\+ express-graphql@0.8.0  
\+ graphql@14.3.1  
\+ express@4.17.0  
added 53 packages from 38 contributors and audited 151 packages in 6.169s  
found 0 vulnerabilities

Now, we’ll create a new file called `index.js` and create a new barebones Express server there:

// index.jsconst express = require('express');  
const app = express();app.get('/', function(req, res) {  
  res.send('Express is working!')  
});app.listen(4000, function() {  
  console.log('Listening on port 4000')  
});

Try running `node index.js`. You should see a message “Listening on port 4000” and if you visit [http://localhost:4000/](http://localhost:4000/) then you’ll see “Express is working!”

# 2\. Add in GraphQL & Define Schema

We already installed the GraphQL npm package. Now, let’s use it.

First, we need to import the necessary building blocks:

const graphqlHTTP = require('express-graphql');  
const { buildSchema } = require('graphql');

Next, we’ll use those building blocks.

Let’s start by defining the schema of our GraphQL API. What should an incoming query look like?

For now, let’s just define a hello world schema to get things working:

let schema = buildSchema(\`  
  type Query {  
    hello: String  
  }  
\`);

This simple schema lets GraphQL know that when someone sends a query for “hello”, we’re going to end up returning a string.

Notice those little backticks (\`) in there. Those indicate we’re using a JavaScript template literal. Basically, we use those backticks to tell JavaScript that we’re about to write in a different language — the GraphQL query language.

# 3\. Resolving queries

So, when someone submits a query for `hello` we know we’re going to end up returning a string. That’s defined in our schema.

Now, we need to tell GraphQL exactly what string it should return.

Figuring out what data to return based on an incoming query is the job of a “resolver” in GraphQL.

In this example, the resolver is simple. We’re literally going to return the string “Hello world”

return 'Hello world!';

However, we need to wrap that return statement inside a function that can get called multiple times, any time someone makes a query for hello:

function() {  
  return 'Hello world!';  
}

Now, `hello` might not be the only query type we implement. In the future, we might also include “endpoints” for other functionality. So, we should make sure this function we just created is mapped to hello and saved in an object alongside all the other resolvers for our API.

let root = {  
  hello: function() {  
    return 'Hello world!';  
  },  
}

It’s convention to call the object that hold all the resolvers `root`, but you can call it whatever you want.

# 4\. Setting up an endpoint

Astute readers will notice that we imported `graphqlHTTP` in step 2 but we haven’t used it yet. Now is the time.

We’ve now got everything in place for our GraphQL server. We just need to make it available via a URL endpoint.

In Express, we’ll create a new route to serve up the GraphQL api:

app.use('/graphql', graphqlHTTP({  
  schema: schema,  
  rootValue: root,  
  graphiql: true,  
}));

Schema and root point to the variables we defined in steps 2 & 3.

graphiql is a useful visual tool that installs along with GraphQL. As we’ll see in a second, it makes it easy to test out how your API is working.

Here’s the final [state of our source code](https://github.com/bennett39/graphql-medium/blob/master/index.js) for our GraphQL server.

# 5\. Run it & write a query

We’re ready to test it out!

1.  Start the app with `npm index.js`
2.  Go to [http://localhost:4000/graphql](http://localhost:4000/graphql?)

You should see the graphiql interface:

![](https://miro.medium.com/max/600/1*Ljwn539rzdaPvucdt733Bg.png?q=20)



We can now use this interface to make sure our API is working!

Let’s write a query. This one is going to be super simple. We always wrap our GraphQL queries in curly braces. Then, we specify the schema object we’re querying followed by any attributes we want to fetch.

In this case, there’s only one thing in our API to fetch so far:

{  
  hello  
}

If you click the submit button, you’ll see:

{  
  "data": {  
    "hello": "Hello world!"  
  }  
}

It’s working!

# Adding more endpoints

Adding endpoints to your API is as simple as defining new fields in your schema and then adding a resolver function to `root`

You can gradually get more complex with your queries as well. I recommend this guide on [building a dice rolling API](https://graphql.org/graphql-js/passing-arguments/) from the official docs as your next step.

# GraphQL, FTW

GraphQL is awesome and growing rapidly in adoption. It has the potential to become a ubiquitous technology for APIs over the coming years.

Hopefully this guide gave you a good introduction to how and why you can use GraphQL in your projects.

Share your thoughts in the comments below! I read every reply.

## Free Resources

Want my best content on web development and becoming a better programmer?

[Join my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).