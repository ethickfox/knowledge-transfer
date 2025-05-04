---
Interview graded: true
Last edited time: 2024-04-21T17:13
Last recall: 2023-07-22
Needs Rework: false
Status: Not started
Topic:
  - "[[Data Structures]]"
  - "[[Core\\|Java Core]]"
---
## EnumMap

  

## Questions

[`**Collection**`](http://docs.oracle.com/javase/7/docs/api/java/util/Collection.html): An interface representing an unordered "bag" of items, called "elements". The "next" element is undefined (random).

- **Describe the Collections Type Hierarchy. What Are the Main Interfaces, and What Are the Differences Between Them?**
    
    he _**Iterable**_ interface represents any collection that can be iterated using the _for-each_ loop. The _**Collection**_ interface inherits from _Iterable_ and adds generic methods for checking if an element is in a collection, adding and removing elements from the collection, determining its size etc.
    
    The _**List**_, _**Set**_, and _**Queue**_ interfaces inherit from the _Collection_ interface.
    
    _**List**_ is an ordered collection, and its elements can be accessed by their index in the list.
    
    _**Set**_ is an unordered collection with distinct elements, similar to the mathematical notion of a set.
    
    _**Queue**_ is a collection with additional methods for adding, removing and examining elements, useful for holding elements prior to processing.
    
    _**Map**_ interface is also a part of the collection framework, yet it does not extend _Collection_. This is by design, to stress the difference between collections and mappings which are hard to gather under a common abstraction. The _Map_ interface represents a key-value data structure with unique keys and no more than one value for each key.
    

### Iterators

- **Describe and compare fail-fast and fail-safe iterators.**
    
    The main distinction between **fail-fast** and **fail-safe** iterators is whether or not the collection can be modified _while_ it is being iterated. Fail-safe iterators allow this; fail-fast iterators do not.
    
    - **Fail-fast** iterators operate directly on the collection itself. During iteration, fail-fast iterators fail as soon as they realize that the collection has been modified (i.e., upon realizing that a member has been added, modified, or removed) and will throw a [`ConcurrentModificationException`](http://docs.oracle.com/javase/6/docs/api/java/util/ConcurrentModificationException.html). Some examples include [`ArrayList`](http://docs.oracle.com/javase/7/docs/api/java/util/ArrayList.html), [`HashSet`](http://docs.oracle.com/javase/7/docs/api/java/util/HashSet.html), and [`HashMap`](http://docs.oracle.com/javase/7/docs/api/java/util/HashMap.html) (most JDK1.4 collections are implemented to be fail-fast).
    - **Fail-safe** iterates operate on a _cloned copy_ of the collection and therefore do _not_ throw an exception if the collection is modified during iteration. Examples would include iterators returned by [`ConcurrentHashMap`](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ConcurrentHashMap.html) or [`CopyOnWriteArrayList`](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/CopyOnWriteArrayList.html).

### List

- `ArrayList`**,** `LinkedList`**, and** `Vector` **are all implementations of the** `List` **interface. Which of them is most efficient for adding and removing elements from the list? Explain your answer, including any other alternatives you may be aware of.**
    
    Of the three, [`LinkedList`](http://docs.oracle.com/javase/7/docs/api/java/util/LinkedList.html) is generally going to give you the best performance. Here’s why:
    
    _**ArrayList**_ is an implementation of the _List_ interface that is based on an array. _ArrayList_ internally handles resizing of this array when the elements are added or removed. You can access its elements in constant time by their index in the array. However, inserting or removing an element infers shifting all consequent elements which may be slow if the array is huge and the inserted or removed element is close to the beginning of the list.
    
    [`ArrayList`](http://docs.oracle.com/javase/7/docs/api/java/util/ArrayList.html) and [`Vector`](http://docs.oracle.com/javase/7/docs/api/java/util/Vector.html) each use an array to store the elements of the list. As a result, when an element is inserted into (or removed from) the middle of the list, the elements that follow must all be shifted accordingly. `Vector` is synchronized, so if a thread-safe implementation is _not_ needed, it is recommended to use `ArrayList` rather than Vector.
    
    [`LinkedList`](http://docs.oracle.com/javase/7/docs/api/java/util/LinkedList.html), on the other hand, is implemented using a doubly linked list. As a result, an inserting or removing an element only requires updating the links that immediately precede and follow the element being inserted or removed. _**LinkedList**_ is a doubly-linked list: single elements are put into _Node_ objects that have references to previous and next _Node_. This implementation may appear more efficient than _ArrayList_ if you have lots of insertions or deletions in different parts of the list, especially if the list is large. In most cases, however, _ArrayList_ outperforms _LinkedList_. Even elements shifting in _ArrayList_, while being an O(n) operation, is implemented as a very fast _System.arraycopy()_ call. It can even appear faster than the _LinkedList_‘s O(1) insertion which requires instantiating a _Node_ object and updating multiple references under the hood. _LinkedList_ also can have a large memory overhead due to a creation of multiple small _Node_ objects.
    
    However, it is worth noting that if performance is that critical, it’s better to just use an array and manage it yourself, or use one of the high performance 3rd party packages such as [Trove](http://trove.starlight-systems.com/) or [HPPC](http://labs.carrotsearch.com/hppc.html).
    

- [`**List**`](http://docs.oracle.com/javase/7/docs/api/java/util/List.html): An interface representing a `Collection` whose elements are ordered and each have a numeric index representing its position, where zero is the first element, and `(length - 1)` is the last.
    
    - [`**ArrayList**`](http://docs.oracle.com/javase/7/docs/api/java/util/ArrayList.html): A `List` backed by an array, where the array has a length (called "capacity") that is at least as large as the number of elements (the list's "size"). When size exceeds capacity (when the `(capacity + 1)-th` element is added), the array is recreated with a new capacity of `(new length * 1.5)`-this recreation is fast, since it uses [`System.arrayCopy()`](http://docs.oracle.com/javase/7/docs/api/java/lang/System.html#arraycopy(java.lang.Object,%20int,%20java.lang.Object,%20int,%20int)). Deleting and inserting/adding elements requires all neighboring elements (to the right) be shifted into or out of that space. Accessing any element is fast, as it only requires the calculation `(element-zero-address + desired-index * element-size)` to find it's location. [In most situations](https://stackoverflow.com/questions/322715/when-to-use-linkedlist-over-arraylist), an `ArrayList` is preferred over a `LinkedList`.
    - [`**LinkedList**`](http://docs.oracle.com/javase/7/docs/api/java/util/LinkedList.html): A `List` backed by a set of objects, each linked to its "previous" and "next" neighbors. A `LinkedList` is also a `Queue` and `Deque`. Accessing elements is done starting at the first or last element, and traversing until the desired index is reached. Insertion and deletion, _once the desired index is reached via traversal_ is a trivial matter of re-mapping only the immediate-neighbor links to point to the new element or bypass the now-deleted element.
    
    |   |   |   |   |   |   |
    |---|---|---|---|---|---|
    ||**Add**|**Remove**|**Get**|**Contains**|**Data  Structure**|
    |**ArrayList**|O(1)|O(n)|O(1)|O(n)|Array|
    |**LinkedList**|O(1)|O(1)|O(n)|O(n)|Linked List|
    |**CopyonWriteArrayList**|O(n)|O(n)|O(1)|O(n)|Array|
    

- **Why is the remove method faster in the linked list than in an array?**
    
    In the linked list, we only need to adjust the references when we want to delete the element from either end or the front of the linked list. But in the array, indexes are used. So to manage proper indexing, we need to adjust the values from the array So this adjustment of value is costlier than the adjustment of references.
    
    **Example** - To Delete from the front of the linked list, internally the references adjustments happened like this.
    
    The only thing that will change is that the head pointer will point to the head’s next node. And delete the previous node. That is the constant time operation.
    
    Whereas in the ArrayList, internally it should work like this-
    
    For deletion of the first element, all the next element has to move to one place ahead. So this copying value takes time. So that is the reason why removing in ArrayList is slower than LinkedList.
    
- **How does the size of ArrayList grow dynamically? And also state how it is implemented internally.**
    
    ArrayList is implemented in such a way that it can grow dynamically. We don't need to specify the size of ArrayList. For adding the values in it, the methodology it uses is
    
    1. Consider initially that there are 2 elements in the ArrayList. **[2, 3]**.
    
    2. If we need to add the element into this. Then internally what will happen is-
    
    - ArrayList will allocate the new ArrayList of Size (current size + half of the current size). And add the old elements into the new. Old - [2, 3], New - [2, 3, null].
    - Then the new value will be inserted into it. [2, 3, 4, null]. And for the next time, the extra space will be available for the value to be inserted.
    
    1. This process continues and the time taken to perform all of these is considered as the amortized constant time.
    
    This is how the ArrayList grows dynamically. And when we delete any entry from the ArrayList then the following steps are performed -
    
    1. It searches for the element index in the array. Searching takes some time. Typically it’s O(n) because it needs to search for the element in the entire array.
    2. 2. After searching the element, it needs to shift the element from the right side to fill the index.
    
    So this is how the elements are deleted from the ArrayList internally. Similarly, the search operations are also implemented internally as defined in removing elements from the list (searching for elements to delete).
    

### Set

- **What makes a HashSet different from a TreeSet?**
    
    Although both HashSet and TreeSet are not synchronized and ensure that duplicates are not present, there are certain properties that distinguish a HashSet from a TreeSet.
    
    - **Implementation:** For a HashSet, the hash table is utilized for storing the elements in an unordered manner. However, TreeSet makes use of the red-black tree to store the elements in a sorted manner.
    - **Complexity/ Performance:** For adding, retrieving, and deleting elements, the time amortized complexity is O(1) for a HashSet. The time complexity for performing the same operations is a bit higher for TreeSet and is equal to O(log n). Overall, the performance of HashSet is faster in comparison to TreeSet.
    - **Methods:** hashCode() and equals() are the methods utilized by HashSet for making comparisons between the objects. Conversely, compareTo() and compare() methods are utilized by TreeSet to facilitate object comparisons.
    - **Objects type:** Heterogeneous and null objects can be stored with the help of HashSet. In the case of a TreeSet, runtime exception occurs while inserting heterogeneous objects or null objects.

- [`**Set**`](http://docs.oracle.com/javase/7/docs/api/java/util/Set.html): An interface representing a `Collection` with no duplicates.
    
    - [`**HashSet**`](http://docs.oracle.com/javase/7/docs/api/java/util/HashSet.html): A `Set` backed by a [`Hashtable`](http://docs.oracle.com/javase/7/docs/api/java/util/Hashtable.html). Fastest and smallest memory usage, when ordering is unimportant.
    - [`**LinkedHashSet**`](http://docs.oracle.com/javase/7/docs/api/java/util/LinkedHashSet.html): A `HashSet` with the addition of a linked list to associate elements in _insertion order_. The "next" element is the next-most-recently inserted element.
    - [`**TreeSet**`](http://docs.oracle.com/javase/7/docs/api/java/util/TreeSet.html): A `Set` where elements are ordered by a [`Comparator`](http://docs.oracle.com/javase/7/docs/api/java/util/Comparator.html) (typically [natural ordering](http://docs.oracle.com/javase/tutorial/collections/interfaces/order.html)). Slowest and largest memory usage, but necessary for comparator-based ordering.
    - [`**EnumSet**`](http://docs.oracle.com/javase/7/docs/api/java/util/EnumSet.html): An extremely fast and efficient `Set` customized for a single enum type.
    
    |   |   |   |   |
    |---|---|---|---|
    |**Add**|**Contains**|**Next**|**Data Structure**|
    |**HashSet**|O(1)|O(1)|O(h/n)|
    |**LinkedHashSet**|O(1)|O(1)|O(1)|
    |**EnumSet**|O(1)|O(1)|O(1)|
    |**TreeSet**|O(log n)|O(log n)|O(log n)|
    |**CopyonWriteArraySet**|O(n)|O(n)|O(1)|
    |**ConcurrentSkipList**|O(log n)|O(log n)|O(1)|
    

### Queue

- [`**Queue**`](http://docs.oracle.com/javase/7/docs/api/java/util/Queue.html): An interface that represents a `Collection` where elements are, typically, added to one end, and removed from the other (FIFO: first-in, first-out).
- [`**Deque**`](http://docs.oracle.com/javase/7/docs/api/java/util/Deque.html): Short for "double ended queue", usually pronounced "deck". A linked list that is typically only added to and read from either end (not the middle).

|   |   |   |   |   |   |
|---|---|---|---|---|---|
||**Offer**|**Peak**|**Poll**|**Size**|**Data Structure**|
|**PriorityQueue**|O(log n )|O(1)|O(log n)|O(1)|Priority Heap|
|**LinkedList**|O(1)|O(1)|O(1)|O(1)|Array|
|**ArrayDequeue**|O(1)|O(1)|O(1)|O(1)|Linked List|
|**ConcurrentLinkedQueue**|O(1)|O(1)|O(1)|O(n)|Linked List|
|**ArrayBlockingQueue**|O(1)|O(1)|O(1)|O(1)|Array|
|**PriorirityBlockingQueue**|O(log n)|O(1)|O(log n)|O(1)|Priority Heap|
|**SynchronousQueue**|O(1)|O(1)|O(1)|O(1)|None!|
|**DelayQueue**|O(log n)|O(1)|O(log n)|O(1)|Priority Heap|
|**LinkedBlockingQueue**|O(1)|O(1)|O(1)|O(1)|Linked List|

- **What do you understand by BlockingQueue?**
    
    lockingQueue is an interface which extends the Queue interface. It provides concurrency in the operations like retrieval, insertion, deletion. While retrieval of any element, it waits for the queue to be non-empty. While storing the elements, it waits for the available space. BlockingQueue cannot contain null elements, and implementation of BlockingQueue is thread-safe.
    

### Map

- [`**Map**`](http://docs.oracle.com/javase/7/docs/api/java/util/Map.html): An interface representing an `Collection` where each element has an identifying "key"--each element is a key-value pair.
    
    - [`**HashMap**`](http://docs.oracle.com/javase/7/docs/api/java/util/HashMap.html): A `Map` where keys are unordered, and backed by a `Hashtable`. One of the most often used implementations of the _Map_ interface is the _**HashMap**_. It is a typical hash map data structure that allows accessing elements in constant time, or O(1), but **does not preserve order and is not thread-safe**.
    - [`**LinkedhashMap**`](http://docs.oracle.com/javase/7/docs/api/java/util/LinkedhashMap.html): Keys are ordered by _insertion order_. To preserve insertion order of elements, you can use the _**LinkedHashMap**_ class which extends the _HashMap_ and additionally ties the elements into a linked list, with foreseeable overhead.
    - [`**TreeMap**`](http://docs.oracle.com/javase/7/docs/api/java/util/TreeMap.html): A `Map` where keys are ordered by a `Comparator` (typically natural ordering).The _**TreeMap**_ class stores its elements in a red-black tree structure, which allows accessing elements in logarithmic time, or O(log(n)). It is slower than the _HashMap_ for most cases, but it allows keeping the elements in order according to some _Comparator_.
    - The **ConcurrentHashMap** is a thread-safe implementation of a hash map. It provides full concurrency of retrievals (as the _get_ operation does not entail locking) and high expected
    - The _**Hashtable**_ class has been in Java since version 1.0. It is not deprecated but is mostly considered obsolete. It is a thread-safe hash map, but unlike _ConcurrentHashMap_, all its methods are simply _synchronized_, which means that all operations on this map block, even retrieval of independent values.concurrency of updates.
    
    |   |   |   |   |
    |---|---|---|---|
    |**Get**|**ContainsKey**|**Next**|**Data Structure**|
    |**HashMap**|O(1)|O(1)|O(h / n)|
    |**LinkedHashMap**|O(1)|O(1)|O(1)|
    |**IdentityHashMap**|O(1)|O(1)|O(h / n)|
    |**WeakHashMap**|O(1)|O(1)|O(h / n)|
    |**EnumMap**|O(1)|O(1)|O(1)|
    |**TreeMap**|O(log n)|O(log n)|O(log n)|
    |**ConcurrentHashMap**|O(1)|O(1)|O(h / n)|
    |**ConcurrentSkipListMap**|O(log n)|O(log n)|O(1)|
    

- **How Is Hashmap Implemented in Java? How Does Its Implementation Use Hashcode and Equals Methods of Objects? What Is the Time Complexity of Putting and Getting an Element from Such Structure?**
    
    The _HashMap_ class represents a typical hash map data structure with certain design choices.
    
    The _HashMap_ is backed by a resizable array that has a size of power-of-two. When the element is added to a _HashMap_, first its _hashCode_ is calculated (an _int_ value). Then a certain number of lower bits of this value are used as an array index. This index directly points to the cell of the array (called a bucket) where this key-value pair should be placed. Accessing an element by its index in an array is a very fast O(1) operation, which is the main feature of a hash map structure.
    
    A _hashCode_ is not unique, however, and even for different _hashCodes_, we may receive the same array position. This is called a collision. There is more than one way of resolving collisions in the hash map data structures. In Java’s _HashMap_, each bucket actually refers not to a single object, but to a red-black tree of all objects that landed in this bucket (prior to Java 8, this was a linked list).
    
    So when the _HashMap_ has determined the bucket for a key, it has to traverse this tree to put the key-value pair in its place. If a pair with such key already exists in the bucket, it is replaced with a new one.
    
    To retrieve the object by its key, the _HashMap_ again has to calculate the _hashCode_ for the key, find the corresponding bucket, traverse the tree, call _equals_ on keys in the tree and find the matching one.
    
    _HashMap_ has O(1) complexity, or constant-time complexity, of putting and getting the elements. Of course, lots of collisions could degrade the performance to O(log(n)) time complexity in the worst case, when all elements land in a single bucket. This is usually solved by providing a good hash function with a uniform distribution.
    
    When the _HashMap_ internal array is filled (more on that in the next question), it is automatically resized to be twice as large. This operation infers rehashing (rebuilding of internal data structures), which is costly, so you should plan the size of your _HashMap_ beforehand.
    
- **What Is the Purpose of the Initial Capacity and Load Factor Parameters of a Hashmap? What Are Their Default Values?**
    
    The _initialCapacity_ argument of the _HashMap_ constructor affects the size of the internal data structure of the _HashMap_, but reasoning about the actual size of a map is a bit tricky. The _HashMap_‘s internal data structure is an array with the power-of-two size. So the _initialCapacity_ argument value is increased to the next power-of-two (for instance, if you set it to 10, the actual size of the internal array will be 16).
    
    The load factor of a _HashMap_ is the ratio of the element count divided by the bucket count (i.e. internal array size). For instance, if a 16-bucket _HashMap_ contains 12 elements, its load factor is 12/16 = 0.75. A high load factor means a lot of collisions, which in turn means that the map should be resized to the next power of two. So the _loadFactor_ argument is a maximum value of the load factor of a map. When the map achieves this load factor, it resizes its internal array to the next power-of-two value.
    
    The _initialCapacity_ is 16 by default, and the loadFactor is 0.75 by default, so you could put 12 elements in a _HashMap_ that was instantiated with the default constructor, and it would not resize. The same goes for the _HashSet_, which is backed by a _HashMap_ instance internally.
    
    Consequently, it is not trivial to come up with _initialCapacity_ that satisfies your needs. This is why the Guava library has _Maps.newHashMapWithExpectedSize()_ and _Sets.newHashSetWithExpectedSize()_ methods that allow you to build a _HashMap_ or a _HashSet_ that can hold the expected number of elements without resizing.
    

### Stack

- [`**Stack**`](http://docs.oracle.com/javase/7/docs/api/java/util/Stack.html): An interface that represents a `Collection` where elements are, typically, both added (pushed) and removed (popped) from the same end (LIFO: last-in, first-out).

- **Immutable collections**
    
    Unmodifiable collections are usually read-only views (wrappers) of other collections. You can't add, remove or clear them, but the underlying collection can change.
    
    Immutable collections can't be changed at all - they don't wrap another collection - they have their own elements.
    
    Collections.unmodifiable* - wraps collection into unmodifiable, returns UnsupportedOperationException for editing operations and returns object itself in case of getting operations. So objects should be immutable too.