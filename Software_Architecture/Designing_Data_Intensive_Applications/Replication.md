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

# Multi-Leader Replication
We call this a multi-leader configuration (also known as master–master or active/active replication) In this setup, each leader simultaneously acts as a follower to the other leaders.
It rarely makes sense to use a multi-leader setup within a single datacenter, because the benefits rarely outweigh the added complexity. However, there are some situtions in which this configuration is reasonable.
## Multi-datacenter operation
In a multi-leader configuration, you can have a leader in each datacenter. Within each datacenter, regular leader–
follower replication is used; between datacenters, each datacenter’s leader replicates
its changes to the leaders in other datacenters.
![](_img/Pasted%20image%2020250619102907.png)
Let’s compare how the single-leader and multi-leader configurations fare in a multi-
datacenter deployment:
**Performance**
In a single-leader configuration, every write must go over the internet to the
datacenter with the leader.
In a multi-leader configuration, every write can be processed in the local datacenter
and is replicated asynchronously to the other datacenters.
**Tolerance of datacenter outages**
In a single-leader configuration, if the datacenter with the leader fails, failover
can promote a follower in another datacenter to be leader. In a multi-leader con‐
figuration, each datacenter can continue operating independently of the others,
and replication catches up when the failed datacenter comes back online.
**Tolerance of network problems**
Traffic between datacenters usually goes over the public internet, which may be
less reliable than the local network within a datacenter. A single-leader configu‐
ration is very sensitive to problems in this inter-datacenter link, because writes
are made synchronously over this link. A multi-leader configuration with asyn‐
chronous replication can usually tolerate network problems better: a temporary
network interruption does not prevent writes being processed.

Although multi-leader replication has advantages, it also has a big downside: the
same data may be concurrently modified in two different datacenters, and those write conflicts must be resolved
**Clients with offline operation**
Another situation in which multi-leader replication is appropriate is if you have an
application that needs to continue to work while it is disconnected from the internet.
In this case, every device has a local database that acts as a leader (it accepts write
requests), and there is an asynchronous multi-leader replication process (sync)
between the replicas of your calendar on all of your devices. The replication lag may
be hours or even days, depending on when you have internet access available.
From an architectural point of view, this setup is essentially the same as multi-leader replication between datacenters, taken to the extreme: each device is a “datacenter,” and the network connection between them is extremely unreliable.

# Handling Write Conflicts
in a multi-leader setup, both writes are successful, and the conflict is only detected asynchronously at some later point in time. At that time, it may be too late to ask the user to resolve the conflict.
![](_img/Pasted%20image%2020250623184856.png)
You could make the conflict detection synchronous—i.e., wait for the
write to be replicated to all replicas before telling the user that the write was success‐
ful. However, by doing so, you would lose the main advantage of multi-leader repli‐
cation: allowing each replica to accept writes independently. If you want synchronous
conflict detection, you might as well just use single-leader replication.

The simplest strategy for dealing with conflicts is to avoid them: if the application can
ensure that all writes for a particular record go through the same leader, then con‐
flicts cannot occur. Since many implementations of multi-leader replication handle
conflicts quite poorly, avoiding conflicts is a frequently recommended approach
For example, in an application where a user can edit their own data, you can ensure
that requests from a particular user are always routed to the same datacenter and use
the leader in that datacenter for reading and writing. Different users may have differ‐
ent “home” datacenters
## Converging toward a consistent state
If each replica simply applied writes in the order that it saw the writes, the database
would end up in an inconsistent state: the final value would be C at leader 1 and B at
leader 2. That is not acceptable—every replication scheme must ensure that the data
is eventually the same in all replicas. Thus, the database must resolve the conflict in a convergent way, which means that all replicas must arrive at the same final value
when all changes have been replicated.
There are various ways of achieving convergent conflict resolution:
- Give each write a unique ID (e.g., a timestamp, a long random number, a UUID, or a hash of the key and value), pick the write with the highest ID as the winner, and throw away the other writes. Although this approach is popular, it is dangerously prone to data loss
- Give each replica a unique ID, and let writes that originated at a higher-numbered replica always take precedence over writes that originated at a lower-numbered replica. This approach also implies data loss.
- Somehow merge the values together—e.g., order them alphabetically and then concatenate them
- Record the conflict in an explicit data structure that preserves all information, and write application code that resolves the conflict at some later time
Note that conflict resolution usually applies at the level of an individual row or docu‐
ment, not for an entire transaction. Thus, if you have a transaction that atomically makes several different writes (see Chapter 7), each write is still considered
separately for the purposes of conflict resolution.

- Conflict-free replicated datatypes (CRDTs) are a family of data structures
for sets, maps, ordered lists, counters, etc. that can be concurrently edited by
multiple users, and which automatically resolve conflicts in sensible ways.
- Mergeable persistent data structures track history explicitly, similarly to the
Git version control system, and use a three-way merge function (whereas CRDTs
use two-way merges).
- Operational transformation is the conflict resolution algorithm behind col‐
laborative editing applications such as Etherpad and Google Docs. It
was designed particularly for concurrent editing of an ordered list of items, such
as the list of characters that constitute a text document.

# Multi-Leader Replication Topologies
A **replication topology** describes the communication paths along which writes are
propagated from one node to another.
A replication topology describes the communication paths along which writes are
propagated from one node to another:
![](_img/Pasted%20image%2020250623190935.png)
The most general topology is all-to-all (Figure 5-8 [c]), in which every leader sends its
writes to every other leader.

- circular topology [34], in which each
node receives writes from one node and forwards those writes (plus any writes of its
own) to one other node.
- Another popular topology has the shape of a star:v one desig‐
nated root node forwards writes to all of the other nodes. The star topology can be
generalized to a tree.

In circular and star topologies, a write may need to pass through several nodes before
it reaches all replicas. Therefore, nodes need to forward data changes they receive
from other nodes. To prevent infinite replication loops, each node is given a unique
identifier, and in the replication log, each write is tagged with the identifiers of all the
nodes it has passed through. When a node receives a data change that is tagged with its own identifier, that data change is ignored, because the node knows that it
has already been processed.
A problem with circular and star topologies is that if just one node fails, it can inter‐
rupt the flow of replication messages between other nodes,
The fault tolerance of a more densely connected topology (such as
all-to-all) is better because it allows messages to travel along different paths, avoiding
a single point of failure.
On the other hand, all-to-all topologies can have issues too. In particular, some net‐
work links may be faster than others (e.g., due to network congestion), with the result
that some replication messages may “overtake”

In Figure 5-9, client A inserts a row into a table on leader 1, and client B updates that
row on leader 3. However, leader 2 may receive the writes in a different order: it may
first receive the update (which, from its point of view, is an update to a row that does
not exist in the database) and only later receive the corresponding insert (which
should have preceded the update).
This is a problem of causality, similar to the one we saw in “Consistent Prefix Reads” the update depends on the prior insert, so we need to make sure that all
nodes process the insert first, and then the update. 
To order these events correctly, a technique called version vectors can be used
If you are using a system with multi-leader replication, it is worth being aware of
these issues, carefully reading the documentation, and thoroughly testing your data‐
base to ensure that it really does provide the guarantees you believe it to have.

# Leaderless Replication
Some data storage systems take a different approach, abandoning the concept of a
leader and allowing any replica to directly accept writes from clients. Some of the ear‐
liest replicated data systems were leaderless [1, 44], but the idea was mostly forgotten
during the era of dominance of relational databases. It once again became a fashiona‐
ble architecture for databases after Amazon used it for its in-house Dynamo system
[37].vi Riak, Cassandra, and Voldemort are open source datastores with leaderless
replication models inspired by Dynamo, so this kind of database is also known as
Dynamo-style.
## Writing to the Database When a Node Is Down
Imagine you have a database with three replicas, and one of the replicas is currently
unavailable—perhaps it is being rebooted to install a system update. In a leader-based configuration, if you want to continue processing writes, you may need to perform a
failover. On the other hand, in a leaderless configuration, failover does not exist.
Now imagine that the unavailable node comes back online, and clients start reading
from it. Any writes that happened while the node was down are missing from that
node. Thus, if you read from that node, you may get stale (outdated) values as
responses.
To solve that problem, when a client reads from the database, it doesn’t just send its
request to one replica: read requests are also sent to several nodes in parallel. The cli‐
ent may get different responses from different nodes; i.e., the up-to-date value from
one node and a stale value from another. Version numbers are used to determine
which value is newer
The replication scheme should ensure that eventually all the data is copied to every
replica. After an unavailable node comes back online, how does it catch up on the
writes that it missed?
**Read repair**
When a client makes a read from several nodes in parallel, it can detect any stale
responses. For example, in Figure 5-10, user 2345 gets a version 6 value from rep‐
lica 3 and a version 7 value from replicas 1 and 2. The client sees that replica 3
has a stale value and writes the newer value back to that replica. This approach
works well for values that are frequently read.
**Anti-entropy process**
In addition, some datastores have a background process that constantly looks for
differences in the data between replicas and copies any missing data from one
replica to another. Unlike the replication log in leader-based replication, this
anti-entropy process does not copy writes in any particular order, and there may
be a significant delay before data is copied.
### Quorums for reading and writing
If we know that every successful write is guaranteed to be present on at least two out
of three replicas, that means at most one replica can be stale. Thus, if we read from at
least two replicas, we can be sure that at least one of the two is up to date. If the third
replica is down or slow to respond, reads can nevertheless continue returning an up-
to-date value.
More generally, if there are n replicas, every write must be confirmed by w nodes to
be considered successful, and we must query at least r nodes for each read.
The quorum condition, w + r > n, allows the system to tolerate unavailable nodes as
follows:
• If w < n, we can still process writes if a node is unavailable.
• If r < n, we can still process reads if a node is unavailable.
• With n = 3, w = 2, r = 2 we can tolerate one unavailable node.
• With n = 5, w = 3, r = 3 we can tolerate two unavailable nodes. This case is illus‐
trated in Figure 5-11.
• Normally, reads and writes are always sent to all n replicas in parallel. The
parameters w and r determine how many nodes we wait for—i.e., how many of
the n nodes need to report success before we consider the read or write to be suc‐
cessful.
## Limitations of Quorum Consistency
Often, r and w are chosen to be a majority (more than n/2) of nodes, because that
ensures w + r > n while still tolerating up to n/2 node failures. But quorums are not
necessarily majorities—it only matters that the sets of nodes used by the read and
write operations overlap in at least one node. Other quorum assignments are possi‐
ble, which allows some flexibility in the design of distributed algorithms
However, even with w + r > n, there are likely to be edge cases where stale values are returned. These depend on the implementation, but possible scenarios include:
- If a sloppy quorum is used, the w writes may end up on different nodes than the r reads, so there is no longer a guaranteed overlap between the r nodes and the w nodes
- If two writes occur concurrently, it is not clear which one happened first. In this case, the only safe solution is to merge the concurrent writes
- If a write happens concurrently with a read, the write may be reflected on only some of the replicas. In this case, it’s undetermined whether the read returns the old or the new value.
- If a write succeeded on some replicas but failed on others (for example because the disks on some nodes are full), and overall succeeded on fewer than w replicas, it is not rolled back on the replicas where it succeeded. This means that if a write was reported as failed, subsequent reads may or may not return the value from that write
- If a node carrying a new value fails, and its data is restored from a replica carrying an old value, the number of replicas storing the new value may fall below w, breaking the quorum condition.
### Monitoring staleness
For leader-based replication, the database typically exposes metrics for the replication
lag, which you can feed into a monitoring system. This is possible because writes are
applied to the leader and to followers in the same order, and each node has a position
in the replication log (the number of writes it has applied locally). By subtracting a
follower’s current position from the leader’s current position, you can measure the
amount of replication lag. However, in systems with leaderless replication, there is no fixed order in which
writes are applied, which makes monitoring more difficult.
## Sloppy Quorums and Hinted Handoff
In a large cluster (with significantly more than n nodes) it’s likely that the client can
connect to some database nodes during the network interruption, just not to the
nodes that it needs to assemble a quorum for a particular value. In that case, database
designers face a trade-off:
• Is it better to return errors to all requests for which we cannot reach a quorum of
w or r nodes?
• Or should we accept writes anyway, and write them to some nodes that are
reachable but aren’t among the n nodes on which the value usually lives?
The latter is known as a sloppy quorum [37]: writes and reads still require w and r
successful responses, but those may include nodes that are not among the designated n “home” nodes for a value.
Once the network interruption is fixed, any writes that one node temporarily
accepted on behalf of another node are sent to the appropriate “home” nodes. This called hinted handoff.
Sloppy quorums are particularly useful for increasing write availability: as long as any
w nodes are available, the database can accept writes. However, this means that even
when w + r > n, you cannot be sure to read the latest value for a key, because the
latest value may have been temporarily written to some nodes outside of n
### Multi-datacenter operation
Each write from a client is sent to all replicas, regardless of datacen‐
ter, but the client usually only waits for acknowledgment from a quorum of nodes
within its local datacenter so that it is unaffected by delays and interruptions on the
cross-datacenter link. The higher-latency writes to other datacenters are often configured to happen asynchronously, although there is some flexibility in the configuration
# Detecting Concurrent Writes
Dynamo-style databases allow several clients to concurrently write to the same key,
which means that conflicts will occur even if strict quorums are used. The situation is
similar to multi-leader replication (see “Handling Write Conflicts” on page 171),
although in Dynamo-style databases conflicts can also arise during read repair or
hinted handoff.
The problem is that events may arrive in a different order at different nodes, due to
variable network delays and partial failures. For example, two cli‐
ents, A and B, simultaneously writing to a key X in a three-node datastore:
- Node 1 receives the write from A, but never receives the write from B due to a transient outage.
- Node 2 first receives the write from A, then the write from B.
- Node 3 first receives the write from B, then the write from A.
If each node simply overwrote the value for a key whenever it received a write request
from a client, the nodes would become permanently inconsistent,
In order to become eventually consistent, the replicas should converge toward the
same value.

Last write wins (discarding concurrent writes)
One approach for achieving eventual convergence is to declare that each replica need
only store the most “recent” value and allow “older” values to be overwritten and dis‐
carded.
Then, as long as we have some way of unambiguously determining which
write is more “recent,” and every write is eventually copied to every replica, the repli‐
cas will eventually converge to the same value.
Even though the writes don’t have a natural ordering, we can force an arbitrary order
on them. For example, we can attach a timestamp to each write, pick the biggest
timestamp as the most “recent,” and discard any writes with an earlier timestamp.
This conflict resolution algorithm, called last write wins (LWW), is the only sup‐
ported conflict resolution method in Cassandra
LWW achieves the goal of eventual convergence, but at the cost of durability: if there
are several concurrent writes to the same key, even if they were all reported as suc‐
cessful to the client (because they were written to w replicas), only one of the writes
will survive and the others will be silently discarded.
There are some situations, such as caching, in which lost writes are perhaps accepta‐
ble. If losing data is not acceptable, LWW is a poor choice for conflict resolution.
The only safe way of using a database with LWW is to ensure that a key is only writ‐
ten once and thereafter treated as immutable, thus avoiding any concurrent updates
to the same key. For example, a recommended way of using Cassandra is to use a
UUID as the key, thus giving each write operation a unique key

## The “happens-before” relationship and concurrency
An operation A happens before another operation B if B knows about A, or depends
on A, or builds upon A in some way. Whether one operation happens before another
operation is the key to defining what concurrency means. In fact, we can simply say
that two operations are concurrent if neither happens before the other (i.e., neither
knows about the other)
Capturing the happens-before relationship
Note that the server can determine whether two operations are concurrent by looking
at the version numbers—it does not need to interpret the value itself (so the value
could be any data structure)
The server maintains a version number for every key, increments the version
number every time that key is written, and stores the new version number along
with the value written.
• When a client reads a key, the server returns all values that have not been over‐
written, as well as the latest version number. A client must read a key before
writing.
• When a client writes a key, it must include the version number from the prior
read, and it must merge together all values that it received in the prior read. (The
response from a write request can be like a read, returning all current values,
which allows us to chain several writes like in the shopping cart example.)
• When the server receives a write with a particular version number, it can over‐
write all values with that version number or below (since it knows that they have
been merged into the new value), but it must keep all values with a higher ver‐
sion number (because those values are concurrent with the incoming write).

Merging concurrently written values
With the example of a shopping cart, a reasonable approach to merging siblings is to
just take the union. In Figure 5-14, the two final siblings are [milk, flour, eggs,
bacon] and [eggs, milk, ham]; note that milk and eggs appear in both, even
though they were each only written once. The merged value might be something like
[milk, flour, eggs, bacon, ham], without duplicates.
However, if you want to allow people to also remove things from their carts, and not
just add things, then taking the union of siblings may not yield the right result: if you
merge two sibling carts and an item has been removed in only one of them, then the
removed item will reappear in the union of the siblings [37]
an item cannot simply be deleted from the database when it is removed; instead,
the system must leave a marker with an appropriate version number to indicate that
the item has been removed when merging siblings. Such a deletion marker is known
as a tombstone.
Version vectors
single version number to capture dependencies between opera‐
tions, but that is not sufficient when there are multiple replicas accepting writes con‐
currently. Instead, we need to use a version number per replica as well as per key.
Each replica increments its own version number when processing a write, and also
keeps track of the version numbers it has seen from each of the other replicas. This
information indicates which values to overwrite and which values to keep as siblings.
The collection of version numbers from all the replicas is called a version vector [56].
A few variants of this idea are in use, but the most interesting is probably the dotted
version vector
## Summary
In this chapter we looked at the issue of replication. Replication can serve several purposes:

### High availability
Keeping the system running, even when one machine (or several machines, or an entire datacenter) goes down
### Disconnected operation
Allowing an application to continue working when there is a network interrup
tion
### Latency
Placing data geographically close to users, so that users can interact with it faster
### Scalability
Being able to handle a higher volume of reads than a single machine could handle, by performing reads on replicas
### Single-leader replication
Clients send all writes to a single node (the leader), which sends a stream of data change events to the other replicas (followers). Reads can be performed on any replica, but reads from followers might be stale.

Single-leader replication is popular because it is fairly easy to understand and there is no conflict resolution to worry about
### Multi-leader replication
Clients send each write to one of several leader nodes, any of which can accept writes. The leaders send streams of data change events to each other and to any follower nodes.
### Leaderless replication
Clients send each write to several nodes, and read from several nodes in parallel in order to detect and correct nodes with stale data

Multi-leader replication and Leaderless replication could be more robust in the presence of faulty nodes, network interruptions and latency spikes - at cost of very weak consistency guaranties 

Replication can be synchronous or asynchronous, which has a profound effect on the system behavior when there is a fault. Although asynchronous replication can be fast when the system is running smoothly, it’s important to figure out what happens when replication lag increases and servers fail. If a leader fails and you promote an asynchronously updated follower to be the new leader, recently committed data may be lost.

 Few consistency models which are helpful for deciding how an application should behave under replication lag:

### Read-after-write consistency
Users should always see data that they submitted themselves.
### Monotonic reads
After users have seen the data at one point in time, they shouldn’t later see the data from some earlier point in time.
### Consistent prefix reads
Users should see the data in a state that makes causal sense: for example, seeing a question and it's reply in correct order 