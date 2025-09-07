The sender can’t even tell whether the packet was delivered: the only option is for the  recipient to send a response message, which may in turn be lost or delayed. These  issues are indistinguishable in an asynchronous network: the only information you  have is that you haven’t received a response yet. If you send a request to another node  and don’t receive a response, it is impossible to tell why. 

The usual way of handling this issue is a timeout: after some time you give up waiting  and assume that the response is not going to arrive. However, when a timeout occurs,  you still don’t know whether the remote node got your request or not

## Detecting Faults 

Many systems need to automatically detect faulty nodes. For example:  
- A load balancer needs to stop sending requests to a node that is dead (i.e., take it  out of rotation).  
- In a distributed database with single-leader replication, if the leader fails, one of  the followers needs to be promoted to be the new leader
