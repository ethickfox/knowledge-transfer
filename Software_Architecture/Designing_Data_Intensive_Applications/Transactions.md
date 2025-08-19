A transaction is a way for an application to group several reads and writes together into a logical unit. Conceptually, all the reads and writes in a transaction are executed as one operation: either the entire transaction succeeds (commit) or it fails(abort, rollback). If it fails, the application can safely retry. With transactions, error handling becomes much simpler for an application, because it doesn’t need to a bout partial failure accessing a database. By using transactions, the application is free to ignore certain potential error scenarios and concurrency issues, because the database takes care of them instead (we call these safety guarantees)
# ACID
in practice, one database’s implementation of ACID does not equal
another’s implementation. For example, as we shall see, there is a lot of ambiguity
around the meaning of isolation [8]. The high-level idea is sound, but the devil is in
the details. Today, when a system claims to be “ACID compliant,” it’s unclear what
guarantees you can actually expect. ACID has unfortunately become mostly a mar‐
keting term.
# Atomicity
In general, atomic refers to something that cannot be broken down into smaller parts.
atomicity describes what happens if a client wants to make several
writes, but a fault occurs after some of the writes have been processed. the writes are grouped together into an atomic
transaction, and the transaction cannot be completed (committed) due to a fault, then
the transaction is aborted and the database must discard or undo any writes it has
made so far in that transaction.
# Consistency
If a transaction starts with a
database that is valid according to these invariants, and any writes during the transac‐
tion preserve the validity, then you can be sure that the invariants are always satisfied.
# Isolation
Isolation in the sense of ACID means that concurrently executing transactions are
isolated from each other: they cannot step on each other’s toes. The classic database
textbooks formalize isolation as serializability, which means that each transaction can
pretend that it is the only transaction running on the entire database. The database
ensures that when the transactions have committed, the result is the same as if they
had run serially (one after another), even though in reality they may have run con‐
currently
However, in practice, serializable isolation is rarely used, because it carries a perfor‐
mance penalty. Some popular databases, such as Oracle 11g, don’t even implement it.
In Oracle there is an isolation level called “serializable,” but it actually implements
something called snapshot isolation, which is a weaker guarantee than serializability
# Durability
Durability is the promise that once a transaction has com‐
mitted successfully, any data it has written will not be forgotten, even if there is a
hardware fault or the database crashes.
In a replicated database, durabil‐
ity may mean that the data has been successfully copied to some number of nodes. In
order to provide a durability guarantee, a database must wait until these writes or
replications are complete before reporting a transaction as successfully committed.
# The need for multi-object transactions
Many distributed datastores have abandoned multi-object transactions because they
are difficult to implement across partitions, and they can get in the way in some sce‐
narios where very high availability or performance is required. However, there is
nothing that fundamentally prevents transactions in a distributed database, and we
will discuss implementations of distributed transactions
- If the transaction actually succeeded, but the network failed while the server tried
to acknowledge the successful commit to the client (so the client thinks it failed),
then retrying the transaction causes it to be performed twice—unless you have an
additional application-level deduplication mechanism in place.
- If the error is due to overload, retrying the transaction will make the problem
worse, not better. To avoid such feedback cycles, you can limit the number of
retries, use exponential backoff, and handle overload-related errors differently
from other errors
- It is only worth retrying after transient errors (for example due to deadlock, iso‐
lation violation, temporary network interruptions, and failover); after a perma‐
nent error (e.g., constraint violation) a retry would be pointless.
- If the transaction also has side effects outside of the database, those side effects
may happen even if the transaction is aborted. If you want to make sure that several different systems either commit or abort together, two-phase commit can help

# Read Committed 
The most basic level of transaction isolation is read committed. v It makes two guaran‐  tees:  
1. When reading from the database, you will only see data that has been committed  (no dirty reads).  
2. When writing to the database, you will only overwrite data that has been com‐  mitted (no dirty writes). 
## No dirty reads 
Imagine a transaction has written some data to the database, but the transaction has  not yet committed or aborted. Can another transaction see that uncommitted data? If  yes, that is called a dirty read

Transactions running at the read committed isolation level must prevent dirty reads. 

If a transaction needs to update several objects, a dirty read means that another  transaction may see some of the updates but not others.

## No dirty writes 
What happens if two transactions concurrently try to update the same object in a  database? We don’t know in which order the writes will happen, but we normally  assume that the later write overwrites the earlier write.  However, what happens if the earlier write is part of a transaction that has not yet  committed, so the later write overwrites an uncommitted value? This is called a dirty  write [28]. Transactions running at the read committed isolation level must prevent  dirty writes, usually by delaying the second write until the first write’s transaction has  committed or aborted. 

Read committed does not prevent the race condition between two  counter increments. In this case, the second write happens after the  first transaction has committed, so it’s not a dirty write. It’s still incorrect, but for  a different reason

Read committed is a very popular isolation level. It is the default setting in Oracle  11g, PostgreSQL, SQL Server 2012, MemSQL, and many other databases

Most commonly, databases prevent dirty writes by using row-level locks: when a  transaction wants to modify a particular object (row or document), it must first  acquire a lock on that object. It must then hold that lock until the transaction is com‐  mitted or aborted. Only one transaction can hold the lock for any given object; if  another transaction wants to write to the same object, it must wait until the first  transaction is committed or aborted before it can acquire the lock and continue.

Read skew is considered acceptable  under read committed isolation: the

**Snapshot isolation** is the most common solution to this problem. The idea is that  each transaction reads from a consistent snapshot of the database—that is, the trans‐  action sees all the data that was committed in the database at the start of the transac‐  tion. Even if the data is subsequently changed by another transaction, each  transaction sees only the old data from that particular point in time. 

Snapshot isolation is a boon for long-running, read-only queries such as backups and  analytics. It is very hard to reason about the meaning of a query if the data on which it operates is changing at the same time as the query is executing.

Like read committed isolation, implementations of snapshot isolation typically use  write locks to prevent dirty writes

which means that a transaction that makes a write can block the progress of another  transaction that writes to the same object. However, reads do not require any locks.  From a performance point of view, a key principle of snapshot isolation is readers  never block writers, and writers never block readers.

The database must potentially keep  several different committed versions of an object, because various in-progress trans‐  actions may need to see the state of the database at different points in time. Because it  maintains several versions of an object side by side, this technique is known as multiversion concurrency control (MVCC). 

If a database only needed to provide read committed isolation, but not snapshot iso‐  lation, it would be sufficient to keep two versions of an object: the committed version  and the overwritten-but-not-yet-committed version. However, storage engines that  support snapshot isolation typically use MVCC for their read committed isolation  level as well. A typical approach is that read committed uses a separate snapshot for  each query, while snapshot isolation uses the same snapshot for an entire transaction. 

Put another way, an object is visible if both of the following conditions are true:  
• At the time when the reader’s transaction started, the transaction that created the  object had already committed.
• The object is not marked for deletion, or if it is, the transaction that requested  deletion had not yet committed at the time when the reader’s transaction started. 

Snapshot isolation is a useful isolation level, especially for read-only transactions.  However, many databases that implement it call it by different names. In Oracle it is  called serializable, and in PostgreSQL and MySQL it is called repeatable read [23]. 



There are several other interesting kinds of conflicts that can occur between concur‐  rently writing transactions. The best known of these is the lost update problem,. The lost update problem can occur if an application reads some value from the data‐  base, modifies it, and writes back the modified value (a read-modify-write cycle). If  two transactions do this concurrently, one of the modifications can be lost, because  the second write does not include the first modification.

Atomic write operations 
Many databases provide atomic update operations, which remove the need to imple‐  ment read-modify-write cycles in application code. They are usually the best solution  if your code can be expressed in terms of those operations.

UPDATE counters SET value = value + 1 WHERE key = 'foo'; 

Atomic operations are usually implemented by taking an exclusive lock on the object  when it is read so that no other transaction can read it until the update has been applied. This technique is sometimes known as cursor stability

Explicit locking 

the application to explicitly lock  objects that are going to be updated. Then the application can perform a readmodify-write cycle, and if any other transaction tries to concurrently read the same  object, it is forced to wait until the first read-modify-write cycle has completed. 

BEGIN TRANSACTION;
SELECT * FROM figures  WHERE name = 'robot' AND game_id = 222  FOR UPDATE;  

-- Check whether move is valid, then update the position  
-- of the piece that was returned by the previous SELECT.  

UPDATE figures SET position = 'c4' WHERE id = 1234;  
COMMIT; 

PostgreSQL’s repeatable read, Oracle’s  serializable, and SQL Server’s snapshot isolation levels automatically detect when a  lost update has occurred and abort the offending transaction. However, MySQL/  InnoDB’s repeatable read does not detect lost updates

Compare-and-set 
In databases that don’t provide transactions, you sometimes find an atomic compareand-set operation. 

Conflict resolution and replication 
Locks and compare-and-set operations assume that there is a single up-to-date copy  of the data. However, databases with multi-leader or leaderless replication usually  allow several writes to happen concurrently and replicate them asynchronously, so  they cannot guarantee that there is a single up-to-date copy of the data. Thus, techni‐  ques based on locks or compare-and-set do not apply in this context.

in such replicated databases is to allow concurrent writes to create several  conflicting versions of a value (also known as siblings), and to use application code or  special data structures to resolve and merge these versions after the fact. 

Atomic operations can work well in a replicated context, especially if they are com‐  mutative (i.e., you can apply them in a different order on different replicas, and still  get the same result). For example, incrementing a counter or adding an element to a  set are commutative operations.

the last write wins (LWW) conflict resolution method is prone to  lost updates. Unfortunately, LWW is the default in many replicated databases. 


# Write Skew and Phantoms 
write skew [28]. It is neither a dirty write nor a lost update,  because the two transactions are updating two different objects (Alice’s and Bob’s oncall records, respectively). It is less obvious that a conflict occurred here, but it’s defi‐  nitely a race condition: if the two transactions had run one after another, the second doctor would have been prevented from going off call. The anomalous behavior was  only possible because the transactions ran concurrently. 
problem. Write  skew can occur if two transactions read the same objects, and then update some of  those objects (different transactions may update different objects). In the special case  where different transactions update the same object, you get a dirty write or lost  update anomaly
- Atomic single-object operations don’t help, as multiple objects are involved. 
- The automatic detection of lost updates that you find in some implementations  of snapshot isolation unfortunately doesn’t help either: write skew is not auto‐  matically detected in PostgreSQL’s repeatable read, MySQL/InnoDB’s repeatable  read, Oracle’s serializable, or SQL Server’s snapshot isolation level [23]. Auto‐  matically preventing write skew requires true serializable isolation
- If you can’t use a serializable isolation level, the second-best option in this case is  probably to explicitly lock the rows that the transaction depends on. 

check for the absence of rows matching some search con‐  dition, and the write adds a row matching the same condition. If the query in step 1  doesn’t return any rows, SELECT FOR UPDATE can’t attach locks to anything. 
This effect, where a write in one transaction changes the result of a search query in  another transaction, is called a phantom [3]. Snapshot isolation avoids phantoms in  read-only queries, but in read-write transactions like the examples we discussed,  phantoms can lead to particularly tricky cases of For example, in the meeting room booking case you could imagine creating a table of  time slots and rooms. Each row in this table corresponds to a particular room for a  particular time period (say, 15 minutes). You create rows for all possible combina‐  tions of rooms and time periods ahead of time, e.g. for the next six months.  Now a transaction that wants to create a booking can lock (SELECT FOR UPDATE) the  rows in the table that correspond to the desired room and time period. After it has  acquired the locks, it can check for overlapping bookings and insert a new booking as  before. Note that the additional table isn’t used to store information about the book‐  ing—it’s purely a collection of locks which is used to prevent bookings on the same  room and time range from being modified concurrently.  This approach is called materializing conflicts, because it takes a phantom and turns it  into a lock conflict on a concrete set of rows that exist in the database. Unfortu‐  nately, it can be hard and error-prone to figure out how to materialize conflicts, and  it’s ugly to let a concurrency control mechanism leak into the application data model.  For those reasons, materializing conflicts should be considered a last resort if no  alternative is possible. A serializable isolation level is much preferable in most cases. 

# Serializability 
Serializable isolation is usually regarded as the strongest isolation level. It guarantees  that even though transactions may execute in parallel, the end result is the same as if  they had executed one at a time, serially, without any concurrency. Thus, the database  guarantees that if the transactions behave correctly when run individually, they con‐  tinue to be correct when run concurrently—in other words, the database prevents all  possible race conditions. 

Most databases that  provide serializability today use one of three techniques
- executing transactions in a serial order
- Two-phase locking
- Optimistic concurrency control techniques such as serializable snapshot isolation 

Two-Phase Locking (2PL) 
if two transactions concurrently try to write to the same object,  the lock ensures that the second writer must wait until the first one has finished its  transaction (aborted or committed) before it may continue. 

Two-phase locking is similar, but makes the lock requirements much stronger. Sev‐  eral transactions are allowed to concurrently read the same object as long as nobody  is writing to it. But as soon as anyone wants to write (modify or delete) an object,  exclusive access is required: 

- If transaction A has read an object and transaction B wants to write to that  object, B must wait until A commits or aborts before it can continue. (This  ensures that B can’t change the object unexpectedly behind A’s back.) 
• If transaction A has written an object and transaction B wants to read that object,  B must wait until A commits or aborts before it can continue.

In 2PL, writers don’t just block other writers; they also block readers and vice versa. 
The lock can either be in shared mode or in exclusive mode.
- a transaction wants to read an object, it must first acquire the lock in shared  mode. Several transactions are allowed to hold the lock in shared mode simulta‐  neously, but if another transaction already has an exclusive lock on the object,  these transactions must wait.  • If a transaction wants to write to an object, it must first acquire the lock in exclu‐  sive mode. No other transaction may hold the lock at the same time (either in  shared or in exclusive mode), so if there is any existing lock on the object, the  transaction must wait.  • If a transaction first reads and then writes an object, it may upgrade its shared  lock to an exclusive lock. The upgrade works the same as getting an exclusive  lock directly.  • After a transaction has acquired the lock, it must continue to hold the lock until  the end of the transaction (commit or abort). This is where the name “twophase” comes from: the first phase (while the transaction is executing) is when  the locks are acquired, and the second phase (at the end of the transaction) is  when all the locks are released. 

Since so many locks are in use, it can happen quite easily that transaction A is stuck  waiting for transaction B to release its lock, and vice versa. This situation is called  deadlock. The database automatically detects deadlocks between transactions and  aborts one of them so that the others can make progress. The aborted transaction  needs to be retried by the application. 

predicate lock works sim‐  ilarly to the shared/exclusive lock described earlier, but rather than belonging to a  particular object (e.g., one row in a table), it belongs to all objects that match some  search condition,

The key idea here is that a predicate lock applies even to objects that do not yet exist  in the database, but which might be added in the future (phantoms). If two-phase  locking includes predicate locks, the database prevents all forms of write skew and  other race conditions, and so its isolation becomes serializable. 

Unfortunately, predicate locks do not perform well: if there are many locks by active  transactions, checking for matching locks becomes time-consuming. For that reason,  most databases with 2PL actually implement index-range locking (also known as nextkey locking), which is a simplified approximation of predicate locking

Index-range locks 
provides effective protection against phantoms and write skew. Index-range  locks are not as precise as predicate locks would be (they may lock a bigger range of objects than is strictly necessary to maintain serializability), but since they have much  lower overheads, they are a good compromise.  If there is no suitable index where a range lock can be attached, the database can fall  back to a shared lock on the entire table. This will not be good for performance, since  it will stop all other transactions writing to the table, but it’s a safe fallback position. 

# Serializable Snapshot Isolation (SSI) 
serializable snapshot isolation (SSI) is very promis‐  ing. It provides full serializability, but has only a small performance penalty com‐  pared to snapshot isolation.

Two-phase locking is a so-called pessimistic concurrency control mechanism: it is  based on the principle that if anything might possibly go wrong (as indicated by a  lock held by another transaction), it’s better to wait until the situation is safe again  before doing anything. It is like mutual exclusion, which is used to protect data struc‐  tures in multi-threaded programming. 

By contrast, serializable snapshot isolation is an optimistic concurrency control tech‐  nique. Optimistic in this context means that instead of blocking if something poten‐  tially dangerous happens, transactions continue anyway, in the hope that everything  will turn out all right. When a transaction wants to commit, the database checks  whether anything bad happened (i.e., whether isolation was violated); if so, the transaction is aborted and has to be retried. Only transactions that executed serializably  are allowed to commit. 

As the name suggests, SSI is based on snapshot isolation—that is, all reads within a  transaction are made from a consistent snapshot of the database. This is the main difference compared to ear‐  lier optimistic concurrency control techniques. On top of snapshot isolation, SSI adds  an algorithm for detecting serialization conflicts among writes and determining  which transactions to abort. 

How does the database know if a query result might have changed? There are two  cases to consider:
• Detecting reads of a stale MVCC object version (uncommitted write occurred  before the read)
• Detecting writes that affect prior reads (the write occurs after the read) 

Detecting stale MVCC reads 
snapshot isolation is usually implemented by multi-version concurrency  control (MVCC; see Figure 7-10). When a transaction reads from a consistent snap‐  shot in an MVCC database, it ignores writes that were made by any other transac‐  tions that hadn’t yet committed at the time when the snapshot was taken.



