---
Assignee: Nikita KatyshevMikhail Krasikov
Duration: Invalid date
Interview graded: true
Last edited time: 2023-11-29T18:04
Last recall: 2023-07-22
Needs Rework: false
Status: In progress
Topic:
  - "[[Message Brockers]]"
---
# **Kafka**

— распределённый программный брокер сообщений


Брокер сообщений - архитектурный паттерн в распределённых системах; приложение, которое преобразует сообщение по одному протоколу от приложения-источника в сообщение протокола приложения-приёмника, тем самым выступая между ними посредником. Кроме преобразования сообщений из одного формата в другой, в задачи брокера сообщений также входит:

- проверка сообщения на ошибки;
- маршрутизация конкретному приемнику(ам);
- разбиение сообщения на несколько маленьких, а затем агрегирование ответов приёмников и отправка результата источнику;
- сохранение сообщений в базе данных;
- вызов веб-сервисов;
- распространение сообщений подписчикам, если используются шаблоны типа издатель-подписчик.

Использование брокеров сообщений позволяет разгрузить веб-сервисы в распределённой системе, так как при отправке сообщений им не нужно тратить время на некоторые ресурсоёмкие операции типа маршрутизации и поиска приёмников. Кроме того, брокер сообщений для повышения эффективности может реализовывать стратегии упорядоченной рассылки и определение приоритетности, балансировать нагрузку и прочее.

[![](https://lh6.googleusercontent.com/2kwgI6Lkz6DfRugAQ4hOhcg1NF2cRP4LmmncBg9Cdn1nfPJUUCxdoUrnnvXBRvBw4nqmdTe7PZ65vsqe86CdEb1-fhrp7zJIITr32GWN-ikBhI3EjJ2F4nLT3lo-bwoT2WWV0cRZYQpg5OBifcTetP-AR9NgNX4dJ_b2fw3f3eVGDyYXuIj1tRs_8Lat)](https://lh6.googleusercontent.com/2kwgI6Lkz6DfRugAQ4hOhcg1NF2cRP4LmmncBg9Cdn1nfPJUUCxdoUrnnvXBRvBw4nqmdTe7PZ65vsqe86CdEb1-fhrp7zJIITr32GWN-ikBhI3EjJ2F4nLT3lo-bwoT2WWV0cRZYQpg5OBifcTetP-AR9NgNX4dJ_b2fw3f3eVGDyYXuIj1tRs_8Lat)

Системы очередей обычно состоят из трёх базовых компонентов:

1) сервер,

2) продюсеры, которые отправляют сообщения в некую именованную очередь, заранее сконфигурированную администратором на сервере,

3) консьюмеры, которые считывают те же самые сообщения по мере их появления.

[![](https://lh3.googleusercontent.com/fRtgu37rO7SuUOCJdXVzFXDrVEKvzgoH-WA6Gd9iKrHQ-0RFX83po-1YZFoUStrp7RJcTM-1m3samzIvDuGBd6wZtdch9oU-rmuD-FxG1HPzWyAh7qr2aJe1tzhgL2fkJ6UG_G3S2DzwbkauxzoyOrj3TR6yin3fPwALcYe1SJujoJ2QDshtyKGSdWYT)](https://lh3.googleusercontent.com/fRtgu37rO7SuUOCJdXVzFXDrVEKvzgoH-WA6Gd9iKrHQ-0RFX83po-1YZFoUStrp7RJcTM-1m3samzIvDuGBd6wZtdch9oU-rmuD-FxG1HPzWyAh7qr2aJe1tzhgL2fkJ6UG_G3S2DzwbkauxzoyOrj3TR6yin3fPwALcYe1SJujoJ2QDshtyKGSdWYT)

Потребители могут использовать 2 модели запросов:

- **pull-модель** — консьюмеры сами отправляют запрос раз в n секунд на сервер для получения новой порции сообщений(Kafka)
- **push-модель** — сервер делает запрос к клиенту, посылая ему новую порцию данных.(По такой модели, например, работает RabbitMQ.)

Жизненный цикл сообщений в системах очередей:

1. Продюсер отправляет сообщение на сервер.
2. Консьюмер фетчит (от англ. fetch — принести) сообщение и его уникальный идентификатор сервера.
3. Сервер помечает сообщение как in-flight. Сообщения в таком состоянии всё ещё хранятся на сервере, но временно не доставляются другим консьюмерам. Таймаут этого состояния контролируется специальной настройкой.
4. Консьюмер обрабатывает сообщение, следуя бизнес-логике. Затем отправляет ack или nack-запрос обратно на сервер, используя уникальный идентификатор, полученный ранее — тем самым либо подтверждая успешную обработку сообщения, либо сигнализируя об ошибке.
5. В случае успеха сообщение удаляется с сервера навсегда. В случае ошибки или таймаута состояния in-flight сообщение доставляется консьюмеру для повторной обработки.

[![](https://lh5.googleusercontent.com/Y6QSBTsyPypFvCFQg5ukfa7On93DFo75az8w_nEB1Xuo3guk0FTtxKyaTijIILJtW599-6RYBNfeMgfU22BpMfClVX7yu4ZRW90cdx03pgBIcd3kmCINSbeUFQaY0uCBoUe94uvYuj2RO3OQuD036jf1FeSkGpOlmLbBT9ZCQPd62PYaOku1uDVFPwQz)](https://lh5.googleusercontent.com/Y6QSBTsyPypFvCFQg5ukfa7On93DFo75az8w_nEB1Xuo3guk0FTtxKyaTijIILJtW599-6RYBNfeMgfU22BpMfClVX7yu4ZRW90cdx03pgBIcd3kmCINSbeUFQaY0uCBoUe94uvYuj2RO3OQuD036jf1FeSkGpOlmLbBT9ZCQPd62PYaOku1uDVFPwQz)

Фундаментальное отличие Kafka от очередей состоит в том, как сообщения хранятся на брокере и как потребляются консьюмерами.

- Сообщения в Kafka не удаляются брокерами по мере их обработки консьюмерами — данные в Kafka могут храниться днями, неделями, годами.
- Благодаря этому одно и то же сообщение может быть обработано сколько угодно раз разными консьюмерами и в разных контекстах.

Каждое сообщение (event или message) в Kafka состоит из ключа, значения, таймстампа и опционального набора метаданных (так называемых хедеров).

[![](https://lh6.googleusercontent.com/JWtX0xp4kOHARUhgNLrs81Li69DOEeJB9SabNYCtAa6pEZOUeFBury-YylsR4GzWg7betSAnAWIIex4g5x2SKBwnWFbmXjt9zheHfsSwmN52v8YDlzf7IN_kaW5KaCIbLWQ_-2CR8tPCxQBOdmxTGwAVzpiaapUMSt_xn3_sepLhXOgiX0hmyLZ4Fsc7)](https://lh6.googleusercontent.com/JWtX0xp4kOHARUhgNLrs81Li69DOEeJB9SabNYCtAa6pEZOUeFBury-YylsR4GzWg7betSAnAWIIex4g5x2SKBwnWFbmXjt9zheHfsSwmN52v8YDlzf7IN_kaW5KaCIbLWQ_-2CR8tPCxQBOdmxTGwAVzpiaapUMSt_xn3_sepLhXOgiX0hmyLZ4Fsc7)

[![](https://lh3.googleusercontent.com/Ep0vUPZvpNZmGMGmL9XpNVmCb6-FAZhCpyhyFsW8vXwIQWNPphqd9N-lYptBij9I00EFrulH1ckMJ9t9ckYkgyHyf0_hCBWkF_tTbbrmy9LoVTgvxqgPbx-4soMZgWtBfiIZEOlEqETMIOQd7BlEeJoqjrfx_PVFfBW1-iu7DCGDW8o1qf-9xPgFAWnr)](https://lh3.googleusercontent.com/Ep0vUPZvpNZmGMGmL9XpNVmCb6-FAZhCpyhyFsW8vXwIQWNPphqd9N-lYptBij9I00EFrulH1ckMJ9t9ckYkgyHyf0_hCBWkF_tTbbrmy9LoVTgvxqgPbx-4soMZgWtBfiIZEOlEqETMIOQd7BlEeJoqjrfx_PVFfBW1-iu7DCGDW8o1qf-9xPgFAWnr)

У каждой партиции есть «лидер» (Leader) — брокер, который работает с клиентами. Именно лидер работает с продюсерами и в общем случае отдаёт сообщения консьюмерам. К лидеру осуществляют запросы фолловеры (Follower) — брокеры, которые хранят реплику всех данных партиций. Сообщения всегда отправляются лидеру и, в общем случае, читаются с лиде

[![](https://lh4.googleusercontent.com/MU3m-CehTXMevlsx-iIShfvL2wF0nsz_04cseZmoQdEWD4sXGz91vZrIw-UfXPyijL8EA2B_iQ0YeAI1GF0BY75QYbXSghJvAsdP4cVyJf8HFH0QEufcoELhG2puvMUsvwAA0d1j7KI_aNbeTkn51ceamuw-Fad6d2K-ZFZYbZkSbwbavajWQREsD78r)](https://lh4.googleusercontent.com/MU3m-CehTXMevlsx-iIShfvL2wF0nsz_04cseZmoQdEWD4sXGz91vZrIw-UfXPyijL8EA2B_iQ0YeAI1GF0BY75QYbXSghJvAsdP4cVyJf8HFH0QEufcoELhG2puvMUsvwAA0d1j7KI_aNbeTkn51ceamuw-Fad6d2K-ZFZYbZkSbwbavajWQREsD78r)

Every message your producers send to a **Kafka** partition has an **offset**—a sequential index number that identifies each message. To keep track of which messages have already been processed, your consumer needs to commit the offsets of the messages that were processed.

There are different ways in which commit can happen and each way has its own pros andcons. Let’s see them in detail:

# Auto-committing messages

The default configuration of the consumer is to auto-commit messages. Consumer auto-commits the offset of the latest read messages at the configured interval of time. If we make enable.auto.commit = true and set auto.commit.interval.ms=2000 , then consumer will commit the offset every two seconds. There are certain risks associated with this option. For example, you set the interval to 10 seconds and consumer starts consuming the data. At the seventh second, your consumer fails, what will happen? Consumer hasn’t committed the read offset yet so when it starts again, it will start reading from the start of the last committed offset and this will lead to duplicates.

A message can be _**committed**_ once it is processed, so that no other consumers in the group read that message again (until you manually seek).

If the processing is a heavy operation, you can divide it into steps and write the intermediate results in intermediate topics.

If you are running multiple consumers as a group each of the consumers will get a subset of data (called topic partitions) to work on. So, no consumer belonging to the group interferes with another consumer in its group.

However, in case if one of the consumers in the group dies, any of the other consumers takeover its work from the point where it left off ([see Transactional processing](https://kafka.apache.org/23/javadoc/org/apache/kafka/clients/consumer/KafkaConsumer.html)) and process them.

You might be interested in having [_exactly-once processing_](https://www.confluent.io/blog/exactly-once-semantics-are-possible-heres-how-apache-kafka-does-it/) semantics where a message is processed only once.

![[../Software_Architecture/_img/Untitled 53.png|Untitled 53.png]]

[https://www.baeldung.com/java-kafka-streams-vs-kafka-consumer](https://www.baeldung.com/java-kafka-streams-vs-kafka-consumer)

RabbitMQ and Apache Kafka move data from producers to consumers in different ways. RabbitMQ is a general-purpose message broker that prioritizes end-to-end message delivery. Kafka is a distributed event streaming platform that supports the real-time exchange of continuous big data.

RabbitMQ and Kafka are designed for different use cases, which is why they handle messaging differently. Next, we discuss some specific differences.

### **Message consumption**

In RabbitMQ, the broker ensures that consumers receive the message. The consumer application takes a passive role and waits for the RabbitMQ broker to push the message into the queue. For example, a banking application might wait for SMS alerts from the central transaction processing software.

Kafka consumers, however, are more proactive in reading and tracking information. As messages are added to physical log files, Kafka consumers keep track of the last message they've read and update their offset tracker accordingly. An offset tracker is a counter that increments after reading a message. With Kafka, the producer is not aware of message retrieval by consumers.

### **Message priority**

RabbitMQ brokers allow producer software to escalate certain messages by using the priority queue. Instead of sending messages with the _first in, first out_ order, the broker processes higher priority messages ahead of normal messages. For example, a retail application might queue sales transactions every hour. However, if the system administrator issues a priority backup database message, the broker sends it immediately.

Unlike RabbitMQ, Apache Kafka doesn't support priority queues. It treats all messages as equal when distributing them to their respective partitions.

### **Message ordering**

RabbitMQ sends and queues messages in a specific order. Unless a higher priority message is queued into the system, consumers receive messages in the order they were sent.

Meanwhile, Kafka uses topics and partitions to queue messages. When a producer sends a message, it goes into a specific topic and partition. Because Kafka does not support direct producer-consumer exchanges, the consumer pulls messages from the partition in a different order.

### **Message deletion**

A RabbitMQ broker routes the message to the destination queue. Once read, the consumer sends an acknowledgement (ACK) reply to the broker, which then deletes the message from the queue.

Unlike RabbitMQ, Apache Kafka appends the message to a log file, which remains until its retention period expires. That way, consumers can reprocess streamed data at any time within the stipulated period.

# [Zookeeper](Zookeeper.md)