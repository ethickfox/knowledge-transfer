**Amazon Simple Queue Service (Amazon SQS)** is a fully managed message queuing service that enables you to decouple and scale microservices, distributed systems, and serverless applications. SQS eliminates the complexity and overhead associated with managing and operating message oriented middleware, and empowers developers to focus on differentiating work. Using SQS, you can send, store, and receive messages between software components at any volume, without losing messages or requiring other services to be available.

![Untitled 102.png](Untitled%20102.png)

Amazon Simple Queue Service (Amazon SQS) offers a secure, durable, and available hosted queue that lets you integrate and decouple distributed software systems and components. Amazon SQS offers common constructs such as dead-letter queues and cost allocation tags. It provides a generic web services API that you can access using any programming language that the AWS SDK supports.

  

Amazon SQS supports both standard and FIFO queues:

- Amazon SQS offers standard as the default queue type. Standard queues support a nearly unlimited number of API calls per second, per API action (SendMessage, ReceiveMessage, or DeleteMessage). Standard queues support at-least-once message delivery. However, occasionally (because of the highly distributed architecture that allows nearly unlimited throughput), more than one copy of a message might be delivered out of order. Standard queues provide best-effort ordering which ensures that messages are generally delivered in the same order as they're sent.
- FIFO (First-In-First-Out) queues are designed to enhance messaging between applications when the order of operations and events is critical, or where duplicates can't be tolerated.
- Maximum message size is 256kb

![Untitled 1 29.png](Untitled%201%2029.png)

### More about FIFO queues.

Where is FIFO the best choose:

- To make sure that user-entered commands are run in the right order.
- To display the correct product price by sending price modifications in the right order.
- To prevent a student from enrolling in a course before registering for an account.

FIFO queues also provide exactly-once processing but have a limited number of transactions per second (TPS).

FIFO message ordering.  
The FIFO queue improves upon and complements the standard queue. The most important features of this queue type are FIFO (First-In-First-Out) delivery and exactly-once processing:  
The order in which messages are sent and received is strictly preserved and a message is delivered once and remains available until a consumer processes and deletes it.  
Duplicates aren't introduced into the queue.  
In addition, FIFO queues support message groups that allow multiple ordered message groups within a single queue. There is no quota to the number of message groups within a FIFO queue.  
Amazon SQS dead-letter queues  
Amazon SQS supports dead-letter queues, which other queues (source queues) can target for messages that can't be processed (consumed) successfully. Dead-letter queues are useful for debugging your application or messaging system because they let you isolate problematic messages to determine why their processing doesn't succeed.  
Important:  
The dead-letter queue of a FIFO queue must also be a FIFO queue. Similarly, the dead-letter queue of a standard queue must also be a standard queue.  
Benefits of dead-letter queues.  
Configure an alarm for any messages delivered to a dead-letter queue.  
Examine logs for exceptions that might have caused messages to be delivered to a dead-letter queue.  
Analyze the contents of messages delivered to a dead-letter queue to diagnose software or the producer’s or consumer’s hardware issues.  
Determine whether you have given your consumer sufficient time to process messages  

### **Simple Notification Service**

![Untitled 2 21.png](Untitled%202%2021.png)

SNS provides message delivery from publishers to subscribers (also known as producers and consumers). Publishers communicate asynchronously with subscribers by sending messages to a topic, which is a logical access point and communication channel. Clients can subscribe to the SNS topic and receive published messages using a supported endpoint type, such as Amazon Kinesis Data Firehose, Amazon SQS, AWS Lambda, HTTP, email, mobile push notifications, and mobile text messages (SMS).

Amazon SNS provides the following features and capabilities:

- Application-to-application messaging
- Application-to-person notifications
- Standard and FIFO topics
- Message durability
- Message archiving and analytics
- Message attributes
- Message filtering

Next AWS services has integration with Amazon SNS out of the box:

- Amazon SQS
- AWS Lambda
- AWS Identity and Access Management (IAM)
- AWS CloudFormation

## Your goals

- Understand what is message queue
- Differentiate message queue and message bus.
- Know SQS and SNS use cases.
- Be able to explain how can you decouple data with SQS.