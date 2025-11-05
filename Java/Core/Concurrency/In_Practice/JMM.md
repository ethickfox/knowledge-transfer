# Java Memory Model
```
aVariable = 3; 
```
A memory model addresses the question “Under what conditions does a  thread that reads aVariable see the value 3?”
The Java Language Specification requires the JVM to maintain withinthread as-if-serial semantics: as long as the program has the same result as if it  were executed in program order in a strictly sequential environment, all these  games are permissible.
The Java Memory Model is specified in terms of actions, which include reads and  writes to variables, locks and unlocks of monitors, and starting and joining with threads.

The JMM defines a partial ordering2 called happens-before on all actions  within the program. To guarantee that the thread executing action B can see the  results of action A (whether or not A and B occur in different threads), there must  be a happens-before relationship between A and B. In the absence of a happens-before  ordering between two operations, the JVM is free to reorder them as it pleases. 


### Platform memory models 
memory. Processor architectures provide  varying degrees of cache coherence; some provide minimal guarantees that allow  different processors to see different values for the same memory location at virtually any time. The operating system, compiler, and runtime (and sometimes, the  program, too) must make up the difference between what the hardware provides  and what thread safety requires. 
An  architecture’s memory model tells programs what guarantees they can expect from  the memory system, and specifies the special instructions required (called memory  barriers or fences) to get the additional memory coordination guarantees required  when sharing data.

One convenient mental model for program execution is to imagine that there  is a single order in which the operations happen in a program, regardless of  what processor they execute on, and that each read of a variable will see the  last write in the execution order to that variable by any processor. This happy,  if unrealistic, model is called sequential consistency.
### Reordering 
the JMM can permit actions to appear to execute in different orders from the perspective of different threads, making reasoning about  ordering in the absence of synchronization even more complicated. The various  reasons why operations might be delayed or appear to execute out of order can  all be grouped into the general category of reordering. 
The actions  in each thread have no dataflow dependence on each other, and accordingly can  be executed out of order. (Even if they are executed in order, the timing by which  caches are flushed to main memory can make it appear, from the perspective of  B, that the assignments in A occurred in the opposite order.)

It is prohibitively difficult to reason about ordering  in the absence of synchronization; it is much easier to ensure that your program  uses synchronization appropriately. Synchronization inhibits the compiler, runtime, and hardware from reordering memory operations in ways that would violate the visibility guarantees provided by the JMM.
A data race occurs when a variable is read by more than one thread, and written  by at least one thread, but the reads and writes are not ordered by happens-before.  A correctly synchronized program is one with no data races; correctly synchronized  programs exhibit sequential consistency, meaning that all actions within the program appear to happen in a fixed, global order. 

The rules for happens-before are:  Program order rule. Each action in a thread happens-before every action in that thread that comes later in the program order.  Monitor lock rule. An unlock on a monitor lock happens-before every  subsequent lock on that same monitor lock.3  Volatile variable rule. A write to a volatile field happens-before every  subsequent read of that same field.4  Thread start rule. A call to Thread.start on a thread happens-before  every action in the started thread.  Thread termination rule. Any action in a thread happens-before any  other thread detects that thread has terminated, either by successfully return from Thread.join or by Thread.isAlive returning  false.  Interruption rule. A thread calling interrupt on another thread  happens-before the interrupted thread detects the interrupt (either  by having InterruptedException thrown, or invoking isInterrupted or interrupted).  Finalizer rule. The end of a constructor for an object happens-before  the start of the finalizer for that object.  Transitivity. If A happens-before B, and B happens-before C, then A  happens-before C. 

### Unsafe publication 
The possibility of reordering in the absence of a happens-before relationship explains why publishing an object without adequate synchronization can allow another thread to see a partially constructed object
Unsafe publication can happen as a result of an incorrect lazy initialization,
```
public class UnsafeLazyInitialization {
  private static Resource resource;
  public static Resource getInstance() {
    if (resource == null)
      resource = new Resource();
    // unsafe publication
    return resource;
  }
} 
```
Suppose thread A is the first to invoke getInstance. It sees that resource is  null, instantiates a new Resource, and sets resource to reference it. When thread  B later calls getInstance, it might see that resource already has a non-null value  and just use the already constructed Resource. This might look harmless at first,  but there is no happens-before ordering between the writing of resource in A and the  reading of resource in B. A data race has been used to publish the object, and  therefore B is not guaranteed to see the correct state of the Resource. 

Since neither thread used synchronization, B could possibly see A’s actions in a different order than A performed them. So even though A initialized  the Resource before setting resource to reference it, B could see the write to  resource as occurring before the writes to the fields of the Resource. B could thus  see a partially constructed Resource that may well be in an invalid state—and  whose state may unexpectedly change later. 

With the exception of immutable objects, it is not safe to use an object that  has been initialized by another thread unless the publication happensbefore the consuming thread uses it. 

### Safe publication 
ensure that the published  object is visible to other threads because they ensure the publication happensbefore the consuming thread loads a reference to the published object.
The lazy initialization holder class idiom [EJ Item 48] in Listing 16.6 uses a  class whose only purpose is to initialize the Resource. The JVM defers initializing the ResourceHolder class until it is actually used [JLS 12.4.1], and because the  Resource is initialized with a static initializer, no additional synchronization is  needed. The first call to getResource by any thread causes ResourceHolder to be  loaded and initialized, at which time the initialization of the Resource happens  through the static initializer. 

```
public class ResourceFactory {
  private static class ResourceHolder {
    public static Resource resource = new Resource();
  }

  public static Resource getResource() {
    return ResourceHolder.resource;
  }
} 
```
### Double-checked locking 
The common code path—fetching a reference to an  already constructed Resource—doesn’t use synchronization. And that’s where  the problem is: as described in Section 16.2.1, it is possible for a thread to see a  partially constructed Resource. 
The real problem with DCL is the assumption that the worst thing that can  happen when reading a shared object reference without synchronization is to  erroneously see a stale value (in this case, null); in that case the DCL idiom  compensates for this risk by trying again with the lock held. But the worst case is  actually considerably worse—it is possible to see a current value of the reference  but stale values for the object’s state, meaning that the object could be seen to be  in an invalid or incorrect state. 
Subsequent changes in the JMM (Java 5.0 and later) have enabled DCL to work  if resource is made volatile, and the performance impact of this is small since  volatile reads are usually only slightly more expensive than nonvolatile reads. 

```
public class DoubleCheckedLocking {
  private static Resource resource;
  public static Resource getInstance() {
    if (resource == null) {
      synchronized (DoubleCheckedLocking.class) {
        if (resource == null)
          resource = new Resource();
      }
   }
   return resource;
  }
} 
```
However, this is an idiom whose utility has largely passed—the forces that motivated it (slow uncontended synchronization, slow JVM startup) are no longer  in play, making it less effective as an optimization. The lazy initialization holder  idiom offers the same benefits and is easier to understand. 

### Initialization safety 
The guarantee of initialization safety allows properly constructed immutable objects  to be safely shared across threads without synchronization, regardless of how  they are published—even if published using a data race.
Without initialization safety, supposedly immutable objects like String can  appear to change their value if synchronization is not used by both the publishing  and consuming threads.
Initialization safety guarantees that for properly constructed objects, all  threads will see the correct values of final fields that were set by the constructor, regardless of how the object is published. Further, any variables  that can be reached through a final field of a properly constructed object  (such as the elements of a final array or the contents of a HashMap referenced by a final field) are also guaranteed to be visible to other threads.6 
For objects with final fields, initialization safety prohibits reordering any part  of construction with the initial load of a reference to that object. All writes to final  fields made by the constructor, as well as to any variables reachable through those  fields, become “frozen” when the constructor completes, and any thread that  obtains a reference to that object is guaranteed to see a value that is at least as up  to date as the frozen value. Writes that initialize variables reachable through final  fields are not reordered with operations following the post-construction freeze. 

Initialization safety means that SafeStates in Listing 16.8 could be safely published even through unsafe lazy initialization or stashing a reference to a SafeStates in a public static field with no synchronization, even though it uses no  synchronization and relies on the non-thread-safe HashSet. 

```
@ThreadSafe  public class SafeStates {  private final Map<String, String> states;  public SafeStates() {  states = new HashMap<String, String>();  states.put("alaska", "AK");  states.put("alabama", "AL");  ...  states.put("wyoming", "WY");  }  public String getAbbreviation(String s) {  return states.get(s);  }  } 

```
However, a number of small changes to SafeStates would take away its  thread safety. If states were not final, or if any method other than the constructor  modified its contents, initialization safety would not be strong enough to safely  access SafeStates without synchronization. If SafeStates had other nonfinal  fields, other threads might still see incorrect values of those fields. And allowing the object to escape during construction invalidates the initialization-safety  guarantee. 

Initialization safety makes visibility guarantees only for the values that  are reachable through final fields as of the time the constructor finishes.  For values reachable through nonfinal fields, or values that may change  after construction, you must use synchronization to ensure visibility. 


