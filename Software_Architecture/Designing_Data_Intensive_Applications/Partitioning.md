For very large datasets, or very high query throughput, replication
not sufficient: we need to break the data up into partitions, also known as sharding.
What we call a partition here is called a shard in MongoDB, Elas‐
ticsearch, and SolrCloud; it’s known as a region in HBase, a tablet
in Bigtable, a vnode in Cassandra and Riak, and a vBucket in
Couchbase. However, partitioning is the most established term, so
we’ll stick with that.

In effect, each partition is a small database of its own, although the database may support operations that touch multiple partitions at the same time.

The main reason for wanting to partition data is scalability. Different partitions can be placed on different nodes in a shared-nothing cluster. Thus, a large dataset can be distributed across many disks, and the query load can be distributed across many processors.

# Partitioning and Replication
Partitioning is usually combined with replication so that copies of each partition are stored on multiple nodes. This means that, even though each record belongs to exactly one partition, it may still be stored on several different nodes for fault tolerance.

A node may store more than one partition. If a leader–follower replication model is used, Each
partition’s leader is assigned to one node, and its followers are assigned to other nodes. Each node may be the leader for some partitions and a follower for other partitions.
# Partitioning of Key-Value Data
 If the partitioning is unfair, so that some partitions have more data or queries than others, we call it skewed. The presence of skew makes partitioning much less effective.
In an extreme case, all the load could end up on one partition, so 9 out of 10 nodes are idle and your bottleneck is the single busy node. A partition with disproportionately high load is called a hot spot.
The simplest approach for avoiding hot spots would be to assign records to nodes randomly. That would distribute the data quite evenly across the nodes, but it has a big disadvantage: when you’re trying to read a particular item, you have no way of knowing which node it is on, so you have to query all nodes in parallel.
We can do better. Let’s assume for now that you have a simple key-value data model, in which you always access a record by its primary key.
### Partitioning by Key Range
One way of partitioning is to assign a continuous range of keys to each partition, like the volumes of a paper encyclopedia. If you know the boundaries between the ranges, you can easily determine which partition contains a given key. If you also know which partition is assigned to which node, then you can make your request directly to the appropriate
node. 
In order to distribute the data evenly, the partition boundaries need to adapt to the data. The partition boundaries might be chosen manually by an administrator, or the database can choose them automatically.
Within each partition, we can keep keys in sorted order. This has the advantage that range scans are easy, and you can treat the key as a concatenated index in order to fetch several related records in one query. Range scans are very useful, because they let you easily fetch, say, all the readings from a particular month.
However, the downside of key range partitioning is that certain access patterns can lead to hot spots. If the key is a timestamp, then the partitions correspond to ranges of time—e.g., one partition per day. Unfortunately, because we write data from the sensors to the database as the measurements happen, all the writes end up going to the same partition (the one for today), so that partition can be overloaded with writes while others sit idle
To avoid this problem in the sensor database, you need to use something other than the timestamp as the first element of the key. For example, you could prefix each timestamp with the sensor name so that the partitioning is first by sensor name and then by time.

# Partitioning by Hash of Key
A good hash function takes skewed data and makes it uniformly distributed. Say you
have a 32-bit hash function that takes a string. Whenever you give it a new string, it
returns a seemingly random number between 0 and 232 − 1. Even if the input strings
are very similar, their hashes are evenly distributed across that range of numbers.

Once you have a suitable hash function for keys, you can assign each partition a
range of hashes (rather than a range of keys), and every key whose hash falls within a
partition’s range will be stored in that partition.

Many programming languages have simple hash functions built in
(as they are used for hash tables), but they may not be suitable for partitioning: for
example, in Java’s Object.hashCode() and Ruby’s Object#hash, the same key may
have a different hash value in different processes

This technique is good at distributing keys fairly among the partitions. The partition
boundaries can be evenly spaced, or they can be chosen pseudorandomly (in which
case the technique is sometimes known as consistent hashing).
Consistent hashing, as defined by Karger et al. [7], is a way of evenly distributing load
across an internet-wide system of caches such as a content delivery network (CDN).
It uses randomly chosen partition boundaries to avoid the need for central control or
distributed consensus. this particular approach
actually doesn’t work very well for databases [8], so it is rarely used in practice (the
documentation of some databases still refers to consistent hashing, but it is often
inaccurate). Because this is so confusing, it’s best to avoid the term consistent hashing
and just call it hash partitioning instead.
Unfortunately however, by using the hash of the key for partitioning we lose a nice
property of key-range partitioning: the ability to do efficient range queries.
Keys that were once adjacent are now scattered across all the partitions, so their sort order is lost. In MongoDB, if you have enabled hash-based sharding mode, any range query has to be sent to all partitions
Cassandra achieves a compromise between the two partitioning strategies [11, 12,
13]. A table in Cassandra can be declared with a compound primary key consisting of
several columns. Only the first part of that key is hashed to determine the partition,
but the other columns are used as a concatenated index for sorting the data in Cas‐
sandra’s SSTables. A query therefore cannot search for a range of values within the first column of a compound key, but if it specifies a fixed value for the first column, it
can perform an efficient range scan over the other columns of the key. 
The concatenated index approach enables an elegant data model for one-to-many
relationships. For example, on a social media site, one user may post many updates. If
the primary key for updates is chosen to be (user_id, update_timestamp), then you
can efficiently retrieve all updates made by a particular user within some time inter‐
val, sorted by timestamp. Different users may be stored on different partitions, but
within each user, the updates are stored ordered by timestamp on a single partition.
# Skewed Workloads and Relieving Hot Spots
As discussed, hashing a key to determine its partition can help reduce hot spots.
However, it can’t avoid them entirely: in the extreme case where all reads and writes
are for the same key, you still end up with all requests being routed to the same parti‐
tion. if one key is known to be very hot, a simple technique is to add a random
number to the beginning or end of the key. Just a two-digit decimal random number
would split the writes to the key evenly across 100 different keys, allowing those keys
to be distributed to different partitions.However, having split the writes across different keys, any reads now have to do additional work, as they have to read the data from all 100 keys and combine it. This technique also requires additional bookkeeping: it only makes sense to append the random number for the small number of hot keys; for the vast majority of keys with low write throughput this would be unnecessary overhead. Thus, you also need some way of keeping track of which keys are being split.
# Partitioning and Secondary Indexes
A secondary index usually doesn’t identify
a record uniquely but rather is a way of searching for occurrences of a particular
value. The problem with secondary indexes is that they don’t map neatly to partitions.
There are two main approaches to partitioning a database with secondary indexes:
document-based partitioning and term-based partitioning.
## Partitioning Secondary Indexes by Document
In this indexing approach, each partition is completely separate: each partition main‐
tains its own secondary indexes, covering only the documents in that partition. It
doesn’t care what data is stored in other partitions. Whenever you need to write to
the database—to add, remove, or update a document—you only need to deal with the
partition that contains the document ID that you are writing. For that reason, a
document-partitioned index is also known as a local index
you need to send the query to all partitions, and combine all the
results you get back.
This approach to querying a partitioned database is sometimes known as scatter/
gather, and it can make read queries on secondary indexes quite expensive. Even if
you query the partitions in parallel, scatter/gather is prone to tail latency amplifica‐
tion
## Partitioning Secondary Indexes by Term
Rather than each partition having its own secondary index (a local index), we can
construct a global index that covers data in all partitions. However, we can’t just store
that index on one node, since it would likely become a bottleneck and defeat the pur‐
pose of partitioning. A global index must also be partitioned, but it can be partitioned
differently from the primary key index.
index is partitioned so that colors starting with
the letters a to r appear in partition 0 and colors starting with s to z appear in parti‐
tion 1. 
We call this kind of index term-partitioned, because the term we’re looking for determines the partition of the index.
The advantage of a global (term-partitioned) index over a document-partitioned
index is that it can make reads more efficient: rather than doing scatter/gather over
all partitions, a client only needs to make a request to the partition containing the
term that it wants. However, the downside of a global index is that writes are slower
and more complicated, because a write to a single document may now affect multiplepartitions of the index (every term in the document might be on a different partition, on a different node).

In practice, updates to global secondary indexes are often asynchronous (that is, if
you read the index shortly after a write, the change you just made may not yet be
reflected in the index)