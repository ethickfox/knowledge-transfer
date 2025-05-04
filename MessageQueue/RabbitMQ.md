RabbitMQ, and messaging in general, uses some jargon.

- **_Producing_** means nothing more than sending. A program that sends messages is a _producer_ :
- _A_ **_queue_** is the name for the post box in RabbitMQ. Although messages flow through RabbitMQ and your applications, they can only be stored inside a _queue_. A _queue_ is only bound by the host's memory & disk limits, it's essentially a large message buffer. Many _producers_ can send messages that go to one queue, and many _consumers_ can try to receive data from one _queue_.
- **_Consuming_** has a similar meaning to receiving. A _consumer_ is a program that mostly waits to receive messages:

### **Exchanges  
  
**

### One consumer

Publisher

```Java
amqpTemplate.convertAndSend(queue.getName(), customMessage);
```

Consumer

```Java
@RabbitListener(queues = "${rabbitmq.simple-queue}")
   public void simpleListenQueue(Message<CustomMessage> in) {
      System.out.println(in.getHeaders());
   }
```

### Several Consumers

![Untitled 91.png](Untitled%2091.png)

**Fair dispatch vs Round-robin dispatching**

- By default, RabbitMQ will send each message to the next consumer, in sequence. On average every consumer will get the same number of messages. This way of distributing messages is called round-robin. In this mode dispatching doesn't necessarily work exactly as we want. For example in a situation with two workers, when all odd messages are heavy and even messages are light, one worker will be constantly busy and the other one will do hardly any work. Well, RabbitMQ doesn't know anything about that and will still dispatch messages evenly. This happens because RabbitMQ just dispatches a message when the message enters the queue. It doesn't look at the number of unacknowledged messages for a consumer. It just blindly dispatches every n-th message to the n-th consumer.
- Fair dispatch" is the default configuration for Spring AMQP. With the prefetchCount set to 250 by default, this tells RabbitMQ not to give more than 250 messages to a worker at a time. Or, in other words, don't dispatch a new message to a worker while the number of unacked messages is 250. Instead, it will dispatch it to the next worker that is not still busy.

### Exchanges

The core idea in the messaging model in RabbitMQ is that the producer never sends any messages directly to a queue. Actually, quite often the producer doesn't even know if a message will be delivered to any queue at all.

Instead, the producer can only send messages to an _exchange_. An exchange is a very simple thing. On one side it receives messages from producers and the other side it pushes them to queues. The exchange must know exactly what to do with a message it receives. Should it be appended to a particular queue? Should it be appended to many queues? Or should it get discarded. The rules for that are defined by the _exchange type_.

![Untitled 1 22.png](Untitled%201%2022.png)

There are a few exchange types available: direct, topic, headers and fanout.

```Java
amqpTemplate.convertAndSend(fanout.getName(), "routingKey", customMessage);
```