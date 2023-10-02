---
title: Threads and concurrency on big softwares
date: 2023-10-01 13:00:00 -500
categories: [professional]
tags: [concurrency, orm, open-source, team, go, software-design, gitea, forgejo, learning, experience]
---

When I was working in the addition of an option to migrate from Forgejo instances I saw that the migration of repositories was not made directly, but rather transformed into a task (normal go struct) which saves data and store into a queue the information of this new migration. But what happen after this item is added in this queue?

### Concurrency
One of the greatest power of Go is its support for concurrency that can create user-land lightweight threads and at the same time be idiomatic with built-in support for goroutines and channels. When we work with small examples to learn concurency we may not see too much utility on it and we may never use in small softwares but in this case of migration we can learn a lot with a real world example. Let's begin explaining what is a migration, why it need support for concurrency and how its done.

### What is a migration
When we want to clone a repository on another git service, e.g., from github to gitlab we can start a migration process which will clone the codebase as well commits and another things. 

### Why concurrency
Imagine you're migrating 10 repositories at the same time. I suppose you don't want to stay blocked until it finish. This situation require multiple threads running under the same application. But also we don't want all migrations runs on the same separate goroutine/thread. We need a new execution for each one so we can ensure paralelism. 
But how? Goroutine for each migration request? But what if we keep creating goroutines even if max of CPU physical cores was reached and some threads already finished their work? By doing this we're creating stack and scheduler overhead and not an efficient use of CPU. We need something more clever

### Worker pool queue
First we will create a normal queue that will be responsible for storing items that represents a unit of work, these items will contain the data needed to work on, e.g., a migration, like URL, repository name..., lets call this item as a task. Now, instead of associate a function with a new goroutine we'll create a single handler capable of work with this these tasks. It's still a single function but now we can reuse the same function for multiple tasks and here's where things can get interesting.

What if we have a Manager that can creates n goroutines where n is the number of physical cores. Lets call this goroutines workers. Now what if we integrate everything? We can use this Manager to retrieve new tasks and distributed these set of tasks across multiple workers. If everything works correctly we achivied maximum parallelism.

<img src="{{ site.baseurl }}/assets/concurrency_schema.png" alt="description" width="1000"/>
