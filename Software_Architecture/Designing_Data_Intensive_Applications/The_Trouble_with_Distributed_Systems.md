The sender can’t even tell whether the packet was delivered: the only option is for the  recipient to send a response message, which may in turn be lost or delayed. These  issues are indistinguishable in an asynchronous network: the only information you  have is that you haven’t received a response yet. If you send a request to another node  and don’t receive a response, it is impossible to tell why. 

The usual way of handling this issue is a timeout: after some time you give up waiting  and assume that the response is not going to arrive. However, when a timeout occurs,  you still don’t know whether the remote node got your request or not

## Detecting Faults 

Many systems need to automatically detect faulty nodes. For example:  
- A load balancer needs to stop sending requests to a node that is dead (i.e., take it  out of rotation).  
- In a distributed database with single-leader replication, if the leader fails, one of  the followers needs to be promoted to be the new leader

TCP performs flow control (also known as congestion avoidance or backpressure),  in which a node limits its own rate of sending in order to avoid overloading a network link or the receiving node [27]. This means additional queueing at the  sender before the data even enters the network. 
Moreover, TCP considers a packet to be lost if it is not acknowledged within some  timeout (which is calculated from observed round-trip times), and lost packets are  automatically retransmitted. Although the application does not see the packet loss  and retransmission, it does see the resulting delay

rather than using configured constant timeouts, systems can continually  measure response times and their variability (jitter), and automatically adjust time‐  outs according to the observed response time distribution. This can be done with a  Phi Accrual failure detector [30], which is used for example in Akka and Cassandra  [31]. TCP retransmission timeouts also work similarly

# Unreliable Clocks 

- A time-of-day clock does what you intuitively expect of a clock: it returns the current  date and time according to some calendar (also known as wall-clock time). For exam‐  ple, clock_gettime(CLOCK_REALTIME) on Linuxv and System.currentTimeMillis()  in Java return the number of seconds (or milliseconds) since the epoch: midnight  UTC on January 1, 1970, according to the Gregorian calendar, not counting leap sec‐  onds. Some systems use other dates as their reference point. 
- A monotonic clock is suitable for measuring a duration (time interval), such as a  timeout or a service’s response time: clock_gettime(CLOCK_MONOTONIC) on Linux  and System.nanoTime() in Java are monotonic clocks, for example. The name comes  from the fact that they are guaranteed to always move forward (whereas a time-ofday clock may jump back in time). the absolute  value of the clock is meaningless: it might be the number of nanoseconds since the  computer was started, or something similarly arbitrary. In particular, it makes no  sense to compare monotonic clock values from two different computers, because they  don’t mean the same thing.

One option is for the leader to obtain a lease from the other nodes, which is similar to  a lock with a timeout [63]. Only one node can hold the lease at any one time—thus,  when a node obtains a lease, it knows that it is the leader for some amount of time,  until the lease expires. In order to remain leader, the node must periodically renew the lease before it expires. If the node fails, it stops renewing the lease, so another  node can take over when it expires. 

``` cpp
while (true) {
  request = getIncomingRequest();  // Ensure that the lease always has at least 10 seconds remaining
 if (lease.expiryTimeMillis - System.currentTimeMillis() < 10000) {
  lease = lease.renew();
 }

 if (lease.isValid()) {
  process(request);
 }
} 
```
What’s wrong with this code?
- it’s relying on synchronized clocks: the expiry  time on the lease is set by a different machine example), and it’s being compared to the local  system clock. If the clocks are out of sync by more than a few seconds, this code will  start doing strange things.
- even if we change the protocol to only use the local monotonic clock, there  is another problem: the code assumes that very little time passes between the point  that it checks the time (System.currentTimeMillis()) and the time when the  request is processed (process(request)). Normally this code runs very quickly, so  the 10 second buffer is more than enough to ensure that the lease doesn’t expire in  the middle of processing a request.  However, what if there is an unexpected pause in the execution of the program? For  example, imagine the thread stops for 15 seconds around the line lease.isValid()  before finally continuing.
- the application performs synchronous disk access, a thread may be paused  waiting for a slow disk I/O operation to complete [68]. In many languages, disk  access can happen surprisingly, even if the code doesn’t explicitly mention file  access—for example, the Java classloader lazily loads class files when they are first  used, which could happen at any time in the program execution. I/O pauses and  GC pauses may even conspire to combine their delays [69]. If the disk is actually  a network filesystem or network block device (such as Amazon’s EBS), the I/O  latency is further subject to the variability of network delays
- node in a distributed system must assume that its execution can be paused for a  significant length of time at any point, even in the middle of a function. During the  pause, the rest of the world keeps moving and may even declare the paused node  dead because it’s not responding. Eventually, the paused node may continue running,  without even noticing that it was asleep until it checks its clock sometime later. 

## Limiting the impact of garbage collection 
An emerging idea is to treat GC pauses like brief planned outages of a node, and to  let other nodes handle requests from clients while one node is collecting its garbage.  If the runtime can warn the application that a node soon requires a GC pause, the  application can stop sending new requests to that node, wait for it to finish process‐  ing outstanding requests, and then perform the GC while no requests are in progress.  This trick hides GC pauses from clients and reduces the high percentiles of response  time [70, 71]. Some latency-sensitive financial trading systems [72] use this approach. 
A variant of this idea is to use the garbage collector only for short-lived objects  (which are fast to collect) and to restart processes periodically, before they accumu‐  late enough long-lived objects to require a full GC of long-lived objects [65, 73]. One  node can be restarted at a time, and traffic can be shifted away from the node before  the planned restart, like in a rolling upgrade

## The Truth Is Defined by the Majority 
A distributed system cannot exclusively rely on a single node, because a  node may fail at any time, potentially leaving the system stuck and unable to recover.  Instead, many distributed algorithms rely on a quorum, that is, voting among the  nodes. decisions require some  minimum number of votes from several nodes in order to reduce the dependence on  any one particular node. Most commonly, the quorum is an absolute majority of more than half the nodes 

### Fencing tokens 
When using a lock or lease to protect access to some resource, we need to ensure that a node that is under a false belief of being “the  chosen one” cannot disrupt the rest of the system. A fairly simple technique that ach‐  ieves this goal is called fencing,
Let’s assume that every time the lock server grants a lock or lease, it also returns a  fencing token, which is a number that increases every time a lock is granted (e.g.,  incremented by the lock service). We can then require that every time a client sends a  write request to the storage service, it must include its current fencing token. 
### Byzantine Faults 
Although we assume that nodes are generally honest, it can be worth adding mecha‐  nisms to software that guard against weak forms of “lying”—for example, invalid  messages due to hardware issues, software bugs, and misconfiguration. Such protec‐  tion mechanisms are not full-blown Byzantine fault tolerance, as they would not  withstand a determined adversary, but they are nevertheless simple and pragmatic  steps toward better reliability.
### System Model and Reality 
We formalize the kinds of faults that we expect to happen in a system by defining a system model, which is an abstraction that  describes what things an algorithm may assume. 
With regard to timing assumptions:
- Synchronous model - you know that network delay, pauses, and clock  drift will never exceed some fixed upper bound. The synchronous model is  not a realistic model of most practical systems
- Partially synchronous model- that a system behaves like a synchronous system most of  the time, but it sometimes exceeds the bounds for network delay, process pauses,  and clock drift. This is a realistic model of many systems: most of the time,  networks and processes are quite well behaved—otherwise we would never be  able to get anything done—but we have to reckon with the fact that any timing  assumptions may be shattered occasionally.
- Asynchronous model - In this model, an algorithm is not allowed to make any timing assumption — in  fact, it does not even have a clock

System models for node failures.
- Crash-stop faults - algorithm may assume that a node can fail in only  one way, namely by crashing.that node is gone forever—it never  comes back.
- Crash-recovery faults - We assume that nodes may crash at any moment, and perhaps start responding  again after some unknown time. time. In the crash-recovery model, nodes are assumed  to have stable storage (i.e., nonvolatile disk storage) that is preserved across  crashes, while the in-memory state is assumed to be lost. 
- Byzantine (arbitrary) faults - Nodes may do absolutely anything, including trying to trick and deceive other  nodes

For modeling real systems, the partially synchronous model with crash-recovery  faults is generally the most useful model.
#### Correctness of an algorithm 
if we are generating fencing tokens  for a lock 303), we may require the algorithm to have  the following properties: 
- Uniqueness
- Monotonic sequence
- Availability - A node that requests a fencing token and does not crash eventually receives a  response.
it is worth distinguishing between two different kinds of  properties: safety and liveness properties. In the example just given, uniqueness and  monotonic sequence are safety properties, but availability is a liveness property. 
A giveaway is that liveness properties  often include the word “eventually” in their definition.
A liveness property works the other way round: it may not hold at some point in  time
After a safety property has been violated, the violation cannot be undone—the  damage is already done. 
Safety is often informally defined as nothing bad happens, and liveness as something  good eventually happens.
## Summary 

