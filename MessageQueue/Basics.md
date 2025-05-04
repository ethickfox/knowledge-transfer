### **Messaging Queue.**

![Untitled 50.png](../Software_Architecture/_img/Untitled%2050.png)

![Untitled 1 14.png](../Software_Architecture/_img/Untitled%201%2014.png)

  

![Untitled 2 10.png](Untitled%202%2010.png)

![Untitled 3 10.png](Untitled%203%2010.png)

![Untitled 4 8.png](Untitled%204%208.png)

A message queue is a form of asynchronous inter-service communication. Messages are stored on the queue until they are processed and deleted. Each message is processed only once, by a single consumer. Message queues can be used to decouple heavyweight processing, to buffer or batch work, and to smooth spiky workloads. Common messaging tools are Kafka, RabbitMQ, AWS SQS. A message queue is a queue of messages sent between services/applications.

  

**Message** is some information that is produced by a producer application in byte form, stored in a queue and is consumed by a consumer application.

![Untitled 5 8.png](Untitled%205%208.png)

  

**Dead-letter-queue -** Where all invalid messages go

![Untitled 6 8.png](Untitled%206%208.png)

![Untitled 7 6.png](Untitled%207%206.png)

  

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

![Untitled 8 6.png](Untitled%208%206.png)

![Untitled 9 6.png](Untitled%209%206.png)

![Untitled 10 6.png](Untitled%2010%206.png)

![Untitled 11 6.png](Untitled%2011%206.png)

![Untitled 12 6.png](Untitled%2012%206.png)

Message brockers better not to use in kubernetes, because both can recover themself from fail

![Untitled 13 6.png](Untitled%2013%206.png)

  

## Interservice communication patterns

### Point To Point

![Untitled 14 6.png](Untitled%2014%206.png)

  

![Untitled 15 5.png](../Software_Architecture/_img/Untitled%2015%205.png)

![Untitled 16 5.png](Untitled%2016%205.png)

![Untitled 17 4.png](Untitled%2017%204.png)

### Publish Subscribe

![Untitled 18 4.png](Untitled%2018%204.png)

![Untitled 19 4.png](../Software_Architecture/_img/Untitled%2019%204.png)

![Untitled 20 3.png](Untitled%2020%203.png)

![Untitled 21 3.png](Untitled%2021%203.png)

  

## Event-Driven Microservices Pattern

![Untitled 22 3.png](Untitled%2022%203.png)

### Choreographed Events

![Untitled 23 3.png](../Software_Architecture/_img/Untitled%2023%203.png)

![Untitled 24 3.png](../Software_Architecture/_img/Untitled%2024%203.png)

![Untitled 25 3.png](Untitled%2025%203.png)

![Untitled 26 2.png](Untitled%2026%202.png)

![Untitled 27 2.png](../Software_Architecture/_img/Untitled%2027%202.png)

### Orchestrated Events

![Untitled 28 2.png](../Software_Architecture/_img/Untitled%2028%202.png)

![Untitled 29 2.png](../Software_Architecture/_img/Untitled%2029%202.png)

![Untitled 30 2.png](Untitled%2030%202.png)

![Untitled 31 2.png](../Software_Architecture/_img/Untitled%2031%202.png)

### Hybrid Events

![Untitled 32 2.png](../Software_Architecture/_img/Untitled%2032%202.png)

![Untitled 33 2.png](../Software_Architecture/_img/Untitled%2033%202.png)

## Stream Data Platform

![Untitled 34 2.png](../Software_Architecture/_img/Untitled%2034%202.png)

![Untitled 35 2.png](../Software_Architecture/_img/Untitled%2035%202.png)

![Untitled 36 2.png](Untitled%2036%202.png)

  

## Data Flows

![Untitled 37 2.png](../Software_Architecture/_img/Untitled%2037%202.png)

![Untitled 38 2.png](../Software_Architecture/_img/Untitled%2038%202.png)

### Eventual Consistency

![Untitled 39 2.png](../Software_Architecture/_img/Untitled%2039%202.png)

![Untitled 40 2.png](../Software_Architecture/_img/Untitled%2040%202.png)

![Untitled 41 2.png](../Software_Architecture/_img/Untitled%2041%202.png)

### CQRS

![Untitled 42 2.png](../Software_Architecture/_img/Untitled%2042%202.png)

![Untitled 43 2.png](Untitled%2043%202.png)

### Data Synchronization

![Untitled 44 2.png](Untitled%2044%202.png)