---
title: A journey into open-source
date: 2023-05-04 12:00:00 -500
categories: [professional]
tags: [go, open-source, team, learning, experience]
---

### The beginning
I’d never contributed to open-source before. Barely I saw some repositories and became scared of how people could read more than 10k of code and understand how things work. I was wondering how I could start in this world, even more when I was really liking Golang’s world (you know, for a junior to land a job on a Go job probably his efforts would be 2x than a Node or Python junior dev).
Then I found GSoC! Good place to start. A Google program to help beginners to contribute, I tried to choose the smallest project made in Go (the smallest was very big). So my journey started with Keploy, when I started it had something around 210 commits

### Some history
I was having trouble even understanding what Keploy was, spending some time on it I figured out it was a tool that can test APIs. Basically it stores your APIs responses by recording it and when you need to test it makes these calls to see if the response is matching the last state. Then starting to read its code I almost gave up. I was not understanding.

In the GSoC you need to do some tasks to be accepted. Keploy had a list of them and I started to work on it. My first PR was improving Keploy test coverage. I could barely increase it 0.5% but it was a thing. There were about 6 tasks. 
But then I kind of put myself into a trap. In my second contribution I chose a task related to diff between expected and actual result of Rest APIS (basically a JSON differ). There weren't any easy alternatives for Go as they wanted diffs side-by-side. I had to do it with my own hand.
It took me some weeks and more than 200 lines of (a bit messy) code. I saw others making PRs doing the same task but they used "workarounds" that weren't any way good; they looked like some old terminal with glitched buffers. It was merged
I also make a way to pipe logs to files and also mock the test exporter so we could test it without doing real OS operations. But there were tasks too complex for me at this time. Like use GRpC. I was just a beginner at Go’s world. And also I was beginning at a CS university. Time was a problem

### I was beaten
There was a guy that made all the 6 tasks in few days. Helped to create a documentation for the Keploy’s API and even contribute to non-related parts of the project like the SDK. I thought that maybe my 15 pages 

## TODO: day i started, neha message, 

### My learning and experience
I will make this in points since this was a wonderful experience where I learned a lot: 
- Read a software code requires some days of “I'm not understanding a shit”. Be patient and understand that you will need some days so you can land on it.
- If a tool/lib exists, don’t rewrite yourself. Even easy tools are way, again, way more complex than you can think. I had to do my JSON differ because I had no alternative but it was distant from being good and it took me more time than I ever thought and more than 200 lines of code.
- Your code probably has more side-effects on the software than you think. Be careful when doing changes.
- Git. You don’t understand how much I had to push myself to do “esoterics” git commands because I missed the branch, forced push something wrong, someone pushed changes exactly in the file I was and now I have to resolve merge conflicts, rebasing… I thought I knew something of Git but nah
- Don’t try to do everything at the same time. Prioritize things. Almost every push you will make probably some maintainer will find an issue. It's not because it's working, it hasn't any issue. 
- Communication is everything. Don't be ashamed to ask. There are people that were in the project way before you and can even give you some advice and you will learn a lot with them. Don’t stay on a project made by the community where experts don't help the community
- Tests are complex, always do but take care of decisions, mock things cause overengineering and most of the time it's not necessary. Table-driven tests can be bad in some places.
- Yeah concurrency is important. Even on a small project like Keploy I had trouble with outputs being mixed on stdout because the API Tester were run concurrently. At first I thought it was my code but another guy also stated a problem with race conditions. I think they already solved it.
- Designing your implementation as a proposal is hard but it's worth it. I had to write features on a document and kind of explain the purpose of the feature and how it would be implemented. This is solid as rock. Always try to make first model year changes, don't go coding without think, write it on a paper, draw diagrams and try to understand how this feature will “glue” with others.

There were more things I learned and more history that happened. GSoC is a great place to start in open-source, not necessary but worth it if you want more collaborative development. The key thing is just to start. I hope you have a great day!









