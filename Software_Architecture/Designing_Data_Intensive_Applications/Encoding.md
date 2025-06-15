# Encoding

- Backward compatibility - Newer code can read data that was written by older code.
- Forward compatibility - Older code can read data that was written by newer code.

These encoding libraries are very convenient, because they allow in-memory objects
to be saved and restored with minimal additional code. However, they also have a
number of deep problems:
• The encoding is often tied to a particular programming language, and reading
the data in another language is very difficult. If you store or transmit data in such
an encoding, you are committing yourself to your current programming lan‐
guage for potentially a very long time, and precluding integrating your systems
with those of other organizations (which may use different languages).
• In order to restore data in the same object types, the decoding process needs to
be able to instantiate arbitrary classes. This is frequently a source of security
problems [5]: if an attacker can get your application to decode an arbitrary byte
sequence, they can instantiate arbitrary classes, which in turn often allows them
to do terrible things such as remotely executing arbitrary code [6, 7].
Versioning data is often an afterthought in these libraries: as they are intended
for quick and easy encoding of data, they often neglect the inconvenient prob‐
lems of forward and backward compatibility.
• Efficiency (CPU time taken to encode or decode, and the size of the encoded
structure) is also often an afterthought. For example, Java’s built-in serialization
is notorious for its bad performance and bloated encoding

Thrift and Protocol Buffers
Apache Thrift [15] and Protocol Buffers (protobuf) [16] are binary encoding libraries
that are based on the same principle. Protocol Buffers was originally developed at
Google, Thrift was originally developed at Facebook, and both were made open
source in 2007–08 [17].
Both Thrift and Protocol Buffers require a schema for any data that is encoded. To
encode the data in Example 4-1 in Thrift, you would describe the schema in the
Thrift interface definition language (IDL) like this:
```json
struct Person {
  1: required string       userName,
  2: optional i64          favoriteNumber,
  3: optional list<string> interests
}
```
Actually, it has three—BinaryProtocol, CompactProtocol, and DenseProtocol—although DenseProtocol
is only supported by the C++ implementation, so it doesn’t count as cross-language [18]. Besides those, it also
has two different JSON-based encoding formats [19]. What fun!
The equivalent schema definition for Protocol Buffers looks very similar:
message Person {
    required string user_name       = 1;
    optional int64  favorite_number = 2;
    repeated string interests       = 3;
}
Thrift and Protocol Buffers each come with a code generation tool that takes a
schema definition like the ones shown here, and produces classes that implement the
schema in various programming languages [18]. Your application code can call this
generated code to encode or decode records of
Modes of Dataflow

forward and backward compatibility, which are important for evolv‐
ability (making change easy by allowing you to upgrade different parts of your system
independently, and not having to change everything at once). Compatibility is a rela‐
tionship between one process that encodes the data, and another process that decodes
it.

value in the database may be written by a newer version of the
code, and subsequently read by an older version of the code that is still running.
Thus, forward compatibility is also often required for databases.
Say you add a field to a record schema, and the
newer code writes a value for that new field to the database. Subsequently, an older
version of the code (which doesn’t yet know about the new field) reads the record,
updates it, and writes it back. In this situation, the desirable behavior is usually for
the old code to keep the new field intact, even though it couldn’t be interpreted.

A database generally allows any value to be updated at any time. This means that
within a single database you may have some values that were written five milli‐
seconds ago, and some values that were written five years ago. This
observation is sometimes summed up as data outlives code.
LinkedIn’s document database Espresso uses Avro for storage, allowing it to use
Avro’s schema evolution rules. Schema evolution thus allows the entire database to appear as if it was encoded with a single schema, even though the underlying storage may contain records encoded with various historical versions of the schema


# Dataflow Through Services: REST and RPC
When you have processes that need to communicate over a network, there are a few
different ways of arranging that communication. The most common arrangement is
to have two roles: clients and servers.
server can itself be a client to another service (for example, a typical web
app server acts as client to a database). This approach is often used to decompose a
large application into smaller services by area of functionality, such that one service
makes a request to another when it requires some functionality or data from that
other service. This way of building applications has traditionally been called a service-
oriented architecture (SOA), more recently refined and rebranded as microservices
architecture
 SOAP messages are often too
complex to construct manually, users of SOAP rely heavily on tool support, code
generation, and IDEs [38]. For users of programming languages that are not sup‐
ported by SOAP vendors, integration with SOAP services is difficult.

remote procedure call (RPC), which has been
around since the 1970s [42]. The RPC model tries to make a request to a remote net‐
work service look the same as calling a function or method in your programming lan‐
guage, within the same process (this abstraction is called location transparency).
Although RPC seems convenient at first, the approach is fundamentally flawed [43,
44]. A network request is very different from a local function call:
- A local function call is predictable and either succeeds or fails, depending only on
parameters that are under your control. A network request is unpredictable: the
request or response may be lost due to a network problem, or the remote
machine may be slow or unavailable, and such problems are entirely outside of
your control. Network problems are common, so you have to anticipate them,
for example by retrying a failed request.

- A local function call either returns a result, or throws an exception, or never
returns (because it goes into an infinite loop or the process crashes). A network
request has another possible outcome: it may return without a result, due to a
timeout.
- If you retry a failed network request, it could happen that the requests are
actually getting through, and only the responses are getting lost. In that case,
retrying will cause the action to be performed multiple times, unless you build a
mechanism for deduplication (idempotence) into the protocol.

gRPC supports streams, where a call consists of not
just one request and one response, but a series of requests and responses over time

Some of these frameworks also provide service discovery—that is, allowing a client to
find out at which IP address and port number it can find a particular service.

Custom RPC protocols with a binary encoding format can achieve better perfor‐
mance than something generic like JSON over REST. However, a RESTful API has
other significant advantages: it is good for experimentation and debugging (you can
simply make requests to it using a web browser or the command-line tool curl,
without any code generation or software installation), it is supported by all main‐
stream programming languages and platforms, and there is a vast ecosystem of tools
available (servers, caches, load balancers, proxies, firewalls, monitoring, debugging
tools, testing tools, etc.). For these reasons, REST seems to be the predominant style for public APIs. The main
focus of RPC frameworks is on requests between services owned by the same organi‐
zation, typically within the same datacenter.  For these reasons, REST seems to be the predominant style for public APIs. The main
focus of RPC frameworks is on requests between services owned by the same organi‐
zation, typically within the same datacenter.
we can make a simplifying assumption in the case of dataflow
through services: it is reasonable to assume that all the servers will be updated first,
and all the clients second. Thus, you only need backward compatibility on requests,
and forward compatibility on responses.

The backward and forward compatibility properties of an RPC scheme are inherited
from whatever encoding it uses:
• Thrift, gRPC (Protocol Buffers), and Avro RPC can be evolved according to the
compatibility rules of the respective encoding format.
• In SOAP, requests and responses are specified with XML schemas. These can be
evolved, but there are some subtle pitfalls [47].
• RESTful APIs most commonly use JSON (without a formally specified schema)
for responses, and JSON or URI-encoded/form-encoded request parameters for
requests. Adding optional request parameters and adding new fields to response
objects are usually considered changes that maintain compatibility.

Message-Passing Dataflow
asynchronous message-passing systems,
which are somewhere between RPC and databases. They are similar to RPC in that a
client’s request (usually called a message) is delivered to another process with low
latency. They are similar to databases in that the message is not sent via a direct net‐
work connection, but goes via an intermediary called a message broker (also called a
message queue or message-oriented middleware), which stores the message temporar‐
ily.
Using a message broker has several advantages compared to direct RPC:
• It can act as a buffer if the recipient is unavailable or overloaded, and thus
improve system reliability.
• It can automatically redeliver messages to a process that has crashed, and thus
prevent messages from being lost.
• It avoids the sender needing to know the IP address and port number of the
recipient (which is particularly useful in a cloud deployment where virtual
machines often come and go).
• It allows one message to be sent to several recipients.
• It logically decouples the sender from the recipient (the sender just publishes
messages and doesn’t care who consumes them).

The detailed delivery semantics vary by implementation and configuration, but in
general, message brokers are used as follows: one process sends a message to a named
queue or topic, and the broker ensures that the message is delivered to one or more
consumers of or subscribers to that queue or topic. There can be many producers and
many consumers on the same topic.
A topic provides only one-way dataflow. However, a consumer may itself publish
messages to another topic (so you can chain them together,

The actor model is a programming model for concurrency in a single process. Rather
than dealing directly with threads (and the associated problems of race conditions,
locking, and deadlock), logic is encapsulated in actors. Each actor typically represents
one client or entity, it may have some local state (which is not shared with any other
actor), and it communicates with other actors by sending and receiving asynchro‐
nous messages. Message delivery is not guaranteed: in certain error scenarios, mes‐
sages will be lost. Since each actor processes only one message at a time, it doesn’t
need to worry about threads, and each actor can be scheduled independently by the
framework.

A distributed actor framework essentially integrates a message broker and the actor
programming model into a single framework. However, if you want to perform roll‐
ing upgrades of your actor-based application, you still have to worry about forward
and backward compatibility, as messages may be sent from a node running the new
version to a node running the old version, and vice versa.

Three popular distributed actor frameworks handle message encoding as follows:
 Akka uses Java’s built-in serialization by default, which does not provide forward
or backward compatibility. However, you can replace it with something like Pro‐
tocol Buffers, and thus gain the ability to do rolling upgrades [50].During rolling upgrades, or for various other reasons, we must assume that different
nodes are running the different versions of our application’s code. Thus, it is impor‐
tant that all data flowing around the system is encoded in a way that provides back‐
ward compatibility (new code can read old data) and forward compatibility (old code
can read new data).

several modes of dataflow, illustrating different scenarios in which
data encodings are important:
• Databases, where the process writing to the database encodes the data and the
process reading from the database decodes it
• RPC and REST APIs, where the client encodes a request, the server decodes the
request and encodes a response, and the client finally decodes the response
• Asynchronous message passing (using message brokers or actors), where nodes
communicate by sending each other messages that are encoded by the sender
and decoded by the recipient
