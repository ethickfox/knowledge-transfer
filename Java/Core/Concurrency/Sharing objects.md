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
Publishing an object means making it available to code outside of its current scope, such as by storing a reference to it where other code can find it, returning it from a nonprivate method, or passing it to a method in another class.

The most blatant form of publication is to store a reference in a public static field, where any class and thread could see it,
Publishing an object also publishes any objects referred to by its nonprivate fields. More generally, any object that is reachable from a published object by following some chain of nonprivate field references and method calls has also been published.

``` java
public class ThisEscape {
  public ThisEscape(EventSource source) { source.registerListener(
    new EventListener() {
      public void onEvent(Event e) { doSomething(e);}
    });
  }
}
```

ThisEscape illustrates an important special case of escape—when the this references escapes during construction. When the inner EventListener instance is published, so is the enclosing ThisEscape instance. But an object is in a predictable, consistent state only after its constructor returns, so publishing an object from within its constructor can publish an incompletely constructed object. This is true even if the publication is the last statement in the constructor. If the this reference escapes during construction, the object is considered not properly constructed.

A common mistake that can let the this reference escape during construction is to start a thread from a constructor. When an object creates a thread from its constructor, it almost always shares its this reference with the new thread, either explicitly (by passing it to the constructor) or implicitly (because the Thread or Runnable is an inner class of the owning object). The new thread might then be able to see the owning object before it is fully constructed. There’s nothing wrong with creating a thread in a constructor, but it is best not to start the thread immediately. Instead, expose a start or initialize method that starts the owned thread.

# Thread conﬁnement
This technique, thread conﬁnement,is one of the simplest ways to achieve thread safety. When an object is conﬁned to a thread, such usage is automatically thread-safe even if the confined object itself is not

## Ad-hoc thread confinement
Ad-hoc thread conﬁnement describes when the responsibility for maintaining thread conﬁnement falls entirely on the implementation. Ad-hoc thread confinement can be fragile because none of the language features, such as visibility modiﬁers or local variables, helps confine the object to the target thread.
A special case of thread conﬁnement applies to volatile variables. It is safe to perform read-modify-write operations on shared volatile variables as long as you ensure that the volatile variable is only written from a single thread. In this case, you are conﬁning the modiﬁcation to a single thread to prevent race conditions, and the visibility guarantees for volatile variables ensure that other threads see the most up-to-date value.
Because of its fragility, ad-hoc thread conﬁnement should be used sparingly; if possible, use one of the stronger forms of thread confinment
## Stack confinement
Stack conﬁnement is a special case of thread conﬁnement in which an object can only be reached through local variables. Local variables are intrinsically conﬁned to the executing thread; they exist on the executing thread’s stack, which is not accessible to other threads.
Stack conﬁnement (also called within-thread or thread-local usage, but not to be confused with the ThreadLocal library class) is simpler to maintain and less fragile than ad-hoc thread confinement.
``` java
public int loadTheArk(Collection<Animal> candidates) {
  SortedSet<Animal> animals;
  int numPairs = 0;
  Animal candidate = null; // animals confined to method,
  // don’t let them escape!
  animals = new TreeSet<Animal>(new SpeciesGenderComparator());
  animals.addAll(candidates);
  for (Animal a : animals) {
    if (candidate == null || !candidate.isPotentialMate(a)) {
      candidate = a;
   } else {
    ark.load(new AnimalPair(candidate, a));
    ++numPairs;
    candidate = null;
    }
  }
  return numPairs;
}
```
Maintaining stack conﬁnement for object references requires a little more assistance from the programmer to ensure that the referent does not escape. In loadTheArk, we instantiate a TreeSet and store a reference to it in animals. At this point, there is exactly one reference to the Set, held in a local variable and therefore conﬁned to the executing thread. However, if we were to publish a reference to the Set (or any of its internals), the conﬁnement would be violated and the animals would escape.

# ThreadLocal
A more formal means of maintaining thread confinement is ThreadLocal,which allows you to associate a per-thread value with a value-holding object. ThreadLocal provides get and set accessor methods that maintain a separate copy of the value for each thread that uses it, so a get returns the most recent value passed to set from the currently executing thread.
``` java
private static ThreadLocal<Connection> connectionHolder = new ThreadLocal<Connection>() {
  public Connection initialValue() {
    return DriverManager.getConnection(DB_URL);
  }
};
public static Connection getConnection() { return connectionHolder.get();}
```





