# **Concurrent Collections**
# Collections.synchronizedCollection()
As the name suggests, it returns a thread-safe collection backed up by the specified Collection.Synchronized collections achieve thread-safety through intrinsic locking, and the entire collections are locked. Intrinsic locking is implemented via synchronized blocks within the wrapped collection’s methods.
Synchronized collections assure data consistency/integrity in multi-threaded environments. However, they might come with a penalty in performance, as only one single thread can access the collection at a time
```java
Collection<Integer> syncCollection = Collections.synchronizedCollection(new ArrayList<>());
```
We have next subtypes:
- synchronizedList()
- synchronizedMap()
- synchronizedSortedMap()
- synchronizedSet()
- synchronizedSortedSet()
If we want to iterate over a synchronized collection and prevent unexpected results, we should explicitly provide our own thread-safe implementation of the loop. Hence, we could achieve that using a _synchronized_block:
```java
List<String> syncCollection = Collections.synchronizedList(Arrays.asList("a", "b", "c"));
List<String> uppercasedCollection = new ArrayList<>();
    
Runnable listOperations = () -> {
    synchronized (syncCollection) {
        syncCollection.forEach((e) -> {
            uppercasedCollection.add(e.toUpperCase());
        });
    }
};
```
This is because the iteration on a synchronized collection is performed through multiple calls into the collection. Therefore they need to be performed as a single atomic operation.
# Concurrent Collections
**Concurrent collections (e.g. _ConcurrentHashMap),_ achieve thread-safety by dividing their data into segments**. In a _ConcurrentHashMap_, for example, different threads can acquire locks on each segment, so multiple threads can access the _Map_ at the same time.
Concurrent collections are much more performant than synchronized collections
# BlockingQueue
Java BlockingQueue implementations are thread-safe. All queuing methods are atomic in nature and use internal locks or other forms of concurrency control.

Java BlockingQueue interface is part of java collections framework and it’s primarily used for implementing producer consumer problem. We don’t need to worry about waiting for the space to be available for producer or object to be available for consumer in BlockingQueue because it’s handled by implementation classes of BlockingQueue.
# ConcurrentHashMap
Отдельные коллекции, например ConcurrentHashMap. Достигают синхронизации с помощью деления коллекции на сегменты. То есть несколько потоков могут обратиться к map одновременно, но только к разным ее блокам. CopyOnWrite коллекции удобно использовать, когда write операции довольно редки, например при реализации механизма подписки listeners и прохода по ним.
# CopyOnWrite
Все операции по изменению коллекции (add, set, remove) приводят к созданию новой копии внутреннего массива. Тем самым гарантируется, что при проходе итератором по коллекции не кинется ConcurrentModificationException. Следует помнить, что при копировании массива копируются только референсы (ссылки) на объекты (shallow copy), т.ч. доступ к полям элементов не thread-safe.