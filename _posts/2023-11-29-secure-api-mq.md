---
title: "Secure API | RabbitMQ"
date: 2023-11-24 13:00:00 -500
categories: [professional]
tags: [auth, bcrypt, api, jwt, token, cryptography, rabbitmq, learning, experience]
---

### What’s bcrypt why little-ecom use it

Bcrypt is a widely known password hashing function, it works by adding a salt to a password and hashing it in this format:
```
$2a$12$R9h/cIPz0gi.URNNX3kh2OPST9/PgBkqquzi.Ss7KIUgO2t0jWMUW
\__/\/ \____________________/\_____________________________/
Alg Cost      Salt                        Hash
```
This format makes it impossible for hackers to use rainbow-tables to gain access for common passwords. Rainbow-tables are a common hacker artifact that maps commonly used passwords to possible hashes, so if you don’t use salts hackers can easily match a hashed password to common passwords (even not that common).

This is why the register endpoint from auth service under little-ecom uses it. We ensure security by using bcrypt who add those salts and hash the password. Then, when we want to check if a password is correct, we just take from the body request (encrypted with https), add the salt and check if it correctly matches, of course bcrypt automatizes this process for us, so we don’t need to do it.

Now, you see that it’s not hard even with bcrypt to get a single password if it’s weak. Since the salt is stored openly in the database, if one gain access to it, he can start trying combinations like “dog123”, “password” and use the same bcrypt function with the stored salt and guess it.

But bcrypt turns this process of manual intervention even harder by turning its cryptographic process very heavy in terms of computation, so you cannot create a tool using bcrypt to try billions of times a single password.

### JWT Tokens
JWT Tokens are tokens whose contents inform things like username, privileges, SignIn data… They are meant to keep a secure stateless API while the server can still be able to know whether a user is logged or not, and be able to get useful data just from the token without need for database queries.

When the client makes a call to SignIn endpoint with proper credentials, a JWT token is returned, with expiration date and things like that. Here’s a proper JWT structure, I took from wikipedia:
```
The three parts are encoded separately using Base64url Encoding RFC 4648, and concatenated using periods to produce the JWT:
const token = base64urlEncoding(header) + '.' + base64urlEncoding(payload) + '.' + base64urlEncoding(signature)


The above data and the secret of "secretkey" creates the token:
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJsb2dnZWRJbkFzIjoiYWRtaW4iLCJpYXQiOjE0MjI3Nzk2Mzh9.gzSraSYS8EXBxLN_oWnFSRgCzcmJmMjLiuyu5CSpyHI
```
Source: wikipedia

This signature at the end guarantees the client can’t adulterate the payload to change username or grant more privileges. But even more important, JWT tokens guarantee a stateless API, this means we don’t need to store anywhere data about the logged user on the server-side and keep doing IFs to check. In our own middleware rules we can assign endpoints being only accessible with a valid token. This makes things more decoupled and easy to test.

SignIn and Validate are two endpoints for that, SignIn generates and Validate returns the payload associated with the token, if valid. Now, if the client is a frontend, this token will be stored as a cookie under client-side.

### Cookies vs session
JWT is generally stored as a cookie on client-side, so the browser stores it. Sessions on the other hand are data about user sessions stored on the server-side and identified by a string that is passed as a cookie to a browser. But even if JWT works like a session it doesn’t require the server to store any data. The data is self-contained inside the JWT string.

### Suggestions for improvements
- CSRF tokens: Unique, secret codes added to web forms to prevent unauthorized requests from other sites. It's like a hidden, one-time password for each form submission.

- 2FA Authentication: Adds a second layer of security to account logins. After entering the password, the user must verify their identity through another method, like a text message code or an authentication app.

- Captcha:Challenges designed to tell humans and bots apart. These often involve recognizing distorted text, images, or solving simple puzzles.

- Limit routes: This involves setting a maximum number of requests that can be made to a particular endpoint within a certain time frame. For instance, limiting how many times a user can attempt to log in to prevent brute force attacks.

### RabbitMQ
When I first designed little-ecom I started to think about backend and notifier communication. When implementing async queues a question that often arises is what kind of topology we’ll use and how. A topology is the architecture of our queues and exchanges, the flow of communication between different systems.

We can define a script for creating this topology at the beginning. However, I didn’t take this path. The topology was defined by the services that use it, and only for queues that this system will use, this has advantages but you cannot ensure the topology is well-defined for every system. Debugging will be more frequent.
