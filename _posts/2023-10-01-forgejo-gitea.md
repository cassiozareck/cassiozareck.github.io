---
title: Experience in Gitea/Forgejo
date: 2023-10-01 12:00:00 -500
categories: [professional]
tags: [orm, concurrency, open-source, team, go, software-design, gitea, forgejo, learning, experience]
---


During the college vacations I've turned my routine into 2 things: software contribution and leetcode. I've found the Gitea project on github, a self-hosted git service made in go (almost like github or gitlab but you can set up on your local machine as a server but its lightweight and simple). This project was a bit huge with more than 15k of commits. But you know, after you contribute to a small project it is always a good idea to get a bigger to see how things change.


### Some history
I've started by learning the basics: how to use it, configurations, read documentation, join on discord. This part was boring because there wasn't any code. I thought it would take me months to contribute because Gitea had more than 270k of Go code lines. But quickly I got some easy issue a maintainer had recommended on discord after I said I was motivated to contribute and It took me one day to fix it and got my PR merged. Here's the thing: no matter how big a software is, if its well structured, documented and designed a fix for a bug, as well addition of new features will be trivial.


After that I've sent PRs to fix a problem where settings weren't being loaded while using CLI, fixed some typos, and also I made a mistake. I was so confident about my flow on Gitea that I started to write a feature for the deletion of runned workflows, but I completely ignored the complexity of this work. This is something that really separates beginners from experts. I saw this pattern with my own eyes. More experienced maintainers always say the hard part of some bug fix or new feature. Even if it looks easy they can immediately find painful and complex parts of a problem. It's funny how beginners want to implement everything, even hard things but experts avoid it and easily become skeptical about a feature and most of the time they're right, prioritization is fundamental for the life of a software.


After this sad event, I've started to dive more into concurrency on Gitea but I'll talk about this in another post as I also want to explain how concurrency really works on real-world software. But one thing that was bothering me was some PR's I was making were taking a long time to get merged. Even small fixes with two lines of change were getting ignored for weeks. Even now there are open PR's I made 2 months ago. I guess this is because Gitea is not only a community project, they have a company where I guess they make customization on the software for companies. So mantainers have not too much time on the community Gitea project and your PR must be a bit aligned in a way it does not mess with the implementation Gitea ltd make for companies.


### Migration to Forgejo
Forgejo is a fork from Gitea, but it's 100% from the community, it was forked in 2022 by the reason I said above. It's hosted on codeberg. The community from there is brilliant. I took an issue that was the addition of an option to migrate repository from Forgejo instances like Codeberg instead of using the Gitea option for both. When I send a WIP of the PR it tooks minutes to a maintainer review and engaje in the PR.


By now Im finishing to write a feature that is taking me more than 700 lines of code. It's the addition of a way to migrate and mirror repositories from a given origin, like github starred repositories. This means we can select several ways to "backup" repositories from external git services. It's the most complex feature I've ever worked on. I'll explain this a bit more in the next post with a concurrency explanation.


One thing that is impressive is the synergy between Gitea and Forgejo. At first I thought because of the different philosophy they would hate each one but its not the case. In fact every feature you implement in one will almost certainly be implemented into another and this is the expected case. In fact there's Gitea developers writing PRs on Forgejo... this is very funny but at the same time shows how humans can be humans and work together instead of being competitors that don't have any objective.


### Experience and learning
- How a big codebase can be designed in a clean and flexible way. Lower-level modules should not have access to function on higher-level modules and ideally should not call other modules at the same level. There's a clear hierarchy between routers->web->services->models->modules (the most low-level)
- ORMs, transaction and context: In Gitea and Forgejo we don't work directly with database queries. Instead we rely on ORMS with transactions and contexts. Contexts was another thing I should really understand as request-scoped values and cancel transactions
- Concurrency. I've helped to improve Gitea concurrent queue system documentation and implement a hands-on concurrent system on my new feature as migration scheduling need some way to happen in a separate goroutine.
- Rest APIS, middlewares, chi web framework and http protocol
- Git cherry-pick. I mean, its not so important but not everyone knows git cherry-pick.
- Team-based project, feature implementation, communication and those things


## Resources
- Forgejo (the project loves new contributors): [https://codeberg.org/forgejo/forgejo](https://codeberg.org/forgejo/forgejo)
- Gitea: [https://github.com/go-gitea/gitea](https://github.com/go-gitea/gitea)
- My codeberg profile: [https://codeberg.org/zareck](https://codeberg.org/zareck)




