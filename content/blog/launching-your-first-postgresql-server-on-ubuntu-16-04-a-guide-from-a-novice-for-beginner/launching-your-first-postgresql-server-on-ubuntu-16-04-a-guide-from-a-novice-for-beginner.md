---
title: Launching your first PostgreSQL server on Ubuntu 16.04 — a guide from a novice, for beginners
date: 2018-11-07T18:56:05.365Z
description: I had a difficult time launching my very first instance of PostgreSQL on my local machine. Hopefully, this post can help other newbies in…
---



](/launching-your-first-postgresql-server-on-ubuntu-16-04-a-guide-from-a-novice-for-beginners-89e6076848c0?source=post_page-----89e6076848c0--------------------------------)

![](https://miro.medium.com/max/600/0/1*WL6OacV-vfDVoPVFz73INQ.png)

I had a difficult time launching PostgreSQL on my local machine.

With Postgres, you have to run a server that handles database requests. It’s not the same as your web server that routes web pages. It’s a dedicated Postgres server that makes your databases available to your web application.

This guide will walk you through setting up a local PostgreSQL server on your Linux (Ubuntu 16.04, specifically) computer. I’m mostly writing this guide so that I have these steps to refer back to in the future. This is the guide I wish I could have found on the internet, instead of spending time troubleshooting each problem myself.

# Step 1. Update your system

Check to make sure your environment is up to date with:

> **_sudo apt-get update && sudo apt-get upgrade_**

# Troubleshooting 1.1

At this point, I already had an issue. You might not, but I did. When I tried to update my system I got the following error:

> `**_An error occurred during the signature verification. The repository is not updated and the previous index files will be used. GPG error: http://apt.postgresql.org/pub/repos/apt xenial-pgdg InRelease: The following signatures couldn't be verified because the public key is not available: NOPUBKEY 7FCC7D46ACCC4CF8_**`

This error probably occurred because I installed PostgreSQL incorrectly the first time (don’t ask…) and had to remove it. The solution was to get a new public key from the Ubuntu key server:

> **_sudo apt-key adv — recv-keys — keyserver keyserver.ubuntu.com 7FCC7D46ACCC4CF8_**

# Step 2. Install PostgreSQL via apt-get

This applies to users installing on Ubuntu/Debian Linux. If you’re using some other distro, you’ll probably want to find another guide.

Following the PostgreSQL docs and several guides I found online, I installed PostgreSQL and PostgreSQL-contrib via apt-get:

> **_sudo apt-get install postgresql postgresql-contrib_**

# Step 3. Meet the postgres user on your computer

As part of the installation, PostgreSQL makes itself at home on your computer and creates itself a user. By default, this user is called `postgres`.

When you want to do something in Postgres, you’ll probably want to switch to that user on your computer, since `postgres`owns all the files that the install script created.

The `postgres` user is the one in charge of everything Postgres on your machine. So, switch to that user:

> **_sudo -i -u postgres_**

# Step 4. If you’re lucky, start psql

So, most tutorials on the internet make it seem like you’re home free at this point. They say, just log in as the `postgres` user and then type:

> **_psql_**

And you’ll get an interactive screen and a prompt for issuing SQL commands to your PostgreSQL database. If that happens for you, amazing!

That’s not what happened for me.

# Troubleshooting 4.1.

I got an error when I typed `psql` for the first time:

> `**_psql: could not connect to server: No such file or directory Is the server running locally and accepting connections on Unix domain socket "/var/run/postgresql/.s.PGSQL.5432"?_**`

The issue is kind of obvious — the server for PostgreSQL isn’t running yet! Sure, I installed the package and logged in as `postgres`, but I didn’t boot up the postgres server at all.

Typically, the installer should start Postgres automatically. But because I have a unique setup (I work on a chromebook running Linux as a chroot), I need to start the Postgres server manually on my machine.

Now, I needed a command to boot the server. First, I found `pg_ctl`. And that is indeed the command to start a server. I even read the whole man page for it to make sure I understood how it worked. But when I typed it into the command line I was told that command didn’t exist.

Stack Overflow to the rescue! A commenter on a post about `pg_ctl` mentions that you should really use `pg_ctlcluster` to start a server, as it wraps the `pg_ctl` command. From the man page:

> `**_pg_ctlcluster_**` **_determines the cluster version and data path and calls the right version of_** `**_pg_ctl_**` **_with appropriate configuration parameters and paths._**

Read the rest of the man page and you’ll find the correct syntax for starting a local server:

> **_pg\_ctlcluster \[version\] \[name\] \[action\]_**

In our case, we want to start a version 11 server and name it main, so:

> **_pg\_ctlcluster 11 main start_**

> 07/26/19 update: I’ve found that \`$ sudo /etc/init.d/postgresql restart\` will also do the trick in my particular setup

# Step 5. Test it out

If all goes well (and believe me, I know how much room there is for it to go poorly), you’ll have started a PostgreSQL server locally on your Ubuntu machine that you can then access using the `postgres` username by issuing the command `psql`.

Jeez, what a mouthful.

So, does it work? Let’s try it:

> **_(xenial)postgres@localhost:~$ psql -l_**
> 
> **_List of databases_**
> 
> **_Name | Owner | Encoding | Collate | Ctype | Access privileges_**
> 
> **_postgres | postgres | UTF8 | C.UTF-8 | C.UTF-8 |_**
> 
> **_template0 | postgres | UTF8 | C.UTF-8 | C.UTF-8 | =c/postgres + | | | | | postgres=CTc/postgres_**
> 
> **_template1 | postgres | UTF8 | C.UTF-8 | C.UTF-8 | =c/postgres + | | | | | postgres=CTc/postgres_**
> 
> **_(3 rows)_**

**You have no idea how happy I was when I finally saw that ^^^ table after all that troubleshooting.**

# Conclusion

You can now use Postgres to do all kinds of stuff. Your machine should now be set up to play nice with any PostgreSQL tutorial you find online. Happy querying!!

## Free Resources

A lot has changed since I wrote this post. I’m now a full-time backend web developer and a top writer on Medium.

Want my best content on web development and becoming a better programmer?

[Join my free email series](https://mailchi.mp/2e671faffc04/bennett-medium).