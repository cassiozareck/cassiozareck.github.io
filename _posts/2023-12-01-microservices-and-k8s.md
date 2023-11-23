---
title: "Little-ecom: microservices and k8s"
date: 2023-12-01 13:00:00 -500
categories: [professional]
tags: [k8s, rabbitmq, microservices, mongodb, cloud, personal, learning, experience]
---

It has been 2–3 months since I'm involved in a personal project called little-ecom. It's a small ecommerce api that I've built, and It's capable of
 registering a user, buy a product, send emails, make CRUD operations on items and other basic functionalities. Its microservices-based, and it has 3 main services: backend, auth, notifier. But it has several other deployments and statefulsets needed to work like rabbitmq, mongodb and an efk stack. I want to explain here some of the process I've been through to build this project.

TO-DO: put here the github link or picture explaining the project

### Backend service 
I started by building the backend service using Go. This backend service would be responsible to make crud operations on items (products). I'd chosen to use mongoDB because it's a NoSQL database, and I wanted to learn more about it. I wrote some Go code using mux and http library, deployed on Docker Hub, built the k8s manifest with 2 replicas and deployed it on my minikube cluster. But I was having nightmares to set up my mongo service. This is because mongo replicas cant be run over a deployment.

### Mongo, StatefulSets and Helm charts 
If you want to run your database with replicas and thus, data replication, you will need to store some state between its replicas. For example, in a mongo setup, you will need to store the data of each replica, so if one of them goes down, the other one can still serve the data. Also you may want to store some metadata like the replica configuration. This is not feasible with a deployment because a deployment is a set of pods that can be scaled up and down, and if you scale down a pod, you will lose its data. So, you need to use a statefulset.

Everything was ok if it wasn't the problem that when we work with multiple Mongo replicas, we need to configure them to know each other. Usually I had to join inside the running service and run some mongo commands to configure it. But if I had to reset the cluster by some reason, I would need to do the entire process again. Remembering each command and config.

This is a common behaviour, but since it wasn't being sustainable for me I started to learn about helms and instead of building the k8s manifest and configure mongo by myself I used an out-of-the-box bitnami mongo helm chart which automatizes this process. Helms charts are very powerful in the sense they abstract the complexity of configuring a service and make it easy to deploy, letting you overwrite a small set of common configurations.

### Cluster network and API gateway
I was having a lot of problems with DNS configuration, service communication and discovery, so I started to learn more about k8s network and dedicated a lot of my time on it. But I'll let this for a future post since its vast subject and is attached to how container networking works.

### EFK Stack
I was wanting to visualize each and every log to have control of everything on the cluster, so I had to rack one's brains setting up an elastic-search, fluentd and kibana stack. I did it, but I'll be honest that I haven't used it, it doesn't help that much if you doesnt have a lot of logs to visualize and know how to configure filters. But it was a good experience to learn more about logging and how to configure it.

### RabbitMQ and Notifier service 
I've also written a notifier service that'll send emails whenever new items are added into the aplication, but this implies a backend-notifier communication that needs to happen even with instability or be enqueued for notifier to read. So, I've deployed a RabbitMQ instance on the cluster and used the default exchange to add messages in a queue where notifier will be reading.

Now, its a bit hard to understand the 4 types of exchange from amqtp protocol. There's visualizer on the internet that helped me understanding the use case of each one and the communication flow. I've also written some scripts to initialize RabbitMQ admin panel which is very helpful to manage and visualize exchange, queues and whats bound to it.

### Ingress and reverse proxy
Since little-ecom has 2 main services supposed to be accessible (backend and auth). I needed to expose them to the outside world. I've used an ingress controller to do this. An ingress controller is a reverse proxy that will redirect requests to the correct service. In my case, I defined 2 http rules: if it starts with /auth goes to auth service, in case where it doesn't then should be redirected to the backend service. I've used nginx ingress controller, but there's also traefik and others.

Oh, I had a big problem with it. Since I was using swagger to test my endpoints defined in an OpenAPI file, I had problems with CORS. Since swaggers were running on my browser, it asked for CORS permission to the backend service, but since it was running on a different domain, it was blocked. I've solved this by adding a CORS middleware on the backend service, it wasn't enough, so I also had to write a specification in the ingress controller to allow CORS. This gave me a lot of headaches.

### Auth service

(todo: put stateful mongo file)

One of the reasons I started this project was because since last year I was used to work with docker and docker-compose. Thus, I was wanting to expand my skills into microservice's world, so I started to build an ecommerce. The best way to learn is by practice. If you face the problems by yourself, you'll develop a stronger skill in this subject.

### About this project
As always, these projects takes way more time than we thought. But it's a rewarding experience that I wouldn't get with any tutorial or course. Some of the key things I develop in this process:
- k8s and different components: statefulsets, deployments, nodes, configmaps... 
- rabbbitmq: communication between 2 services intermediated by a queue
- hands-on experience with ingress and reverse-proxies
- how to model a software into small services
- routings and network through a distributed system
- evetual consistency with nosql database
- cluster managament: pods scheduling, solving dns problems, scaling nodes...
- auth


