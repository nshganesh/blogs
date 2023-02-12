---
title: "Microservices Summarised"
description: "Monolith:"
date: "2023-02-12T08:55:05.637Z"
categories: []
published: false
---

Monolith:

i. Shared database/api with both buyer ecosystem and seller ecosystem

Microservices:

i. Single service with dedicated database

Force:

Team of developers

I. Hire based on the likings/requirements.

Ii. Hire quality developers / ownership of services.

Product Development Constants:

New team members must quickly become productive

The application must be easy to understand and modify

Able to make continuous deployment of the application

Able to run multiple instances of the application on multiple machines in order to satisfy scalability and availability

Take advantage of emerging technologies/frameworks

Monolith 1& 2:

Simple to develop, deploy, scale. Testing is easy I guess.

With monolith 1, Hiring pattern can be 1.

With monolith 2, hiring pattern could be 1, 2

Once the codebase size increases all three becomes complicated.

New team member productivity decreases.

Difficult for a team to make a change and update production.

Testing the whole system could become difficult.

Scaling problems:

Caching less effective, increase in memory consumption/io traffic.

Cannot separate out cpu intensive/memory intensive.

SOA:

Each microservice is relatively small. Easier for a developer to understand

Improved fault isolation

application starts faster, which makes developers more productive, and speeds up deployment

Better maintainability, testability, deployability

Eliminates long-term commitment to a technology stack

Automatic scaling

Disadv: Increased memory consumption, Testing between services, Implementing the inter service communication and handling partial failure.

Decompose by business capability

Multi service per host

Event driven architecture

Circuit breaker

Monitoring and Logging

Framework, AWS, Docker, Kubernetes, Tooling, Configuration, Discovery, Routing, Observability, Automated scaling,

Deploy: spinnaker

Auto scale EBS: aminator
