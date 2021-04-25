---
title: Want to Understand Computer Science? Study Abstraction.
date: 2020-02-25T03:34:01.774Z
description: Peel back the layers to become a great developer
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcb785a19bbc5&operation=register&redirect=https%3A%2F%2Flevelup.gitconnected.com%2Fwant-to-understand-computer-science-study-abstraction-cb785a19bbc5&source=post_actions_header--------------------------bookmark_preview-----------)

Most people who are new to coding (and perhaps most developers, period) have a poor understanding of the work that went into the foundation beneath today’s complicated applications.

In this post, I’d like to argue that understanding fundamentals is key to building new things. I’ve argued this point before, in [my most popular (and controversial) Medium post](https://blog.usejournal.com/consider-yourself-a-developer-you-should-solve-the-project-euler-problems-ed8d13397c9c).

![](https://miro.medium.com/max/600/1*lmd-RmzVqCeNgZXGaIOP4Q.jpeg?q=20)



Abstract away the details…

Don’t get me wrong. I’m not telling you that you should write assembly, build a compiler, or design your own OS from scratch.

But I will say that understanding the core concepts of computers and how they work will take you much farther than just understanding a single language, web framework, or library.

> In the day and age of cloud computing, interpreted languages, web frameworks, serverless, and big data, understanding the fundamentals is less common and more valuable than ever.

# Why You Should Care

You might say “That low level work is already done, so why should I care? Somebody else already did all the hard stuff.”

In one sense you’re right. You can write a program that works in JavaScript or Python without any need to understand the implementations and layers you’re building on top of.

In fact, you can have a whole career as a software developer where you never worry about implementation details.

> This is true precisely because the people who went before you were so good at their jobs and refined their work so well.

But here’s the kicker: understanding the fundamentals is the key to **fast learning** and **longevity** in your career as a software engineer.

Languages, frameworks, libraries, and applications come and go. The fundamentals are first principles — here to stay.

# The Magic Question: What If I Go One Level Deeper?

Take a modern, interpreted programming language like Python. Ask 10 Python developers how Python actually executes your code and you’re likely to get 10 different responses. Most of them wrong or incomplete.

I won’t answer the questions of how CPython’s interpreter works, here. For one, I’m not anywhere near an expert on the subject. But mostly, I won’t answer the question to make a point: abstraction is so critical to everything we do as software developers.

If you’re working as a software developer and you’re not writing byte code on punch cards, then you have abstraction (and the work of decades of other programmers) to thank.

This trend won’t end soon. Abstraction continues. The current cutting edge of abstraction is serverless computing, “no code” software, single page applications, and even machine learning.

In the future, who knows what will be abstracted? But if you want to be prepared for it, the best place to start is by studying the layers of abstraction that are beneath the things you’re currently building.

Ask yourself:

> Do I understand what happens one layer deeper?

# Brief & Selected Examples of Abstraction in Computer Science

## Bytes to Assembly

![](https://miro.medium.com/max/600/1*PmW78VeRrQhXZe6h8JJ86w.jpeg?q=20)



Early computers used punch cards for programmers to give instructions

It’s VERY difficult to write binary or bytes-based instructions for computers, but that’s how it was done at first. The earliest computer programs (after the ones that were hard-wired) were literally pieces of paper with holes punched in them.

Early computer scientists realized this was terribly inefficient and frustrating. They developed assembly language to talk to their machines, instead of punch cards:

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*  
;  
; AddNumbers  
; add number 2 at ds:si to number 1 at es:di of width cx  
;   
;  
; INPUT:     es:di ==> number1, ds:si ==> number2, cx= max width  
; OUTPUT:    CF set on overflow  
; DESTROYED: ax, si, di  
;  
;\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*  
AddNumbers:  
 std   ; go from LSB to MSB  
 clc   ;  
 pushf   ; save carry flag  
.top  
 mov ax,0f0fh ; convert from ASCII BCD to BCD  
 and   al,\[si\]  ; get next digit of number2 in al  
 and ah,\[di\]  ; get next digit of number1 in ah  
 popf   ; recall carry flag  
 adc al,ah  ; add these digits  
 aaa   ; convert to BCD  
 pushf   ;  
 add al,'0'  ; convert back to ASCII BCD digit  
 stosb   ; save it and increment both counters  
 dec si  ;  
 loop .top  ; keep going until we've got them all  
 popf   ; recall carry flag  
 ret   ;

## Assembly to Compiled

This was much better than punch cards! But can abstraction help us further?

What if we built a language with a better syntax and then layered that on top of assembly? Then we wouldn’t have to use assembly’s complicated syntax and we could add guard rails and type checking for development help.

If we built a compiler for our new syntax we could get a lot more flexibility while still getting the performance of lower level code. Compile it once, run it infinite times!

#include <stdio.h>  
int main() {  
    int t1 = 0, t2 = 1, nextTerm = 0, n;  
    printf("Enter a positive number: ");  
    scanf("%d", &n);// displays the first two terms which is always 0 and 1  
    printf("Fibonacci Series: %d, %d, ", t1, t2);  
    nextTerm = t1 + t2; while (nextTerm <= n) {  
        printf("%d, ", nextTerm);  
        t1 = t2;  
        t2 = nextTerm;  
        nextTerm = t1 + t2;  
    }return 0;  
}

## Compiled to Interpreted

Plenty of applications still use compiled languages today, but recently interpreted languages (and their further level of abstraction) are on the rise.

Interpreted languages don’t need to be compiled because they have an underlying program that evaluates the code on the fly. This underlying program is usually written in a compiled language, which in turn is written in assembly/bytecode.

This new layer gives us even greater flexibility to write programs quickly:

def fibonacci(n):  
    if n < 2:  
        return 1  
    return fibonacci(n-1) + fibonacci(n-2)

## Local to Servers

It didn’t take long for folks to realize that it would be really handy if multiple computers could share access to the same files

It’d also be cool if there were standard ways to transfer files between computers.

So, we added a layer of abstraction. Files can exist in shared drives but still be available to your computer.

All kinds of technologies like NFS and FTP made this possible. All built by regular people like you and me.

## Individual Servers to the Internet

The Internet is just another abstraction of existing ideas. If you can create a local network, then you should theoretically be able to create a global one.

The internet was just the protocol for linking and sharing files. To share those files, you just need a connection and a server to handle incoming requests.

TCP/IP, SMTP/POP, and more came to help files move across the connected internet.

Browsers came about to help dynamically display Hypertext Markup documents (HTML) and other media files. The internet as we know it is one big abstraction of the bits and bytes on processors, RAM, and hard drives all over the world!

## Privately-Owned to Cloud

![](https://miro.medium.com/max/600/1*lOqD_NHM7gQY9Ax9TF4wZg.jpeg?q=20)



Most of today’s internet lives in massive buildings owned by Amazon, Google, & Microsoft full of servers

It used to be that if you wanted a website or file storage, you had to buy your own physical equipment. You’d have to set up hard drives and servers for yourself and expand your equipment as your needs grew.

Today, however, most companies don’t have much physical IT infrastructure at all (besides employee laptops and wifi networks). This is thanks to another layer of abstraction: the cloud.

AWS, GCP, Azure, Dropbox, and others have made it easier to store files, host applications, and do nearly anything on their servers than it is to build and use your own infrastructure.

On-demand infrastructure is what allowed startups like Uber, Airbnb, and Netflix to run cheaply in their early years and then scale quickly as demand grows.

# Abstraction Is the Past & Future of Computer Science

As you can see, the entire history of software as we know it is all about abstraction and how much more powerful it makes computers.

*   It’s mindboggling how much work went into building assembly language.
*   Which is used in the equally mindboggling C compiler.
*   Which is used to run the complex, dynamic Python language and all its libraries.
*   Which is used to create Flask, a fully-featured web server.
*   Which powers major applications for companies like Airbnb, Uber, and Netflix.
*   Who host their infrastructure on complexly-architected cloud servers that connect to a global internet.

It’s so crazy and cool! There are so many layers and so many people to thank for the current state of software!

# Final Take

So, here’s my hot take:

> If you’re not interested in layers of abstraction, you’re not doing real software engineering — you’re just writing some syntax.

Writing code does not make you a software engineer. Understanding and being thoughtful of the whole context and system that your code is a part of is the key. Even better, build things that make future problems easier — that’s abstraction at its finest.

What does this mean for you? Two things:

1.  If you want to be a great developer, understand the work that went before you so you know what high-value abstraction looks like
2.  To stay up to date and gainfully employed as a developer, follow the trend of abstraction wherever it’s headed next

## About Bennett

I’m a software developer in New York City. I do web stuff in Python and JavaScript.

Like what you’ve read here? I have an [email list](https://mailchi.mp/2e671faffc04/bennett-medium) you can subscribe to. Infrequent emails, only valuable content, no time wasters. I’d love to have you there.

*   **📧** [**Get an email when I publish something new**](https://mailchi.mp/2e671faffc04/bennett-medium) **📧**