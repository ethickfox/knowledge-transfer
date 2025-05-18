### **Messaging Queue.**
1. [ActiveMQ](Message_Queue/ActiveMQ.md)
2. [RabbitMQ](Message_Queue/RabbitMQ.md)
3. [Kafka](Message_Queue/Kafka.md)

![88.png](../Software_Architecture/_img/88.png)

![12.png](../Software_Architecture/_img/12.png)

  ![Producer](Message_Queue/_img/MessageBrocker.canvas)

![4.png](_img/4.png)

![Untitled 3 10.png](Untitled%203%2010.png)

![28.png](_img/28.png)

A message queue is a form of asynchronous inter-service communication. Messages are stored on the queue until they are processed and deleted. Each message is processed only once, by a single consumer. Message queues can be used to decouple heavyweight processing, to buffer or batch work, and to smooth spiky workloads. Common messaging tools are Kafka, RabbitMQ, AWS SQS. A message queue is a queue of messages sent between services/applications.

  

**Message** is some information that is produced by a producer application in byte form, stored in a queue and is consumed by a consumer application.

![7.png](_img/7.png)

  

**Dead-letter-queue -** Where all invalid messages go

![8.png](_img/8.png)

![9.png](_img/9.png)

  

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

![25.png](_img/25.png)

![27.png](_img/27.png)

![30.png](_img/30.png)

![10.png](_img/10.png)

![13.png](_img/13.png)

Message brockers better not to use in kubernetes, because both can recover themself from fail

![23.png](_img/23.png)

  

## Interservice communication patterns

### Point To Point

![14.png](_img/14.png)

  

![72.png](../Software_Architecture/_img/72.png)

![20.png](_img/20.png)

![27.png](../DevOps/_img/27.png)

### Publish Subscribe

![Untitled 18 4.png](Untitled%2018%204.png)

![60.png](../Software_Architecture/_img/60.png)

![99.png](../DevOps/_img/99.png)

![15.png](_img/15.png)

  

## Event-Driven Microservices Pattern

![70.png](../DevOps/_img/70.png)

### Choreographed Events

![128.png](../Software_Architecture/_img/128.png)

![136.png](../Software_Architecture/_img/136.png)

![101.png](../DevOps/_img/101.png)

![22.png](_img/22.png)

![138.png](../Software_Architecture/_img/138.png)

### Orchestrated Events

![124.png](../Software_Architecture/_img/124.png)

![131.png](../Software_Architecture/_img/131.png)

![17.png](_img/17.png)

![120.png](../Software_Architecture/_img/120.png)

### Hybrid Events

![118.png](../Software_Architecture/_img/118.png)

![116.png](../Software_Architecture/_img/116.png)

## Stream Data Platform

![114.png](../Software_Architecture/_img/114.png)

![104.png](../Software_Architecture/_img/104.png)

![16.png](_img/16.png)

  

## Data Flows

![99.png](../Software_Architecture/_img/99.png)

![113.png](../Software_Architecture/_img/113.png)

### Eventual Consistency

![110.png](../Software_Architecture/_img/110.png)

![93.png](../Software_Architecture/_img/93.png)

![91.png](../Software_Architecture/_img/91.png)

### CQRS

![90.png](../Software_Architecture/_img/90.png)

![19.png](_img/19.png)

### Data Synchronization

![18.png](_img/18.png)