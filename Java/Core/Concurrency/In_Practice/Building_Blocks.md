# Synchronized collections
The synchronized collection classes include Vector and Hashtable, 1.2, the synchronized wrapper classes created by the Collections.synchronizedXxx factory methods.

The synchronized collections are thread-safe, but you may sometimes need to use additional client-side locking to guard compound actions.
Common compound actions on collections include iteration (repeatedly fetch elements until the collection is exhausted), navigation (ﬁnd the next element after this one according to some order), and conditional operations such as put-if-absent

An alternative to locking the collection during iteration is to clone the collection and iterate the copy instead. Since the clone is thread-conﬁned, no other thread can modify it during iteration, eliminating the possibility of ConcurrentModificationException.

## Hidden iterators
you have to remember to use locking everywhere a shared collection might be iterated.
The addTenThings method could throw ConcurrentModificationException, because the collection is being iterated by toString in the process of preparing the debugging message. Of course, the real problem is that HiddenIterator is not thread-safe; the HiddenIterator lock should be acquired before using set in the println call, but debugging and logging code commonly neglect to do this.
``` java
@GuardedBy("this")
 private final Set<Integer> set = new HashSet<Integer>();
 public synchronized void add(Integer i) { set.add(i); } public synchronized void remove(Integer i) { set.remove(i); }  public void addTenThings() {
Random r = new Random();
for (int i = 0; i < 10; i++) add(r.nextInt());
System.out.println("DEBUG: added ten elements to " + set); }
}
```
Iteration is also indirectly invoked by the collection’s hashCode and equals methods, which may be called if the collection is used as an element or key of another collection. Similarly, the containsAll, removeAll,and retainAll methods, as well as the constructors that take collections as arguments, also iterate the collection. All of these indirect uses of iteration can cause ConcurrentModificationException.
# Concurrent collections
Replacing synchronized collections with concurrent collections can oﬀer dramatic scalability improvements with little risk.
- BlockingQueue extends Queue to add blocking insertion and retrieval operations. If the queue is empty, a retrieval blocks until an element is available, and if the queue is full (for bounded queues) an insertion blocks until there is space available. Blocking queues are extremely useful in producer-consumer designs,
- ConcurrentHashMap is uses a finer-grained locking mechanism called lock striping. to allow a greater degree of shared access. Arbitrarily many reading threads can access the map concurrently, readers can access the map concurrently with writers, and a limited number of writers can modify the map concurrently. along with the other concurrent collections, further improve on the synchronized collection classes by providing iterators that do not throw ConcurrentModificationException, thus eliminating the need to lock the collection during iteration. The iterators returned by ConcurrentHashMap are weakly consistent instead of fail-fast. A weakly consistent iterator can tolerate concurrent modiﬁcation, traverses elements as they existed when the iterator was constructed, and may (but is not guaranteed to) reflect modiﬁcations to the collection after the construction of the iterator.
   - Since the result of size could be out of date by the time it is computed, it is really only an estimate, so size is allowed to return an approximation instead of an exact count. While at first this may seem disturbing, in reality methods like size and isEmpty are far less useful in concurrent environments because these quantities are moving targets. So the requirements for these operations were weakened to enable performance optimizations for the most important operations, primarily get, put, containsKey,and remove.
   - Only if your application needs to lock the map for exclusive access3 is ConcurrentHashMap not an appropriate drop-in replacement for synchronized Maps
- CopyOnWriteArrayList is a concurrent replacement for a synchronized List that offers better concurrency in some common situations and eliminates the need to lock or copy the collection during iteration.Iterators for the copy-on-write collections retain a reference to the backing array that was current at the start of iteration, and since this will never change, they need to synchronize only brieﬂy to ensure visibility of the array contents. As a result, multiple threads can iterate the collection without interference from one another or from threads wanting to modify the collection. large; the copy-on-write collections are reasonable to use only when iteration is far more common than modiﬁcation.
- Blocking queues provide blocking put and take methods as well as the timed equivalents offer and poll. If the queue is full, put blocks until space becomes available; if the queue is empty, take blocks until an element is available. Queues can be bounded or unbounded; unbounded queues are never full, so a put on an unbounded queue never blocks.One of the most common producer-consumer designs is a thread pool coupled with a work queue; this pattern is embodied in the Executor task execution framework. For mutable objects, producer-consumer designs and blocking queues facilitate serial thread confinement for handing off ownership of objects from producers to consumers. A thread-conﬁned object is owned exclusively by a single thread, but that ownership can be “transferred” by publishing it safely where only one other thread will gain access to it and ensuring that the publishing thread does not access it after the handoff.
- deques lend themselves to a related pattern called work stealing. A producerconsumer design has one shared work queue for all consumers; in a work stealing design, every consumer has its own deque. If a consumer exhausts the work in its own deque, it can steal work from the tail of someone else’s deque.

# Synchronizers
A synchronizer is any object that coordinates the control flow of threads based on its state.
## Latches

