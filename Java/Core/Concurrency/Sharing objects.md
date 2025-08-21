# Sharing Objects
We want not only to prevent one thread from modifying the state of an object when another is using it, but also to ensure that when a thread modiﬁes the state of an object, other threads can actually see the changes that were made. But without synchronization, this may not happen. You can ensure that objects are published safely either by using explicit synchronization or by taking advantage of the synchronization built into library classes.

## Visibility
In order to ensure visibility of memory writes across threads, you must use synchronization.

There is no guarantee that reading thread will see variable written by another thread on a timely basis or even at all. In order to ensure visibility of memory writes across threads, you must use synchronisation

In this case program might output 42, 0 or stuck

``` java
public class NoVisibility {
  private static boolean ready;
  private static int number;

  private static class ReaderThread extends Thread {
    public void run() {
      while (!ready)
      Thread.yield();
      System.out.println(number);
    }
  }
  public static void main(String[] args) {
    new ReaderThread().start();
    number = 42;
    ready = true;
  }
}
```
NoVisibility could loop forever because the value of ready might never become visible to the reader thread. Even more strangely, NoVisibility could print zero because the write to ready might be made visible to the reader thread before the write to number, a phenomenon known as reordering.
This might happen due to **reordering.** In order to have a better performance compiler might cache and reorder operations. When the main thread writes first to number and then to ready without synchronization, the reader thread could see these operations in different order or not at all.

to avoid these complex issues: always use the proper synchronization whenever data is shared across threads.

## Stale data
When the reader thread examines ready, it may see an out-of-date value. Unless synchronization is used every time a variable is accessed, it is possible to see a stale value for that variable. Worse, staleness is not all-or-nothing: a thread can see an up-to-date value of one variable but a stale value of another variable that was written first.

64 bit variables(double, long) that are not marked as volatile are read and written in two 32 bit operations.
## Locking and visibility
Locking is also about memory visibility

We can now give the other reason for the rule requiring all threads to synchronize on the same lock when accessing a shared mutable variable—to guarantee that values written by one thread are made visible to other threads. Otherwise, if a thread reads a variable without holding the appropriate lock, it might see a stale value.

Locking is not just about mutual exclusion; it is also about memory visibility. To ensure that all threads see the most up-to-date values of shared mutable variables, the reading and writing threads must synchronize on a common lock.

## Volatile variables
volatile - means the variable is shared by different threads and operations on that variable shouldn’t be reordered and cached. From a memory perspective writing volatile variable is like exiting synchronization block and reading - is entering.

accessing a volatile variable performs no locking and so cannot cause the executing thread to block, making volatile variables a lighter-weight synchronization mechanism than synchronized.
Use volatile variables only when they simplify implementing and verifying your synchronization policy. Good use is checking a status flag to determine when to exit the loop.

Semantics of volatile are not strong enough to perform increment operation(count++) atomic,
Locking can guarantee both - visibility and atomacity, while volatile only visibility

You can use volatile variable only when all criteria are met:

1. Writes to the variable don’t depend on it’s current state(count++), or you can ensure that only a single thread ever update the value
2. The variable doesn’t participate in invariants with other state variables
3. Locking is not required for any other reason while the variable is being accessed

  
# Publication and escape
