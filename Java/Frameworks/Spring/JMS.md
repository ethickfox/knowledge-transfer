---
Interview graded: true
Last edited time: 2023-08-03T10:06
Needs Rework: false
Status: Not started
Topic:
  - "[Message Brockers](Message%20Brockers)"
  - "[Spring](Spring)"
---
## Configuration with java code

**The** _**JmsTemplate**_ **class handles the creation and release of resources when sending or synchronously receiving messages.**

As a result, the class that uses this _JmsTemplate_ only needs to implement callback interfaces as specified in the method definition.

Starting with Spring 4.1, the _JmsMessagingTemplate_ is built on top of _JmsTemplate,_ which provides an integration with the messaging abstraction, i.e., _org.springframework.messaging.Message._ This, in turn, allows us to create a message to send in a generic manner.

Sending messages  
  

```Java
public class SampleJmsMessageSender {

    private JmsTemplate jmsTemplate;
    private Queue queue;

    // setters for jmsTemplate & queue

    public void simpleSend() {
        jmsTemplate.send(queue, s -> s.createTextMessage("hello queue world"));
    }
}
```

Below is the message receiver class, the Message-Driven POJO (MDP). **We can see that the class** _**SampleListener**_ **is implementing the** _**MessageListener**_ **interface, and provides the text specific implementation for the interface method** _**onMessage().**_

Apart from the _onMessage()_ method, our _SampleListener_ class also called the method _receiveAndConvert()_ for receiving custom messages:

```Java
public class SampleListener implements MessageListener {

    public JmsTemplate getJmsTemplate() {
        return getJmsTemplate();
    }

    public void onMessage(Message message) {
        if (message instanceof TextMessage) {
            try {
                String msg = ((TextMessage) message).getText();
                System.out.println("Message has been consumed : " + msg);
            } catch (JMSException ex) {
                throw new RuntimeException(ex);
            }
        } else {
            throw new IllegalArgumentException("Message Error");
        }
    }

    public Employee receiveMessage() throws JMSException {
        Map map = (Map) getJmsTemplate().receiveAndConvert();
        return new Employee((String) map.get("name"), (Integer) map.get("age"));
    }
}
```

_**DefaultMessageListenerContainer**_ **is the default message listener container that Spring provides, along with many other specialized containers.**

## Configuration with Annotations

**The** _**@JmsListener**_ **is the only annotation required to convert a method of a normal bean into a JMS listener endpoint.** Spring JMS provides many more annotations to ease the JMS implementation.

We can see some of the sample classes annotated below:

```Java
@JmsListener(destination = "myDestination")
public void SampleJmsListenerMethod(Message<Order> order) { ... }
```

In order to add multiple listeners to a single method, we just need to add multiple _@JmsListener_ annotations.

**We'll need to add the** _**@EnableJms**_ **annotation to one of our configuration classes to support the** _**@JmsListener**_ **annotated methods:**

```Plain
@Configuration
@EnableJms
public classAppConfig {
```

## Durable with non durable Listeners

![Untitled 108.png](Untitled%20108.png)

- A **_durable subscriber_** establishes a durable subscription with a unique identity on the JMS provider. A _durable subscription_ allows subscribers to receive all the messages published on a topic, including those published while the subscriber is inactive (for example, if the JMS trigger is disabled). When the associated JMS trigger is disabled, the JMS provider holds the messages in guaranteed storage. If a durable subscription already exists for the specified durable subscriber on the JMS provider, this service resumes the subscription.
- A **_non-durable subscription_** allows subscribers to receive messages on their chosen topic only if the messages are published while the subscriber is active. A non-durable subscription lasts the lifetime of its message consumer. Note that non-durable subscribers cannot receive messages in a load-balanced fashion.

```Java
DefaultJmsListenerContainerFactory factory = new DefaultJmsListenerContainerFactory();
      factory.setConnectionFactory(connectionFactory);
      factory.setPubSubDomain(true);
      factory.setClientId("durableListener");
      factory.setSubscriptionDurable(true);
```

```Java
[Publisher|topic-task1] Test1
[NonDurableReceiver|topic-task1] Test1
[DurableReceiver|topic-task1] Test1
[Publisher|topic-task1] Test2
[DurableReceiver|topic-task1] Test2
[NonDurableReceiver|topic-task1] Test2
//Disable App
[DurableReceiver|topic-task1] Test 3
```

## Synchronous usage

![Untitled 1 34.png](Untitled%201%2034.png)

```Java
@Component
public class Requester {

   @Autowired
   @Qualifier("queuePublisherTemplate")
   private JmsTemplate topicJmsTemplate;

   @Value("${activemq.queue}")
   private String queue;

   public Message processRequest(String message) {
      return topicJmsTemplate.sendAndReceive(queue, messageCreator)
   }
}
```

The `Session` is linked to your send operation, it's not a singleton. And you need to have access to this `Session` to be able to create a `javax.jms.Message`.

There is no way for you to pass a `javax.jms.Message` to `JmsTemplate` because then you would need the session first to create it and that session is actually managed internally by `JmsTemplate`.

You should do things the other way around. You should have a method that returns a `MessageCreator` instead of the `Message` you would like to build. That method can be placed in a Spring bean and can have whatever injection you need. And since it's a method of yours you can pass whatever dynamic value that you want.

  

## **Virtual Topics**

![Untitled 2 25.png](Untitled%202%2025.png)

Virtual topics are a combination of topics and queues. Producers will write messages to a topic while listeners will consume from their own queue. ActiveMQ will cop each message from the topic to the actual consumer queues.

This combination of topics and queues has some advantages over conventional topics:

- Even if a consumer is offline, no messages will be lost. ActiveMQ will copy all messages to the registered queues. As soon as a consumer comes back online, it can handle the messages stored on his queue.
- If a consumer cannot handle a message, it will put on a dead letter queue. The consumer can be fixed and the message can be put back on his own dedicated queue without affecting the other consumer (as a topic would do).
- We can register multiple instances of a consumer on a queue to implement a load balancing mechanism.

  

1. When using virtual topics - there is no need for durable consumers. That is because for each client you will get an instance of regular queue created. If you have 5 clients (application A, B, C, D, E) you will get 5 queues created and populated with the copy of the messages every time message is sent to the virtual topic.
2. Actually it is a limitation of durable consumer - that only ONE connection is allowed per clientId. Being a regular queue, you can create as many consumers as you like and queue will guarantee that 1 message will be received only by 1 consumer. So if you have application A that takes 1 minute to process a message, you can create 5 instances of it listening to the same queue. When you will post 5 messages within 1 second, each of your application will receive its own message to process.
3. There are not well documented requirements which are not intuitive. To make virtual topic work you need
    - Use `VirtualTopic.` in your topic name, for example `VirtualTopic.Orders` (this prefix can be configured)
    - Use `Consumer.` in the name of the queue you. Like `Consumer.ApplicationA.VirtualTopic.Orders` where **ApplicationA** is actually your client id
    - Use regular subscribers not durable ones for the queue above.