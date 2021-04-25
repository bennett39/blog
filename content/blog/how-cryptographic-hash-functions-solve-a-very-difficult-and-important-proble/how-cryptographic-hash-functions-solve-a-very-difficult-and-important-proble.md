---
title: How Cryptographic Hash Functions Solve a Very Difficult and Important Problem
date: 2018-02-19T03:47:48.711Z
description: I’m going to give you a very hard logic challenge. Let’s see if you can think of a way to solve it.
---



](/how-cryptographic-hash-functions-solve-a-very-difficult-and-important-problem-b939da3b0185?source=post_page-----b939da3b0185--------------------------------)

![](https://miro.medium.com/max/600/0/1*Wzfp2M1ae6Fh8EGQcOXujQ.png)

I’m going to give you a very hard logic challenge. Let’s see if you can think of a way to solve it.

For security purposes, I need a way to take a string of characters — no matter how long/short — and transform it into another string of random characters of a standard length. The input string can be anything. The output should have the following characteristics:

1.  Standard length — same output length, regardless of the length of the input
2.  Easy to compute — working forward, it should be easy to find the output
3.  Pre-image resistant — working backward, it should be incredibly difficult to discover the input, given the output
4.  Deterministic — inputting the same string should always yield the same output
5.  Collision resistant — it’s incredibly unlikely that two different input strings would generate the same output
6.  Avalanche effect — even a tiny change in the input should yield a completely different output

Go ahead and think about how you might approach solving this problem. It’s worth giving some mental energy to appreciate how tough of a challenge this is.

The problem above is what cryptographic hash functions solve.

# Why is this useful?

While you’re thinking about how you might solve this challenge, let’s take a look at why it’s a problem worth solving.

Imagine I have a file that I’d like to share. However, I’m worried that somewhere along the way between my computer and the recipient someone will try to tamper with the file. Upon receipt, the recipient may not know if the file has been altered. I need an easy way to verify the file’s authenticity and that it hasn’t been changed in any way.

If I have a cryptographic hash function, I can run the file through the function to generate a near-unique output. Then I can share this unique output, the file’s “signature,” with the recipient. Once the recipient receives the file, they can run the same hash algorithm on the file, and it should generate the same output (“deterministic,” see #4 above).

Due to the avalanche effect (see #6 above), if someone were to tamper with the file, it would completely change the file’s hash. The new hash would no longer match the original signature I published, and the recipient would know the file has been changed in some way.

If a bad actor wanted to tamper with a signed file, they would need to change the file in such a way that the new file produces the same hash as the original (“collision,” see #5 above). With modern hashing algorithms, it’s hard to overstate how unlikely this is. In SHA-256, one of the leading modern hash functions, the chances of a collision are 16^64 or 2^256. Even with all the world’s computing power combined, you wouldn’t find a collision in SHA-256 if you left those computers on for longer than the entire history of the universe. In fact, you’d need supercomputing powers and still billions of times the length of the universe before you’re likely to find a collision. This [video](https://www.youtube.com/watch?v=S9JGmA5_unY) does a great job of explaining how unlikely a collision is (in the context of SHA-256 for Bitcoin).

A file along with its cryptographic hash signature means you can be sure the file hasn’t been altered in any way.

# The implications for cybersecurity

Maybe you can see how cryptographic hashing has an important role to play in cybersecurity. It makes it very difficult for attackers to alter files on a network. It immediately becomes apparent that something about the file has changed as soon as you run a hash function on the file and compare that to the original signature.

But that’s not the only application of hash functions for cybersecurity. Blockchain technology uses hashing to secure a decentralized ledger on a peer-to-peer network. It does so by setting a difficult hashing puzzle that computers on the network compete to solve. This puzzle requires computers on the network to do brute force, guess-and-check hashing over and over, changing a nonsense string on the ledger, until they find a hash that is very small (begins with a lot of leading zeroes). By requiring all this computing power be dedicated to hashing, blockchains guarantee that bad actors can’t take over the network.

This isn’t a post about blockchain or proof of work mining, so I won’t go further into detail here. Perhaps a future post will dive into how blockchain security works.

# So how did they solve the challenge?

I set a challenge for you at the beginning of the article. You probably came up with some ideas of how you might approach this problem. However, it’s not likely you figured out an answer (unless you’re a student of cryptography).

The answer I’ll be sharing here comes from the National Security Agency (NSA), the United States intelligence agency that has authority over signals intelligence, transmitting and intercepting state secrets. The NSA’s answer is called the Secure Hashing Algorithm or SHA. SHA has gone through three iterations. The first, SHA-1, is now somewhat outdated and considered less secure. SHA-2 is the current standard for most hashing applications. I’ll explain the basics of SHA-2 in this section. A new framework, SHA-3 works in a completely different way and is less widely adopted than SHA-2, but it could eventually supersede its predecessor.

SHA-2 took ten years for the best minds at the NSA to design and get thorough review from cryptography experts. It is considered the gold standard for hash functions.

I won’t describe all the steps of a SHA-2 hash in detail here. But here’s a high-level overview of how SHA-2 solves the challenges I laid out above…

# 1\. Standard length

Padding a message is standard practice in most types of cryptography, dating back to the earliest secret codes. This is important because messages often begin and end in similar ways. If an interceptor could find a way to decode the beginning of a message, they could learn the entire original message. Padding makes it less clear where messages begin and end.

In the case of SHA-2, padding also helps standardize the length of the output. SHA-2 has different variants, based on the length of output desired. SHA-256, for instance, produces an output of 64 hexadecimal digits, for a total of 256 bits. The input for SHA-256 is twice the output. So the input, when rendered in binary, can be up to 512 bits long.

If the input is less than 512 bits, we’ll add a bunch of zeros to the end of the message until it reaches 512 total bits. (We also generally add a “1” to mark the end of the message and a few digits at the end that tell the length of the original message.)

If the input is longer than 512 bits, we’ll break it into two pieces. The first 512 bits will go into the first round of SHA-256. Then, the remainder of the message will go into sequential rounds of 512 bits, with padding added to reach the next multiple of 512. Since a hash is half as long as its input, we can do multiple rounds of hashing to eventually reduce any length of input down to a single output.

# 2\. Easy to compute

The SHA-256 computations involve basic addition, AND/OR/NOT/etc, and modulo. All of these are standard operations. Anyone who has studied electrical engineering will easily begin to envision the relatively simple circuitry required to accomplish the hashing operations.

Even the slowest modern computers could easily compute thousands of hashes of SHA-256 per second.

# 3\. Pre-image resistant

SHA-256 uses nonlinear ways of manipulating the input to produce the output. Here are a few examples:

*   In addition to the 512 bits of input, SHA-256 adds 1536 bits of nonsense characters to the end of the hashing input. These nonsense characters come from combining and scrambling parts of the input that came prior.
*   Hashing begins with a set of NSA-designated variables A-H (these are derived mathematically and don’t offer the NSA a backdoor on SHA hashes). Every round of SHA-256 involves 64 cycles of hashing, and each round introduces a new constant, K, into the mix, as well (also mathematically derived).
*   The input gets mixed in with scrambled bits of the constants A-H. The scrambling happens in a nonlinear way, so even a small change in the input vastly changes the output after 64 rounds.

# 4\. Deterministic

SHA doesn’t use random numbers or other probability schemes. Its rules are consistent and unchanging. Therefore, the same input will always produce the same output.

# 5\. Collision resistant

This is the difficult thing to prove, and it’s where SHA-2 spent most of its ten years of development. Cryptographers and mathematicians have to verify every step of SHA-256 to make sure it truly produces 2²⁵⁶ equally likely outcomes.

I’m not a math theorist, so I don’t fully understand how this can be proven theoretically. Suffice it to say that very smart people are convinced that SHA-256 meets the criteria of being collision resistant. Full collision resistance was the problem with SHA-1.

# 6\. Avalanche effect

A small change would get reflected in the 1536 nonsense bits added to the end of the input before you even got started hashing, since those bits come from combining and performing a mixing function on the original input.

Additionally, every round of SHA-256 involves 64 cycles. A small change in the input propagates into a completely different result over the course of 64 cycles.

# Conclusion

I hope this article gives some insight into how hashing works and why it’s important. However, this is a high-level overview, and there are a lot of details I left out for the sake of brevity. It’s possible this article raises more questions than it answers.

SHA-2 is only one of many hashing algorithms. So, I didn’t want to get into too fine of detail on how it works specifically. Other hash algorithms accomplish the same result using different methods.

If there’s interest, I can write another guide that goes into greater depth on what’s happening on the hexadecimal and even binary level when conducting a SHA-256 hash.

Cryptographic hash algorithms solve a really interesting puzzle in an ingenious way. They’re amazing and mind boggling in how they create unique outputs for inputs of various lengths. They’re also incredibly useful for authentication, tamper-proofing, and securing peer networks using proof of work.

—

_If you liked this: clap, follow me, and/or get in touch —_ [_bennettgarner.com_](http://bennettgarner.com)