---
title: Getting started with Redis - It’s easier than you think
date: 2019-05-16T18:24:27.197Z
description: Redis is a data store that offers performance benefits over traditional databases. However, far from being a replacement for databases…
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F23d3729230fa&operation=register&redirect=https%3A%2F%2Flevelup.gitconnected.com%2Fgetting-started-with-redis-its-easier-than-you-think-23d3729230fa&source=post_actions_header--------------------------bookmark_preview-----------)

![](https://miro.medium.com/max/600/1*77Vo1RFQ-5DcLKdeHbb2-A.png?q=20)



Redis is a data store that offers performance benefits over traditional databases. However, far from being a replacement for databases, it’s a small, fast, easy-to-use tool to supplement your existing application.

In this quick post, we’ll break down what Redis is and how it works. More importantly, we’ll also see what Redis is not so that you can make good decisions about how to use it. Along the way, we’ll also introduce some example code so you can get a feel for how it works.

# Redis Architecture

Redis has two key features that make it an ideal solution for some software challenges:

1.  Redis is a key-value data store. This means that keys are hashed, and as a result we can look up data in constant — O(1) — time. Instead of querying a database, we just ask Redis if the key exists. If it does, then Redis returns the value.
2.  Redis operates in-memory. On a hardware level, Redis is saving and manipulating data in RAM instead of on disk. If you’ve studied computer science, you know that operations from RAM are significantly faster than read/writes to disk.

Of course, the fact that Redis operates in-memory has consequences for data persistence. So, Redis does have the ability to save a current snapshot of data to disk as well.

# What Redis architecture means practically

These architecture decisions behind Redis lead to interesting consequences:

Pros:

*   Redis is ridiculously fast compared to databases — like 100,000 reads/writes per second
*   Redis implements highly efficient data structures for manipulating data without needing to write SQL-like queries
*   Because it’s so simple, Redis has very little overhead. It’s a small application at ~16,000 lines of C code

Cons:

*   Since it’s on RAM, data persistence can be an issue, so you’ll need to instruct Redis to save a snapshot to disk if you make critical changes
*   Redis has limited ability to create relationships between data objects — it’s not a replacement for a relational (e.g. MySQL) or document-based (e.g. MongoDB) database
*   Clustering and key management in production applications can be a challenge without the right rules in place

# What can I use Redis for?

Redis has a host of potential applications for scenarios where you need quick access to data:

*   Cache the results of an expensive database query across different pages when you know that data is not likely to change significantly
*   Cache recent changes/updates/posts/events to power a “Recently added” section to your website that updates dynamically without needing to query the whole database like `SELECT * FROM posts WHERE ... ORDER BY time DESC LIMIT 10;`
*   Cache and count new scores/prices/impressions in a leaderboard to power a “Top Scores” or “Top Stories” section of your website, by using Redis’s increment commands to count impressions or add a key to a set if it’s unique and its score exceeds a certain threshold
*   Create a publish-subscribe model where Redis natively manages the pushing of new events to subscribers
*   Create and maintain an independent message queue for passing data between services in your application since Redis lists can be pushed and popped from either the head or the tail in constant time
*   Cache a user’s session data across various pages on your site, so you don’t need to re-query the database for user information that’s not likely to change

# What does Redis code look like?

Redis has tons of features — too many for me to cover in depth here. However, here are the basic data types in Redis:

## Strings

These are the basic key-value pairs in Redis. Set the value of a given key and access/update it later.

127.0.0.1:6379> SET coffees\_drunk 0  
OK  
127.0.0.1:6379> INCR coffees\_drunk  
(integer) 1  
127.0.0.1:6379> INCR coffees\_drunk  
(integer) 2  
127.0.0.1:6379> INCR coffees\_drunk  
(integer) 3  
127.0.0.1:6379> GET coffees\_drunk  
"3"

## Lists

Most programming languages implement lists as a stack. So, you can append to the end of the list and you can pop the last thing in the list. But if you want to get the first item in the list (queue-like behavior), then that happens in O(n) time because the whole list needs to get shifted back.

Since Redis is written in C, they can do some memory management that makes push/pop on both the left and right of the list in O(1) time.

127.0.0.1:6379> LPUSH mylist "apple"  
(integer) 1  
127.0.0.1:6379> LPUSH mylist "banana"  
(integer) 2  
127.0.0.1:6379> RPUSH mylist "carrot"  
(integer) 3  
127.0.0.1:6379> LRANGE mylist 0 -1  
1) "banana"  
2) "apple"  
3) "carrot"

## Sets

Sets are unordered groups of data where all the items in the set are unique. Redis also allows you to compare and combine sets.

127.0.0.1:6379> SADD cars "Accord"  
(integer) 1  
127.0.0.1:6379> SADD cars "Camry"  
(integer) 1  
127.0.0.1:6379> SADD cars "Focus"  
(integer) 1  
127.0.0.1:6379> SISMEMBER cars "Camry"  
(integer) 1  
127.0.0.1:6379> SISMEMBER cars "Jetta"  
(integer) 0  
127.0.0.1:6379> SMEMBERS cars  
1) "Accord"  
2) "Camry"  
3) "Focus"

## Ordered Sets

There are scenarios where you want your data to be grouped and unique (like a set), but order is also important. Redis ordered sets are a bit more complicated, but they’re very powerful when you need them.

Like presidents ordered by birth year, for example:

127.0.0.1:6379> ZADD presidents 1732 "washington"  
(integer) 1  
127.0.0.1:6379> ZADD presidents 1743 "jefferson"  
(integer) 1  
127.0.0.1:6379> ZADD presidents 1917 "kennedy"  
(integer) 1  
127.0.0.1:6379> ZADD presidents 1809 "lincoln"  
(integer) 1  
127.0.0.1:6379> ZADD presidents 1856 "wilson"  
(integer) 1  
127.0.0.1:6379> ZRANGE presidents 0 -1  
1) "washington"  
2) "jefferson"  
3) "lincoln"  
4) "wilson"  
5) "kennedy"

## Hashes

Hashes allow you to treat keys like objects. Each key now stores a set of related information. For instance, you could create a `user` key that has information on the user’s username, email, name, address, etc.

127.0.0.1:6379> HSET superhero:1 name "Superman"  
(integer) 1  
127.0.0.1:6379> HSET superhero:1 power "Strength"  
(integer) 1  
127.0.0.1:6379> HSET superhero:1 aka "Clark Kent"  
(integer) 1  
127.0.0.1:6379> HMSET superhero:2 name "Flash" power "Speed" aka "Barry Allen"  
OK  
127.0.0.1:6379> HMSET superhero:3 name "Batman" power "Wealth" aka "Bruce Wayne"  
OK  
127.0.0.1:6379> HGET superhero:1 name  
"Superman"  
127.0.0.1:6379> HGETALL superhero:2  
1) "name"  
2) "Flash"  
3) "power"  
4) "Speed"  
5) "aka"  
6) "Barry Allen"

# Learning the basics of Redis

Redis has a fun way to introduce you to the syntax. There’s an [online interactive tutorial](https://try.redis.io/) that walks you through all the basics.

# Installing Redis locally

Ready to start playing with Redis yourself?

$ wget [http://download.redis.io/releases/redis-5.0.5.tar.gz](http://download.redis.io/releases/redis-5.0.5.tar.gz)  
$ tar xzf redis-5.0.5.tar.gz  
$ cd redis-5.0.5  
$ make

_NB: This is for the current version of Redis as of writing. If you’re reading this later, you should really visit the_ [_Redis download page_](https://redis.io/download) _to get the latest version._

# Using Redis in your application

For most application frameworks, it’s super easy to get Redis working within your application. Here are some of the most common:

*   Spring Boot: [Spring Data Redis](https://docs.spring.io/spring-data/data-redis/docs/current/reference/html/)
*   Rails: [redis-rails](https://github.com/redis-store/redis-rails)
*   Django: [django-redis](https://github.com/niwinz/django-redis)
*   Laravel: [predis](https://laravel.com/docs/5.8/redis)
*   NodeJS: [node\_redis](https://github.com/NodeRedis/node_redis)

Of course, it’s not just web applications that can use Redis. Any application might benefit from a Redis server. There are clients for all major languages. A [complete list of clients](https://redis.io/clients) is on the Redis website.

# Take your application to the next level

Redis is an amazing tool for improving the performance of your application. When you cache data or keep recent entries/counters in memory, you can significantly decrease load times for users.

The best part? It’s not that hard to get started! You can use Redis alongside your existing tools with zero drawbacks. Try it out!

## About Bennett

I’m a software developer in New York City. I do web stuff in Python and JavaScript.

Like what you’ve read here? I have an [email list](https://mailchi.mp/2e671faffc04/bennett-medium) you can subscribe to. Infrequent emails, only valuable content, no time wasters. I’d love to have you there.