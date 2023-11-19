---
title: "Little-ecom: microservices and k8s"
date: 2023-12-01 13:00:00 -500
categories: [professional]
tags: [k8s, rabbitmq, microservices, mongodb, cloud, personal, learning, experience]
---

It has been 2 months since I'm involved in a personal project called little-ecom. It's a small ecommerce
that I've built. Its 100% microservices-based and it has 3 main services: backend, auth, notifier. But it has
several other deployments and statefulsets needed to work like rabbitmq, mongodb and an efk stack.
TO-DO: put here the general architecture

Since last year I was used to work with docker and docker-compose and I was wanting to expand my skills into
microservices world, so I started to build an ecommerce. I always thought the best way to learn is by pratice
and search, if you face the problems by yourself, you'll develop a stronger skill in this subject. 

### Backend service 
I started by building the backend service using Go, this backend service would be responsible to make crud operations on a
a mongo database. The backend deployment was easy, just some doc reading and tutorials. I've used mux and the default
go HTTP library to make the REST endpoints.

Then, build docker image, publish it on dockerhub, write a k8s manifest and call kubectl. But, I was having nightmares to setup my mongo service. This is because I was wanting to run it on multiple replicas, the problem was mongo replicas cannot be run over a deployment.

### Mongo and StatefulSets 
If you want to run your database with replicas and thus, data replication, you will need to store some state between its replicas. For example, in a mongo setup you'll need a master node (write and read capabilities), while anothers just can just read data. StatefulSets can save you in that situation.

So, I've begin to learn about statefulsets, then I've built a small manifest for my mongo server, but the real trouble 
was that often I needed to reset the cluster, but once I do that I lose mongo inner replica configuration. When dealing with mongo instances if you want to replicate data it's not only about k8s, I had to join inside the mongo pod to configure a new ReplicaSet pointing to the mongo DNS service.

This is a common behaviour, but since it was not being sustainable for me I learned about helms and instead of build
the k8s manifest and configure mongo myself I used an out-of-the-box bitnami mongo helm chart which automatizes
this process. 

But then, another problem comes in the table, when dealing with clusters the visualization between the
communication between different pods becomes very important. It's not easy to visualize if backend requests are reaching inside Mongo service, logs doesn't help that much.  

### Cluster network and API gateway
I'll write another post stating about veths, linux iptables, middlewares, node and pod communication and kubelets, it's a very
big subject and I've spent some time to learn it

### EFK Stack
I was wanting to visualize each and every log to have control of everything on the cluster, so I had to rack one's brains setting up an elastic-search, fluentd and kibana stack. I did, but I'll be honest I didn't used it, it doesn't help that much if you doesnt have very useful logs and my mongo was emiting 1000 prints per second. And also my 2-nodes minikube cluster was getting heavy, I could hear my pc fan louder than my room fan, I give up this part. 

### Notifier service and RabbitMQ
I've also written a notifier service that'll send emails whenever new items are added into the aplication, but this implies a backend-notifier communication that needs to happen even with instability or be enqueued for notifier to read. So, I've deployed a RabbitMQ instance on the cluster and used the default exchange to add messages in a queue where notifier will be reading.

Now, its a bit hard to understand the 4 types of exchange from amqtp protocol. There's visualizer on the internet that helped me understanding the use case of each one and the communication flow. I've also written some scripts to initialize RabbitMQ admin panel which is very helpful to manage and visualize exchange, queues and whats bound to it. 

### Ingress and reverse proxy
Ingress is useful in clusters since you will probably have gateway/backend replicas where you want some load balancing and probably some routing-specific setup. An Ingress is pretty easy to setup. I've used the nginx helm chart as my ingress controllerimplementation. It serve as a reverse-proxy to my API gateway replicas.

### Auth service

(todo: put stateful mongo file)

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


