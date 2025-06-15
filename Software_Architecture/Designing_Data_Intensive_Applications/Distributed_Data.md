# Distributed Data
There are various reasons why you might want to distribute a database across multi‐
ple machines:
## Scalability
If your data volume, read load, or write load grows bigger than a single machine
can handle, you can potentially spread the load across multiple machines.
## Fault tolerance/high availability
If your application needs to continue working even if one machine (or several
machines, or the network, or an entire datacenter) goes down, you can use multi‐
ple machines to give you redundancy. When one fails, another one can take over.
## Latency
If you have users around the world, you might want to have servers at various
locations worldwide so that each user can be served from a datacenter that is geo‐
graphically close to them. That avoids the users having to wait for network pack‐
ets to travel halfway around the world.

## Scaling to Higher Load
If all you need is to scale to higher load, the simplest approach is to buy a more powerful machine (sometimes called vertical scaling or scaling up). The problem with a shared-memory approach is that the cost grows faster than linearly: a machine with twice as many CPUs, twice as much RAM, and twice as much
disk capacity as another typically costs significantly more than twice as much. And
due to bottlenecks, a machine twice the size cannot necessarily handle twice the load.
Shared-Nothing Architectures: 
1. By contrast, shared-nothing architectures (sometimes called horizontal scaling or scaling out) have gained a lot of popularity. In this approach, each machine or virtual machine running the database software is called a node. Each node uses its CPUs,RAM, and disks independently. Any coordination between nodes is done at the software level, using a conventional network.No special hardware is required by a shared-nothing system, so you can use whatever machines have the best price/performance ratio. You can potentially distribute data across multiple geographic regions, and thus reduce latency for users and potentially be able to survive the loss of an entire datacenter.
2. While a distributed shared-nothing architecture has many advantages, it usually also incurs additional complexity for applications and sometimes limits the expressiveness of the data models you can use. In some cases, a simple single-threaded program can perform significantly better than a cluster with over 100 CPU cores. On the other hand, shared-nothing systems can be very powerful.
There are two common ways data is distributed across multiple nodes:
