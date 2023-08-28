---
title: SOLID/discrete math
date: 2023-06-15 12:00:00 -500
categories: [professional]
tags: [math, programming-architeture, learning, experience]
---

While I was contributing to Keploy at half of April I thought I was thinking too much on just code. I felt a need to expand my knowledge and delve more into fundamental topics, leading me to the Mathematics for Computer Science course from MIT! What a journey! Prof. Leighton is one of the best professors I have ever seen. He explains the subject deeply without keeping hours on the same thing. Also, they make you think, you cannot go to another lecture without doing exercises and really understand what’s happening. I want to finish it another time.

### Technical things about the math course
The course starts teaching discrete math. This is one of the most different things I have ever seen. You use a kind of philosophical logic to prove things. And with very, very basic axioms such as the Well Ordering Principle (and its brother Induction) you can prove very complex things like some properties of prime numbers. There's a list of these very basic axioms called ZFC set theory and you can prove almost every concept of math using it.

Being realistic its simple, not easy. You have to put your attention on this and have a strong abstractive thinking. Sometimes you’ll just ask yourself "how can I have counterexamples of this if it's clearly impossible?". Or "why does this need proof it's obvious". Generally obvious things are pretty hard to prove. The course also passes through some of Alan Turing's paper of cryptography and even explains the ENIGMA's gap. Graph theory is also there!

### Relation to programming and why math is important
Today I was doing some exercises on leetcode and I challenged myself to not write code till I have finished to think on the entire algorithm. I tried to draw it and put it on the paper explaining with normal language. It was way more hard than just keeping directly writing some python code and figuring out things from there. A friend of mine also said that it's harder to explain an algorithm than just code it.

When we're writing or creating an algorithm we usually don't try to abstract an algorithm. We ignore thinking in it with mathematical terms. This is bad since the own act of writing algorithms turns itself into an algorithm. For example, when you try to represent a graph in a mathematical way, thinking about it abstractly without programming language barriers, you can understand way better what's happening and how to manipulate things. A good algorithm isn’t created thinking in programming but rather on abstract (imaginary) objects we can manipulate. Then we can try to make the computer understand.

Also, the course teaches you about argument, reasoning, logic, these ares important abilites. But its not just about learn, its about pratice, put it in a paper and make things works following a rational path. This is something that trains your brain to solve problems even not related to math, how to reach at a concise and robust solution to a hard problem giving very few informations.

### About SOLID principles
In half of May I also started to read Clean Code as I stopped contributing. This book is needed if you work with code daily. It is a guide to how to save time, money and let a software survive for more time by teaching you how to write good code. 

### A bit of what I learned/reinforced
 - Open-closed principle can be hard. This requires you to analyze when a class is de facto doing more than the needed, also sometimes there's not much sense to make a lot of polymorphism and exotic classes. The thing is to always try to make your code extensible without having to change it every time.
 - The level of a function should always be the same. If you need to do lower-level operations, call lower-level functions don't try to embed it into the code.
 - Always make interfaces when dealing with an external API that can change. Create a protection layer.
 - Demeter's law is pretty good. You should never let a module navigate between objects of an object you're using.
 - Don't make object/data structures hybrids. Objects use abstraction to hide its data.

There's some points I disagree with in the book like abstract factory, small functions, detailed names for functions, huge amount of polymorphism… these aren't common things in Go, for example. Looking at any open-source code from a medium project you will see that. Bigger functions make more sense and are more readable sometimes. Just keep them consistent and doing just one thing

Overall, I learned a lot more than this but for this post it's enough. Knowledge is something magic and pleasurable. Sadly we have finite time and a limited brain. But the path of learning isn't an arrival, it's a journey, enjoy it and keep a life seeking for new knowledge and experiences. Hope you have a great day! 














