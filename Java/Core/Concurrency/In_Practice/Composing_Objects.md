# Composing Objects
The design process for a thread-safe class should include these three basic elements:
• Identify the variables that form the object’s state;
• Identify the invariants that constrain the state variables;
• Establish a policy for managing concurrent access to the object’s state.

Objects and variables have a state space: the range of possible states they can take on. The smaller this state space, the easier it is to reason about. By using final fields wherever practical, you make it simpler to analyze the possible states an object can be in.

Some objects also have methods with state-based preconditions. For example, you cannot remove an item from an empty queue; a queue must be in the “nonempty” state before you can remove an element. Operations with state-based preconditions are called state-dependent
In many cases, ownership and encapsulation go together—the object encapsulates the state it owns and owns the state it encapsulates. It is the owner of a given state variable that gets to decide on the locking protocol used to maintain the integrity of that variable’s state.
state. Ownership implies control, but once you publish a reference to a mutable object, you no longer have exclusive control; at best, you might have “shared ownership”.

# Instance conﬁnement
If an object is not thread-safe, several techniques can still let it be used safely in a multithreaded program. You can ensure that it is only accessed from a single thread (thread confinement), or that all access to it is properly guarded by a lock.

Encapsulation simpliﬁes making classes thread-safe by promoting instance confinement, often just called conﬁnement

is still possible to violate conﬁnement by publishing a supposedly conﬁned object; if an object is intended to be conﬁned to a speciﬁc scope, then letting it escape from that scope is a bug. Conﬁned objects can also escape by publishing other objects such as iterators or inner class instances that may indirectly publish the conﬁned objects.

Following the principle of instance conﬁnement to its logical conclusion leads you to the Java monitor pattern.2 An object following the Java monitor pattern encapsulates all its mutable state and guards it with the object’s own intrinsic lock.
``` java
public class PrivateLock {
    private final Object myLock = new Object();
    @GuardedBy("myLock") Widget widget;
    void someMethod() {
        synchronized(myLock) {
        // Access or modify the state of widget }
    }
}
```
There are advantages to using a private lock object instead of an object’s intrinsic lock (or any other publicly accessible lock). Making the lock object private encapsulates the lock so that client code cannot acquire it, whereas a publicly accessible lock allows client code to participate in its synchronization policy— correctly or incorrectly. Clients that improperly acquire another object’s lock could cause liveness problems, and verifying that a publicly accessible lock is properly used requires examining the entire program rather than a single class.

