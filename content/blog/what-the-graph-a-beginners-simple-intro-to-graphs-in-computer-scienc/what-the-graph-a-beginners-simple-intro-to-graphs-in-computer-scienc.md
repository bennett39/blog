---
title: Don’t Understand Graphs? Here’s Why You Should Study Graphs in Computer Science
date: 2018-12-15T17:49:09.438Z
description: Graphs are everywhere, all around you! But chances are you don’t really understand them.
---



](/what-the-graph-a-beginners-simple-intro-to-graphs-in-computer-science-3808d542a0e5?source=post_page-----3808d542a0e5--------------------------------)

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3808d542a0e5&operation=register&redirect=https%3A%2F%2Fbennettgarner.medium.com%2Fwhat-the-graph-a-beginners-simple-intro-to-graphs-in-computer-science-3808d542a0e5&source=post_actions_header--------------------------bookmark_preview-----------)

_Graphs are everywhere, all around you! But chances are you don’t really understand them._

![](https://miro.medium.com/max/600/1*NxT9a0rJJHZt1vvTXN0Eiw.png?q=20)



A directed, weighted, cyclic graph. A lot of jargon. Don’t worry, I’ll explain.

For many self-taught devs, graphs can be intimidating and difficult to learn.

For that matter, graphs can be baffling to experienced devs and computer science grads who haven’t worked with them for a while.

But graphs are cool and vital ways of representing information and relationships in the world around us. We can use graphs to do amazing stuff with computers, and graph algorithms offer a lot of tools to understand complex networks and relationships.

In this simple post, I’ll expose you to the basics of graphs. Nothing too fancy, complex, or mathematical here. Just the essentials. By the end, I hope you’ll see why they’re worth learning about and playing with.

# A little motivation

Before we dive into the theory, I thought I’d provide some motivation for learning graphs in the first place. What are graphs and what can we do with them?

> At its most basic, a graph is a group of dots connected by lines.

That’s the essential picture you need in your head. All the complicated notation you find in comp sci textbooks (e.g. `G(V, E)`) is simply a way to abstract the concept of dots connected by lines.

We use graphs to model relationships in the world. For example:

*   Google Maps uses a series of dots and lines to model the road network and give you directions to your final destination
*   Facebook friend networks are a graph where each person is a dot, and the friendships between people are lines
*   The Internet is a giant graph, where web pages are dots and the links between pages are lines

We can model objects in physical space, relationships between people, and document structures all using graphs, simple dots and lines!

The upshot is once we have the relationships modeled, we can:

*   Find the shortest path between two points
*   Identify groups of relationships
*   Store data and create links between it in almost any context (think linked lists and trees)

# Nodes/Vertices & Edges

When computer scientists talk about graphs, they don’t use the terms “dots” and “lines.”

Instead, each dot is called a **node** or a **vertex** (plural “vertices”).

Each line is called an **edge** or an **arc**.

By far, the most common combination of these terms is vertex and edge. When you see someone represent a graph with the notation `G(V, E)` it literally means “a graph with vertices and edges.”

# Directed vs. Undirected

![](https://miro.medium.com/max/600/1*t9lHiQpKm_vdLp2Cadog7g.jpeg?q=20)



Directed vs undirected graphs

Google Maps wouldn’t be very useful if its instructions told you to turn the wrong way down a one way street, would it?

Sometimes edges of graphs need to point in a direction. When this is the case, we call it a directed graph. We use arrows when we draw a directed graph so everyone knows what we mean.

Twitter is a directed graph because relationships only go in one direction. You can have lots of followers without needing to follow all of them back.

In contrast, Facebook friends are an undirected graph. When you become friends with someone new, that relationship goes both ways and there’s no directionality to your relationship.

Some terminology to describe the way an edge is pointing:

*   The **head** of an edge is the vertex that the edge is pointing toward
*   The **tail** of an edge is vertex that the edge is pointing away from

# Cyclic vs. Acyclic

If your undirected graph contains a loop where you can follow the edges and return to a point, then you have a cyclic graph.

If your directed graph has a loop where you can follow the edges in the correct direction and return to a point, then that graph is also cyclic.

An acyclic graph, on the other hand, has no loops.

For instance, this graph is acyclic because it has no loops. While the vertices are well-connected, they only go in one direction.

![](https://miro.medium.com/max/600/1*OhRJtkdSyST86_cDTrioIw.png?q=20)



Directed acyclic graph

# Weighted edges

If we want to make our calculations more interesting when finding the shortest path, for instance, we can add weight to the edges of our graph.

![](https://miro.medium.com/max/600/1*FpM--G8JvbV_yMOaIjNChw.jpeg?q=20)



Weighted, undirected, cyclic graph (whew, mouthful of jargon)

Google uses weighting to take into account things like traffic when it gives you directions.

There are all kinds of applications of weights. They might represent strength, distance, difficulty, or desirability. It’s up to you!

# Problems You Can Solve with Graphs

That about covers the basic concepts and jargon you’ll need to know to start learning more about these essential data types in computer science.

So what can you do with them?

There are well-established algorithms for many tasks:

*   Is there a path between two points?
*   Finding the shortest path
*   Finding the best starting point
*   Making the smallest cut (break a graph into two pieces, but snip the fewest edges possible)
*   Breadth-first and depth-first traversal of the entire reachable graph from a given vertex
*   Searching/inserting/deleting from a tree
*   Searching/inserting/deleting from a linked list
*   Find the max flow

These algorithms could help you do things like:

*   Build a Sudoku puzzle solver
*   Settle up debts between friends in the least payments possible
*   Determine efficient delivery routes
*   Make a search engine
*   Create an intelligent online dating app

# Get your graph on!

Chances are if you build anything complex with computers, you’re going to use a graph, whether you know it or not. I hope this simple introduction gives you the basics you need.

This is by no means exhaustive, and PhDs have dedicated their entire lives to studying graphs. In many ways, the field of computer science is the study of graphs.

# Free Resources

Want my best content on web development and becoming a better programmer?

[Join my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).