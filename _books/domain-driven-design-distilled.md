---
layout: book
title: "Domain-Driven Design Distilled"
author: "Vaughn Vernon"
description: "A short but comprehensive resource about DDD. The perfect book to get familiarized with this approach."
cover_image: domain-driven-design-distilled.jpg
published: false
---


## Notes

This book can be read by non-technical people.
Dig into the why DD first before any implementation details
We are all using DDD notion without realizing it (namespace, events, DB transactions, uniquitous langage, ...)
Bring business experts closer to tech teams.
Worth reading even if not doing any DDD.
Complete with code evans original book if you want to really dig into it.


## Chapter 1: DDD for me

> DDD is a set of tools that assist you in designing and implementing software that delivers high value, both strategically and tactically. Your organization can't be the best at everything, so it had better choose carefully at what it must excel.

Common problems:

* Software development is considered a cost center rather than a profit center.
* Developers are too wrapped up with technology and trying to solve problems using technology rather than careful thought and design.
* The database is given too much priority, and most discussions about the solutions center around the database and a data model rather than business processes and operations.
* Developers don't give proper emphasis to naming objects and operations according to the business purpose that they fill.
* The previous problem is generally the result of poor collaboration with the business.
* Project estimates are high in demand, and very frequently producing them can add significant time and effort, resulting in the delay of software deliverables.
* Developers house business logic in user interface components and persistence components.
* They are broken, slow, and locking database queries that block users from performing time-sensitive business operation
* There are wrong abstractions, where developers attempt to address all current and imagined future needs by overly generalizing solutions rather than addressing actual concrete business needs.
* There are strongly coupled services, where an operation is performed in one service, and that service calls directly to another service to cause a balancing operation.

Strategic design first then tactical design
Strategic: Bounded Context, Ubiquitous Language, Subdomain, Domain Expert, Context Mapping
Tactical: Aggregate, Domain event, Bounded Context

## Chapter 2: Strategic Design with Bounded Contexts and the Ubiquitous Language

DDD is primarily about modeling a Ubiquitous langage in an explicit Bounded Context.

## Chapter 3: Strategic Design with Subdomains

Subdomains:
* Core domain
* Supporting domain
* Generic subdomain

## Chapter 4: Strategic design with Context Mapping

Context Mapping: when a core domain integrates with other bounded contexts (a line between bounded context)

A contex mapping implies an inter-team realtionship
Possible relationships:
* Partnership
* Shared Kernel
* Customer-Supplier

Anticorruption layer (implemented by the downstream team)
Open Host Service. API, Published Langage (exL json schema)

RPC (ex: SOAP)
RESTful
Messaging

Domain events are published by an aggregate and subscribed to by an interested bounded context.

## Chapter 5: Tactical Design with Aggregates

Aggregate

Value Object: it models an immutable conceptual whole. A value object is often used to describe, quantify or measure an Entity.

Entity: models an individual thing. Each entity has an unique indentifier. They are often mutable.

An aggregate is composed of one or more entities (one of them is the aggregate root) and can have value objects.

Each aggregate forms a transactional consistency boundary. Modify and commit only one aggregate instance in one transaction.

Design aggregates to be a sound encapsulation for unit testing.

Aggregate rule of thumb:
* Protect business invariants inside Aggregate boundaries
* Design small aggregates
* Reference other aggregates by identity only
* Update other aggregates using eventual consistency

"One possible way to find the correct business threshold is by presenting an exaggerated time frame (such as weeks or months) that is obsiously unacceptable. This will likely cause business experts to respond with an acceptable time frame."

## Chapter 6: Tactical Design with Domain Events

A domain event is a reccor of some business-significant occurrence in a Bounded Context.

Event sourcing: persisting all the domain events that have occured for an aggregate instance as a record of what changed about that Aggregated instance.

## Chapter 7: Acceleration and management tools

Event storming: a rapid design technique that is meant to engage both domain experts and developers in a fast-paced learning process.
1. Storm out the business process by creating a series of domain events on sticky notes.
2. Create the commands that cause each Domain event.
3. Associate the Entity/Aggregate on which the command is executed and that produces the domain event outcome.
4. [optional] Draw boundaries and lines without arrows to show on your modeling surface.
5. [optional] Identify the various views that your users will need to carry out their actions, and important roles for various users.
