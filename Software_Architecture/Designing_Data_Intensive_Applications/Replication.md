# Replication
Keeping a copy of the same data on several different nodes, potentially in differ‐
ent locations. Replication provides redundancy: if some nodes are unavailable,
the data can still be served from the remaining nodes. Replication can also help
improve performance.
 - To keep data geographically close to your users (and thus reduce latency)
 - To allow the system to continue working even if some of its parts have failed(and thus increase availability)
 - To scale out the number of machines that can serve read queries (and thus increase read throughput)
Every write to the database needs to be processed by every replica; otherwise, the replicas would no longer contain the same data. The most common solution for this is
called leader-based replication (also known as active/passive or master–slave replication)
1. One of the replicas is designated the leader (also known as master or primary).
When clients want to write to the database, they must send their requests to the
leader, which first writes the new data to its local storage.
2. The other replicas are known as followers (read replicas, slaves, secondaries, or hot
standbys).i Whenever the leader writes new data to its local storage, it also sends
the data change to all of its followers as part of a replication log or change stream.
Each follower takes the log from the leader and updates its local copy of the data‐
base accordingly, by applying all writes in the same order as they were processed
on the leader.
3. When a client wants to read from the database, it can query either the leader or
any of the followers. However, writes are only accepted on the leader (the followers are read-only from the client’s point of view).
## Synchronous Versus Asynchronous Replication
the replication to follower 1 is synchronous: the leader
waits until follower 1 has confirmed that it received the write before reporting success
to the user, and before making the write visible to other clients. The replication to
follower 2 is asynchronous: the leader sends the message, but doesn’t wait for a
response from the follower.
The advantage of synchronous replication is that the follower is guaranteed to have
an up-to-date copy of the data that is consistent with the leader. If the leader suddenly fails, we can be sure that the data is still available on the follower. The disadvantage is that if the synchronous follower doesn’t respond (because it has crashed,
or there is a network fault, or for any other reason), the write cannot be processed.
The leader must block all writes and wait until the synchronous replica is available
again.
In practice, if you enable synchronous replication on a database, it usually means that one of the followers is synchronous, and the others are asynchronous. If the synchronous follower becomes unavailable or slow, one of the asynchronous followers is made synchronous. This guarantees that you have an up-to-date copy of the data on at least two nodes:
- This configuration is sometimes also called semi-synchronous 
  Often, leader-based replication is configured to be completely asynchronous. In this case, if the leader fails and is not recoverable, any writes that have not yet been replicated to followers are lost. This means that a write is not guaranteed to be durable, even if it has been confirmed to the client. However, a fully asynchronous configuration has the advantage that the leader can continue processing writes, even if all of its followers have fallen behind.
- How do you ensure that the new follower has an accurate copy of the leader’s data? Simply copying data files from one node to another is typically not sufficient: clients are constantly writing to the database, and the data is always in flux, so a standard file copy would see different parts of the database at different points in time. The result might not make any sense. setting up a follower can usually be done without downtime. Conceptually, the process looks like this:
	1. Take a consistent snapshot of the leader’s database at some point in time—if possible, without taking a lock on the entire database. Most databases have this feature, as it is also required for backups. In some cases, third-party tools are needed, such as innobackupex for MySQL.
	2. Copy the snapshot to the new follower node.
	3. The follower connects to the leader and requests all the data changes that have happened since the snapshot was taken. This requires that the snapshot is associated with an exact position in the leader’s replication log. That position has various names: for example, PostgreSQL calls it the log sequence number, and MySQL calls it the binlog coordinates.
	4. When the follower has processed the backlog of data changes since the snapshot, we say it has caught up. It can now continue to process data changes from the leader as they happen.
# Handling Node Outages
## Follower failure: Catch-up recovery
On its local disk, each follower keeps a log of the data changes it has received from
the leader. If a follower crashes and is restarted, or if the network between the leader
and the follower is temporarily interrupted, the follower can recover quite easily:
from its log, it knows the last transaction that was processed before the fault occur‐
red. Thus, the follower can connect to the leader and request all the data changes that
occurred during the time when the follower was disconnected. When it has applied
these changes, it has caught up to the leader and can continue receiving a stream of
data changes as before.
## Leader failure: Failover
Handling a failure of the leader is trickier: one of the followers needs to be promoted
to be the new leader, clients need to be reconfigured to send their writes to the new
leader, and the other followers need to start consuming data changes from the new
leader. This process is called failover.
Failover can happen manually (an administrator is notified that the leader has failed
and takes the necessary steps to make a new leader) or automatically. An automatic
failover process usually consists of the following steps:
- Determining that the leader has failed. There are many things that could potentially go wrong: crashes, power outages, network issues, and more. There is no foolproof way of detecting what has gone wrong, so most systems simply use a timeout: nodes frequently bounce messages back and forth between each other, and if a node doesn’t respond for some period of time—say, 30 seconds—it is assumed to be dead.
- Choosing a new leader. The best candidate for leadership is usually the replica with the most up-to-date data changes from the old leader (to minimize any data loss). Getting all the nodes to agree on a new leader is a consensus problem,
- Reconfiguring the system to use the new leader. Clients now need to send their write requests to the new leader. If the old leader comes back, it might still believe that it is the leader, not realizing that the other replicas have forced it to step down. The system needs to ensure that the old leader becomes a follower and recognizes the new leader.

Statement-based replication
In the simplest case, the leader logs every write request (statement) that it executes
and sends that statement log to its followers. For a relational database, this means
that every INSERT, UPDATE, or DELETE statement is forwarded to followers, and each
follower parses and executes that SQL statement as if it had been received from a
client.
Any statement that calls a nondeterministic function, such as NOW() to get the
current date and time or RAND() to get a random number, is likely to generate a
different value on each replica.
• If statements use an autoincrementing column, or if they depend on the existing
data in the database (e.g., `UPDATE … WHERE <some condition>`), they must be
executed in exactly the same order on each replica, or else they may have a differ‐
ent effect. This can be limiting when there are multiple concurrently executing
transactions.
• Statements that have side effects (e.g., triggers, stored procedures, user-defined
functions) may result in different side effects occurring on each replica, unless
the side effects are absolutely deterministic.
It is possible to work around those issues—for example, the leader can replace any
nondeterministic function calls with a fixed return value when the statement is log‐
ged so that the followers all get the same value. However, because there are so many
edge cases, other replication methods are now generally preferred.
Statement-based replication was used in MySQL before version 5.1. It is still some‐
times used today, as it is quite compact, but by default MySQL now switches to row-
based replication (discussed shortly) if there is any nondeterminism in a statement.

Write-ahead log (WAL) shipping
the log is an append-only sequence of bytes containing all writes to the
database. We can use the exact same log to build a replica on another node: besides
writing the log to disk, the leader also sends it across the network to its followers.
This method of replication is used in PostgreSQL and Oracle, among others [16]. The
main disadvantage is that the log describes the data on a very low level: a WAL con‐
tains details of which bytes were changed in which disk blocks. This makes replica‐
tion closely coupled to the storage engine. If the database changes its storage format
from one version to another, it is typically not possible to run different versions of
the database software on the leader and the followers.
That may seem like a minor implementation detail, but it can have a big operational
impact. If the replication protocol allows the follower to use a newer software version
than the leader, you can perform a zero-downtime upgrade of the database software
by first upgrading the followers and then performing a failover to make one of the
upgraded nodes the new leader. If the replication protocol does not allow this version
mismatch, as is often the case with WAL shipping, such upgrades require downtime.

Logical (row-based) log replication
An alternative is to use different log formats for replication and for the storage
engine, which allows the replication log to be decoupled from the storage engine
internals. This kind of replication log is called a logical log, to distinguish it from the
storage engine’s (physical) data representation.

A logical log for a relational database is usually a sequence of records describing
writes to database tables at the granularity of a row:
• For an inserted row, the log contains the new values of all columns.
• For a deleted row, the log contains enough information to uniquely identify the
row that was deleted. Typically this would be the primary key, but if there is no
primary key on the table, the old values of all columns need to be logged.
• For an updated row, the log contains enough information to uniquely identify
the updated row, and the new values of all columns (or at least the new values of
all columns that changed).

Trigger-based replication
if you want to only replicate a subset of the data, or want to replicate from one kind of
database to another, or if you need conflict resolution logic, then you may need to move replication up to the application layer.
Some tools, such as Oracle GoldenGate [19], can make data changes available to an
application by reading the database log. An alternative is to use features that are
available in many relational databases: triggers and stored procedures.

# Partitioning
Splitting a big database into smaller subsets called partitions so that different partitions can be assigned to different nodes (also known as sharding)

## Problems with Replication Lag
In this read-scaling architecture, you can increase the capacity for serving read-only
requests simply by adding more followers. However, this approach only realistically
works with asynchronous replication—if you tried to synchronously replicate to all
followers, a single node failure or network outage would make the entire system unavailable for writing. And the more nodes you have, the likelier it is that one will
be down, so a fully synchronous configuration would be very unreliable.

if an application reads from an asynchronous follower, it may see out‐
dated information if the follower has fallen behind. This leads to apparent inconsis‐
tencies in the database: if you run the same query on the leader and a follower at the
same time, you may get different results, because not all writes have been reflected in
the follower. This inconsistency is just a temporary state—if you stop writing to the
database and wait a while, the followers will eventually catch up and become consis‐
tent with the leader. For that reason, this effect is known as eventual consistency

In normal operation, the delay between a write happening on
the leader and being reflected on a follower—the replication lag—may be only a frac‐
tion of a second, and not noticeable in practice.
## Reading Your Own Writes
When new data is submitted, it must be sent to
the leader, but when the user views the data, it can be read from a follower. With asynchronous replication, there is a problem, illustrated in Figure 5-3: if the
user views the data shortly after making a write, the new data may not yet have
reached the replica.
In this situation, we need read-after-write consistency, also known as read-your-writes
consistency. This is a guarantee that if the user reloads the page, they will always
see any updates they submitted themselves. It makes no promises about other users:
other users’ updates may not be visible until some later time.
Implementation:
- When reading something that the user may have modified, read it from the leader; otherwise, read it from a follower.
- If most things in the application are potentially editable by the user, that approach won’t be effective, as most things would have to be read from the leader (negating the benefit of read scaling). In that case, other criteria may be used to decide whether to read from the leader. For example, you could track the time of the last update and, for one minute after the last update, make all reads from the leader. You could also monitor the replication lag on followers and prevent queries on any follower that is more than one minute behind the leader.
- The client can remember the timestamp of its most recent write—then the system can ensure that the replica serving any reads for that user reflects updates at least until that timestamp.
- If your replicas are distributed across multiple data centers (for geographical proximity to users or for availability), there is additional complexity. Any request that needs to be served by the leader must be routed to the datacenter that contains the leader. Another complication arises when the same user is accessing your service from multiple devices, for example a desktop web browser and a mobile app. In this case you may want to provide cross-device read-after-write consistency: if the user enters some information on one device and then views it on another device, they should see the information they just entered.
- Approaches that require remembering the timestamp of the user’s last update become more difficult, because the code running on one device doesn’t know what updates have happened on the other device. This metadata will need to be centralized.
- If your replicas are distributed across different data centers, there is no guarantee that connections from different devices will be routed to the same datacenter. If your approach requires reading from the leader, you may first need to route requests from all of a user’s devices to the same datacenter.
## Monotonic Reads
anomaly that can occur when reading from asynchronous
followers is that it’s possible for a user to see things moving backward in time.
This can happen if a user makes several reads from different replicas.
Monotonic reads is a guarantee that this kind of anomaly does not happen. It’s a
lesser guarantee than strong consistency, but a stronger guarantee than eventual con‐
sistency. When you read data, you may see an old value; monotonic reads only means
that if one user makes several reads in sequence, they will not see time go backward—
i.e., they will not read older data after having previously read newer data.
One way of achieving monotonic reads is to make sure that each user always makes
their reads from the same replica
## Consistent Prefix Reads
Preventing this kind of anomaly requires another type of guarantee: consistent prefix reads. This guarantee says that if a sequence of writes happens in a certain order, then anyone reading those writes will see them appear in the same order.
If the database always applies writes in the same order, reads always see
a consistent prefix, so this anomaly cannot happen. However, in many distributed databases, different partitions operate independently, so there is no global ordering of writes: when a user reads from the database, they may see some parts of the database in an older state and some in a newer state.
One solution is to make sure that any writes that are causally related to each other are written to the same partition—but in some applications that cannot be done efficiently. There are also algorithms that explicitly keep track of causal dependencies,
