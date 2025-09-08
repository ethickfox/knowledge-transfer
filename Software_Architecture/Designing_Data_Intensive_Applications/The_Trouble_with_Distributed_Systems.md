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

