---
Interview graded: true
Last edited time: 2023-07-23T12:38
Last recall: 2023-07-22
Needs Rework: false
Status: Not started
Topic:
  - "[[Message Brockers]]"
---
### **Messaging Queue.**

![[../Software_Architecture/_img/Untitled 50.png|Untitled 50.png]]

![[../Software_Architecture/_img/Untitled 1 14.png|Untitled 1 14.png]]

  

![[Untitled 2 10.png|Untitled 2 10.png]]

![[Untitled 3 10.png|Untitled 3 10.png]]

![[Untitled 4 8.png|Untitled 4 8.png]]

A message queue is a form of asynchronous inter-service communication. Messages are stored on the queue until they are processed and deleted. Each message is processed only once, by a single consumer. Message queues can be used to decouple heavyweight processing, to buffer or batch work, and to smooth spiky workloads. Common messaging tools are Kafka, RabbitMQ, AWS SQS. A message queue is a queue of messages sent between services/applications.

  

**Message** is some information that is produced by a producer application in byte form, stored in a queue and is consumed by a consumer application.

![[Untitled 5 8.png|Untitled 5 8.png]]

  

**Dead-letter-queue -** Where all invalid messages go

![[Untitled 6 8.png|Untitled 6 8.png]]

![[Untitled 7 6.png|Untitled 7 6.png]]

  

Services communicate using either synchronous protocols such as HTTP/REST or asynchronous protocols such as AMQP.

Each service has its own database in order to be decoupled from other services.

**API gateway**

An API gateway is an API management tool that sits between a client and a collection of backend services.

An API gateway acts as a reverse proxy to accept all application programming interface (API) calls, aggregate the various services required to fulfill them, and return the appropriate result.

**Trade offs**

- Complexity
    - Artifact sprawl
    - Additional components
    - Operational runbook
    - Issue source identification
- Observability
    - Lack of immidiate response
    - log aggregation
    - metrics correlation

![[Untitled 8 6.png|Untitled 8 6.png]]

![[Untitled 9 6.png|Untitled 9 6.png]]

![[Untitled 10 6.png|Untitled 10 6.png]]

![[Untitled 11 6.png|Untitled 11 6.png]]

![[Untitled 12 6.png|Untitled 12 6.png]]

Message brockers better not to use in kubernetes, because both can recover themself from fail

![[Untitled 13 6.png|Untitled 13 6.png]]

  

## Interservice communication patterns

### Point To Point

![[Untitled 14 6.png|Untitled 14 6.png]]

  

![[../Software_Architecture/_img/Untitled 15 5.png|Untitled 15 5.png]]

![[Untitled 16 5.png|Untitled 16 5.png]]

![[Untitled 17 4.png|Untitled 17 4.png]]

### Publish Subscribe

![[Untitled 18 4.png|Untitled 18 4.png]]

![[../Software_Architecture/_img/Untitled 19 4.png|Untitled 19 4.png]]

![[Untitled 20 3.png|Untitled 20 3.png]]

![[Untitled 21 3.png|Untitled 21 3.png]]

  

## Event-Driven Microservices Pattern

![[Untitled 22 3.png|Untitled 22 3.png]]

### Choreographed Events

![[../Software_Architecture/_img/Untitled 23 3.png|Untitled 23 3.png]]

![[../Software_Architecture/_img/Untitled 24 3.png|Untitled 24 3.png]]

![[Untitled 25 3.png|Untitled 25 3.png]]

![[Untitled 26 2.png|Untitled 26 2.png]]

![[../Software_Architecture/_img/Untitled 27 2.png|Untitled 27 2.png]]

### Orchestrated Events

![[../Software_Architecture/_img/Untitled 28 2.png|Untitled 28 2.png]]

![[../Software_Architecture/_img/Untitled 29 2.png|Untitled 29 2.png]]

![[Untitled 30 2.png|Untitled 30 2.png]]

![[../Software_Architecture/_img/Untitled 31 2.png|Untitled 31 2.png]]

### Hybrid Events

![[../Software_Architecture/_img/Untitled 32 2.png|Untitled 32 2.png]]

![[../Software_Architecture/_img/Untitled 33 2.png|Untitled 33 2.png]]

## Stream Data Platform

![[../Software_Architecture/_img/Untitled 34 2.png|Untitled 34 2.png]]

![[../Software_Architecture/_img/Untitled 35 2.png|Untitled 35 2.png]]

![[Untitled 36 2.png|Untitled 36 2.png]]

  

## Data Flows

![[../Software_Architecture/_img/Untitled 37 2.png|Untitled 37 2.png]]

![[../Software_Architecture/_img/Untitled 38 2.png|Untitled 38 2.png]]

### Eventual Consistency

![[../Software_Architecture/_img/Untitled 39 2.png|Untitled 39 2.png]]

![[../Software_Architecture/_img/Untitled 40 2.png|Untitled 40 2.png]]

![[../Software_Architecture/_img/Untitled 41 2.png|Untitled 41 2.png]]

### CQRS

![[../Software_Architecture/_img/Untitled 42 2.png|Untitled 42 2.png]]

![[Untitled 43 2.png|Untitled 43 2.png]]

### Data Synchronization

![[Untitled 44 2.png|Untitled 44 2.png]]