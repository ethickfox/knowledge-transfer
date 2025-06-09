# Producer-Consumer Problem
The producer-consumer problem is a synchronization problem between different processes. 
There are three entities in this problem:
1. Producer - produces some items and pushes them into the memory buffer.
2. Consumer - consumes these items by popping them out of the buffer.
3. Memory buffer -  buffer is of fixed size. If it is full, the producer waits for the consumer to consume an item before pushing a new one. The producer and consumer cannot access the buffer at the same time – that is, it's mutually exclusive.

If the buffer is empty, then the consumer waits for the producer to push an item, which it consumes after the producer pushes it.