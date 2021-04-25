---
title: Developers - Don’t Make These Top 10 Security Mistakes in Your Applications
date: 2020-06-09T13:42:52.348Z
description: What I learned as a developer from the world’s foremost organization dedicated to web security…
---



[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff568b881f406&operation=register&redirect=https%3A%2F%2Fmedium.com%2Fswlh%2Ftop-10-web-app-security-risks-how-to-stop-them-according-to-the-experts-at-owasp-f568b881f406&source=post_actions_header--------------------------bookmark_preview-----------)

![](https://miro.medium.com/max/600/1*v89LGRBm9fPHQwCKnSaZvA.jpeg?q=20)



As a developer, you’re the first line of defense against data breaches. You should know what to look out for, and you have a responsibility to your users to follow best practices.

Luckily, there’s an organization dedicated to providing you with up-to-date guidelines for how to secure your web applications. Every web developer should know about the OWASP Top Ten.

# The OWASP Top Ten Application Security Risks

The Open Web Application Security Project (OWASP) is a nonprofit dedicated to promoting security on the web. They’re an awesome organization, and they do a lot of research into the threats and exploits facing modern applications.

According to the experts:

> Using the OWASP Top 10 is perhaps the most effective first step towards changing the software development culture within your organization into one that produces more secure code.

Following OWASP’s recommendations is the gold standard for security. If you’re a web developer, you need to know about OWASP and understand their top recommendations.

So, what are the biggest threats to your application?

## 1\. Injection

Websites need to accept data from their users. They wouldn’t be very useful otherwise.

However, before you do anything with that data (store it, execute code on it, use it to look something up, etc), you need to make sure it’s cleaned and escaped of special characters.

If you don’t, attackers can potentially run their own code on your servers.

![](https://miro.medium.com/max/600/1*P4nj9fJjSeJ9-c0rwSZqlg.png?q=20)



A mom exploits a SQL injection against her son’s school

The best way to prevent injection is to use a library that sanitizes user entered data every time and as soon as user data hits your server. Every programming language for the web has tools & libraries to help sanitize inputs.

This is the #1 security risk because it’s the most severe (allows an attacker to run arbitrary code) and also still surprisingly common despite being a known exploit for decades.

Want to [learn more about preventing injection](https://cheatsheetseries.owasp.org/cheatsheets/Injection_Prevention_Cheat_Sheet.html)? OWASP has you covered.

## 2\. Broken Authentication

When an attacker can login as someone else, you’ve obviously got a problem.

This category of risk encompasses a lot of potential problems:

*   Storing passwords as plaintext in the database (NEVER, never, NEVER do this!) & then having your database compromised
*   Storing session tokens in insecure locations, allowing attackers to use the token to gain authorization
*   Allowing users to pick common or weak passwords that are easy for attackers to guess
*   Not expiring session or API tokens, meaning a token only needs to be exposed once & it’s valid forever
*   Sending back different and identifiable information based on failed requests. Bad: “No such username” (If an attacker keeps guessing long enough, they’ll find a valid username that returns a different response) Good: “Invalid credentials”

The top ways to prevent these issues, according to OWASP:

*   Where possible, implement multi-factor authentication to prevent automated, credential stuffing, brute force, and stolen credential re-use attacks.
*   Do not deploy with any default credentials, particularly for admin users.
*   Implement weak-password checks, such as validating new or changed passwords against a list of the top 10,000 worst passwords and requiring passwords to be at least 8 characters.
*   Ensure registration, credential recovery, and API pathways are hardened against account enumeration attacks by using the same messages for all outcomes.
*   Limit or increasingly delay failed login attempts. Log all failures and alert administrators when credential stuffing, brute force, or other attacks are detected.
*   Use a server-side, secure, built-in session manager that generates a new random session ID with high entropy after login. Session IDs should not be in the URL, be securely stored and invalidated after logout, idle, and absolute timeouts.

## 3\. Sensitive Data Exposure

Over the past few years, this is the most common attack vector.

Things start to get tricky, because attackers can exploit many types of man-in-the middle scenarios to steal data when it’s in transit or temporarily being stored in plaintext.

The attacks can take many forms, but the most common ways attackers steal information are:

*   Downgrading connections from HTTPS to HTTP to decrypt data in transit
*   Cracking a weaker encryption scheme that was used to store or transfer data
*   Pre-hashing popular passwords using unsalted and simpler encryption schemes then matching those hashes to stolen user tables
*   Using SQL injection to make the database engine fetch data into plaintext that was encrypted

To prevent these attacks:

*   All sensitive data should be encrypted with a modern encryption algorithm
*   If you don’t absolutely need to store some data, then discard it — if you don’t store it, attackers cant steal it
*   Store passwords using strong adaptive and salted hashing functions with a work factor (delay factor), such as [Argon2](https://www.cryptolux.org/index.php/Argon2), [scrypt](https://wikipedia.org/wiki/Scrypt), [bcrypt](https://wikipedia.org/wiki/Bcrypt) or [PBKDF2](https://wikipedia.org/wiki/PBKDF2).
*   Make sure session cookies and other sensitive browser data are only sent on HTTPS and that they’re not available in client-side javascript

## 4\. XML External Entities

XEE attacks allow an attacker to make your code evaluate an external URL when they upload an XML file to your application.

These types of attacks are especially prevalent and successful against older applications with dependencies that aren’t up to date to protect against XEE attacks.

The easiest way to prevent these attacks is not to use XML. Pick a stricter data format like JSON or YAML.

If you have to use XML, make sure all your parsers, processors, and dependencies are up to date. Source Code Analysis Tools can help identify issues in existing applications.

## 5\. Broken Access Control

This category is similar to #2 (Broken Authentication), but it applies when an attacker can access sensitive data without being logged in as the correct user.

For example, say an attacker logs in to your website and navigates to the billing history page at a URL like:

`mysite.com/users/1809/billing/history`

It’s a good guess that the `1809` part of the URL is the user ID.

If an attacker can change that ID and see other users’ billing data, then you have a broken access control attack.

Similarly, attackers can manipulate other IDs, tokens, or credentials to visit blocked URLs or elevate their privileges to admin.

Checklist to prevent these attacks:

*   Deny by default for all web requests and API endpoints — make authentication standardized and default across your application
*   Create, read, update, and delete access on data models should always be user-specific and managed with explicit model permissions
*   Disable web server directory listing and ensure file metadata (e.g. .git) and backup files are not present within web roots
*   Log access control failures, alert admins when appropriate (e.g. repeated failures)
*   Rate limit your API and URLs to prevent manipulation that comes from automated tools

## 6\. Security Misconfiguration

No matter how secure your server or client-side framework is, it means nothing if you have the security settings poorly configured.

For example, many frameworks include debug settings that are useful when developing but dangerous if released into production.

Other pitfalls:

*   Leaving default accounts active and/or with their default passwords unchanged
*   Having error messages that reveal stack traces or other info about configuration
*   Using out of date frameworks and libraries without the most recent security patches
*   Leaving environment variables or other secure keys in publicly accessible config files or source control

Luckily, remedying these problems is easy. Use the most up-to-date version of your framework & libraries. Then, learn about and follow security best practices for configuring your application for production.

Additionally, make sure the process for deploying to production always and predictably includes these security hardening steps.

## 7\. Cross-Site Scripting (XSS)

This is an increasingly popular (and dangerous) attack vector as client-side JavaScript has become more prevalent.

Basically, this attack involves running malicious JavaScript and HTML in the user’s browser. While the code appears to come from a verified site, it’s actually from a dangerous source.

For example, if a site you trusted opened a popup like this, would you fill it out?

![](https://miro.medium.com/max/600/1*cNNE6eY224iZ8SnKS_okLw.png?q=20)



Example XSS attack

While it looks trustworthy, it may also have come from an attacker spoofing the design and features of the target site.

Malicious XSS attacks come in 3 basic varieties:

1.  **Reflected XSS**: The application or API includes unvalidated and unescaped user input as part of HTML output.
2.  **Stored XSS**: The application or API stores unsanitized user input that is viewed at a later time by another user or an administrator.
3.  **DOM XSS**: JavaScript frameworks, single-page applications, and APIs that dynamically include attacker-controllable data to a page are vulnerable to DOM manipulation.

To protect against these attacks:

*   Use up-to-date frameworks that protect against XSS — Django, Rails, Spring, React, Angular, and all other major frameworks include provisions for XSS
*   Never put untrusted data inside a script, HTML comment, tags, tag attributes, or directly in CSS
*   Escape any untrusted data you do insert into the page:

 & --> &amp;  
 < --> &lt;  
 > --> &gt;  
 " --> &quot;  
 ' --> &#x27;  
 / --> &#x2F;

## 8\. Insecure Deserialization

Complex applications often need to pass data models back and forth between services.

For example, a React app may need to talk to a Spring Boot backend. Since the requests can be complicated and based on a lot of conditions, the developers just serialize the React application’s state and then send that to the backend with every request.

When an attacker notices that the Java objects are coming back with a certain signature, they can use tools like Java Serial Killer to infiltrate the server.

Similarly, a PHP site might use PHP’s object serialization to save a cookie with the user’s session ID and privileges. Spoofing that serialization would allow an attacker to elevate their privileges or login as someone else.

What you can do about deserialization attacks:

*   Don’t serialize objects directly. Instead, use a data format that only allows for primitive types (e.g. — Use a JSON schema of your object)
*   Add hashed signatures to serialized objects to make sure they haven’t been tampered with.
*   Only deserialize in isolated, low privilege situations & log all exceptions so you can disable or rate limit users that are deserializing too often

## 9\. Using Components with Known Vulnerabilities

This one is also easy to avoid. If your third-party libraries are out of date, chances are you have a security vulnerability.

Upgrading your dependencies will usually fix these vulnerabilities.

In fact, source control providers like GitHub can help you monitor and prioritize these issues:

![](https://miro.medium.com/max/600/1*WkPHcHtEMzpQvW8oyryhIw.png?q=20)



Furthermore, GitHub can automatically create a pull request to upgrade your dependencies.

## 10\. Insufficient Logging & Monitoring

> What gets measured gets managed.

Nearly every major security incident would be mitigated with earlier warnings.

Logging and monitoring is the foundation of security, because it allows you to track, review, and blacklist suspicious requests.

According to OWASP:

> Most successful attacks start with vulnerability probing. Allowing such probes to continue can raise the likelihood of successful exploit to nearly 100%.  
> In 2016, identifying a breach took an [average of 191 days](https://www-01.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=SEL03130WWEN&) — plenty of time for damage to be inflicted.

Things to start monitoring today:

*   Logins
*   Failed logins
*   Any high-value transactions
*   Request frequency by user

Once you have logs for these things:

*   Make sure there are alert thresholds for suspicious activity
*   Log this data to somewhere outside the local server
*   Preferably, use a third party monitoring service to consume and visualize your logs — raising alarms in real time
*   Run a penetration testing scan (using a tool like [OWASP ZAP](https://owasp.org/www-project-zap/) — see I told you OWASP is awesome) and make sure it triggers your alerts

Since the variety of possible attacks is so wide, logging is your best bet to detect attacks when they come.

# What I Learned from the OWASP Top Ten

Discovering the OWASP top ten has changed the way I approach web development.

Of course, I’ve always known that security is important, and I’ve heard of the more common attacks in this list.

Thanks to OWASP, however, we now have a clear checklist of attack vectors and best practices to protect against them.

OWASP even has awesome [open source projects](https://owasp.org/projects/) to help test, harden, and analyze your application. Their [Juice Shop](https://owasp.org/www-project-juice-shop/) example is especially helpful to train developers on the ways these attacks could take place.

Every developer has a responsibility to protect user data. It’s critical that all developers know about the [OWASP Top Ten](https://owasp.org/www-project-top-ten/) security risks and how to address them.

# About Bennett

I’m a software developer in New York City. I do web stuff in Python and JavaScript.

Like what you’ve read here? I have an [email list](https://mailchi.mp/2e671faffc04/bennett-medium) you can subscribe to. Infrequent emails, only valuable content, no time wasters. I’d love to have you there.