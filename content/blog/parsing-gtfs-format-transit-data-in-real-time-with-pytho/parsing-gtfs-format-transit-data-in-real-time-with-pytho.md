---
title: Parsing GTFS format transit data in real time with Python
date: 2019-04-22T19:07:57.644Z
description: This week I started playing around with real-time transit data from New York City’s Metropolitan Transit Authority. It’s really cool, and…
---



](/parsing-gtfs-format-transit-data-in-real-time-with-python-3a528ba7aab7?source=post_page-----3a528ba7aab7--------------------------------)

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3a528ba7aab7&operation=register&redirect=https%3A%2F%2Fbennettgarner.medium.com%2Fparsing-gtfs-format-transit-data-in-real-time-with-python-3a528ba7aab7&source=post_actions_header--------------------------bookmark_preview-----------)

![](https://miro.medium.com/max/600/1*96KOSrAEDeWPPP9tAZzRRw.jpeg?q=20)



This week I started playing around with real-time transit data from New York City’s Metropolitan Transit Authority. It’s really cool, and quite easy to get your hands on this data. However, getting started with the GTFS data format can be challenging at first.

This is the quick setup guide for Python that I wish I had earlier this week.

# Get yourself an API key

Before we start, you’ll need to decide what GTFS transit data you want to access.

I live in New York, so I’m working with [NYC’s real-time data feeds](https://datamine.mta.info/). Most major cities offer their data and the GTFS format has become a standard.

Check out this awesome list of [public APIs](https://github.com/toddmotto/public-apis#transportation) for ideas of transportation data (or any other type of data) you can use for your projects!

# Install Google’s GTFS Python library

Google has already created a library of GTFS bindings for Python. This took me a little while to find and start using, so I’ll help you out with some setup instructions.

To install:

pip install --upgrade gtfs-realtime-bindings

Now try:

python -m google.transit.gtfs\_realtime\_pb2

If you don’t see an error, you’ve got it installed correctly!

# Install requests if you haven’t already

Most Python developers will already have the requests library installed. It allows you to make HTTP requests from within a Python program.

To install:

pip install --upgrade requests

To check your installation, start a Python shell:

python

Import requests and check your version:

\>>> import requests  
\>>> requests.\_\_version\_\_  
'2.21.0'

# Building a barebones data feed

Now the fun part. Let’s get some data!

Essentially, we need to do three things to start working with real-time transit data:

1.  Initialize the FeedMessage parser from Google
2.  Get the response from the API
3.  Pass the response to the parser

Then we can start working with or storing the data however we want.

# 1\. Initialize an instance of FeedMessage

Google defines a FeedMessage class in its library. We’ll add data to this class later, but right now we just need to initialize it.

feed = gtfs\_realtime\_pb2.FeedMessage()

# 2\. Get the response from the API

Let’s go get the data from the API now that we’ll pass to the FeedParser in step 3.

response = requests.get(<URL OF YOUR GTFS SOURCE>, allow\_redirects=True)

This is the most basic use of the requests library. We allow redirects so the API can route our request.

# 3\. Pass the response to the parser

The FeedMessage class has a ParseFromString() method to read in the data.

feed.ParseFromString(response.content)

The parsed data is now available in the entity attribute:

\>>> len(feed.entity)  
373  
\>>> feed.entity\[0\]  
id: "000001"  
trip\_update {  
  trip {  
    trip\_id: "057150\_1..N03R"  
    start\_date: "20190422"  
    route\_id: "1"  
  }  
  stop\_time\_update {  
    arrival {  
      time: 1555943380  
    }  
    stop\_id: "101N"  
  }  
}

# Use trip\_update

Not every entity in the feed will have a real-time update of transit status. Destinations and departure locations might also be included.

If you want to just focus on data that is updating a currently ongoing revenue trip, then filter for trip\_update using the FeedMessage’s HasField method:

for entity in feed.entity:  
    if entity.HasField('trip\_update'):  
        # Do something

For example, in my data today, only 218 of my 373 entities were actually trip updates:

\>>> len(feed.entity)  
373  
\>>> sum(\[1 for ent in feed.entity if ent.HasField('trip\_update')\])  
218  
\>>> sum(\[1 for ent in feed.entity if not ent.HasField('trip\_update')\])  
155

# Put it all together

Want to see a barebones application to pull GTFS data in action?

[Clone my repo](https://github.com/bennett39/mta-real-time-data.git) and follow the instructions in the README!

## Read more

Like what you’ve read here? I have an [email list](http://eepurl.com/goJwcT) you can subscribe to.