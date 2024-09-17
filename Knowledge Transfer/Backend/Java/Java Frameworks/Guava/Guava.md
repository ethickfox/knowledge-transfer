---
Duration: Invalid date
Interview graded: true
Last edited time: 2023-07-23T12:38
Last recall: 2023-07-22
Needs Rework: false
Status: Ready To Discuss
Topic:
  - "[[Data Structures]]"
---
==**Guava**== - open-source set of libraries for Java, developed by Google Engineers. It provides utility methods for collections (old and new), caching, primitive support, concurrency, common annotations, string processing, I/O and validations.

### Collections

Guava provides new Collection types:  
  
[https://github.com/google/guava/wiki/NewCollectionTypesExplained](https://github.com/google/guava/wiki/NewCollectionTypesExplained)

### ==**Table**==

Map with two keys. Works as replacement for cases like `Map<FirstName, Map<LastName, Person>>`

```Java
Table<Vertex, Vertex, Double> weightedGraph = HashBasedTable.create();
weightedGraph.put(v1, v2, 4);
weightedGraph.put(v1, v3, 20);
weightedGraph.put(v2, v3, 5);

weightedGraph.row(v1); // returns a Map mapping v2 to 4, v3 to 20
weightedGraph.column(v3); // returns a Map mapping v1 to 20, v2 to 5
```

**Implementations**:

- [`HashBasedTable`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/HashBasedTable.html), which is essentially backed by a `HashMap<R, HashMap<C, V>>`.
- [`TreeBasedTable`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/TreeBasedTable.html), which is essentially backed by a `TreeMap<R, TreeMap<C, V>>`.
- [`ImmutableTable`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/ImmutableTable.html)
- [`ArrayTable`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/ArrayTable.html), which requires that the complete universe of rows and columns be specified at construction time, but is backed by a two-dimensional array to improve speed and memory efficiency when the table is dense. 

```Java
Table<String, String, Double> weightedGraph = HashBasedTable.create();
weightedGraph.put("v1", "v2", 4.0);
weightedGraph.put("v1", "v3", 20.0);
weightedGraph.put("v2", "v3", 5.0);
```

Result:

```Java
		v1  v2  v3
v1      4.0 20.0
v2          5.0
v3
```

Consists of backingMap and rowMap

![[/Untitled 55.png|Untitled 55.png]]

### ==**BiMap**==

Map that

- allows you to view the "inverse" `BiMap<V, K>` with [`inverse()`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/BiMap.html#inverse--)
- ensures that values are unique, making [`values()`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/BiMap.html#values--) a `Set`

`BiMap.put(key, value)` will throw an `IllegalArgumentException`if you attempt to map a key to an already-present value. If you wish to delete any preexisting entry with the specified value, use [`BiMap.forcePut(key, value)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/BiMap.html#forcePut-java.lang.Object-java.lang.Object-)instead.

**Implementations**:

|   |   |   |
|---|---|---|
|Key-Value Map Impl|Value-Key Map Impl|Corresponding `BiMap`|
|`HashMap`|`HashMap`|[`HashBiMap`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/HashBiMap.html)|
|`ImmutableMap`|`ImmutableMap`|[`ImmutableBiMap`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/ImmutableBiMap.html)|
|`EnumMap`|`EnumMap`|[`EnumBiMap`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/EnumBiMap.html)|
|`EnumMap`|`HashMap`|[`EnumHashBiMap`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/EnumHashBiMap.html)|

### ==**Multimap**==

Map that allows to associate keys with arbitrarily many values.

```Java
a -> [1, 2, 4]
b -> [3]
c -> [5]
```

Usage:

```Java
// creates a ListMultimap with tree keys and array list values
ListMultimap<String, Integer> treeListMultimap =
    MultimapBuilder.treeKeys().arrayListValues().build();

// creates a SetMultimap with hash keys and enum set values
SetMultimap<Integer, MyEnum> hashEnumMultimap =
    MultimapBuilder.hashKeys().enumSetValues(MyEnum.class).build();
```

[`Multimap.get(key)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multimap.html#get-K-) returns a _view_ of the values associated with the specified key, even if there are none currently.

```Java
Set<Person> aliceChildren = childrenMultimap.get(alice);
aliceChildren.clear();
aliceChildren.add(bob);
aliceChildren.add(carol);
```

writes through to the underlying multimap.  
Other ways of modifying the multimap (more directly) include:  

|   |   |   |
|---|---|---|
|Signature|Description|Equivalent|
|[`put(K, V)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multimap.html#put-K-V-)|Adds an association from the key to the value.|`multimap.get(key).add(value)`|
|[`putAll(K, Iterable<V>)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multimap.html#putAll-K-java.lang.Iterable-)|Adds associations from the key to each of the values in turn.|`Iterables.addAll(multimap.get(key), values)`|
|[`remove(K, V)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multimap.html#remove-java.lang.Object-java.lang.Object-)|Removes one association from `key` to `value` and returns `true` if the multimap changed.|`multimap.get(key).remove(value)`|
|[`removeAll(K)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multimap.html#removeAll-java.lang.Object-)|Removes and returns all the values associated with the specified key. The returned collection may or may not be modifiable, but modifying it will not affect the multimap. (Returns the appropriate collection type.)|`multimap.get(key).clear()`|
|[`replaceValues(K, Iterable<V>)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multimap.html#replaceValues-K-java.lang.Iterable-)|Clears all the values associated with `key` and sets `key` to be associated with each of `values`. Returns the values that were previously associated with the key.|`multimap.get(key).clear(); Iterables.addAll(multimap.get(key), values)`|

`Multimap` also supports a number of powerful views. Such as asMap, entries, keySet, keys, values

**Implementations**:

|   |   |   |
|---|---|---|
|Implementation|Keys behave like...|Values behave like..|
|[`ArrayListMultimap`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/ArrayListMultimap.html)|`HashMap`|`ArrayList`|
|[`HashMultimap`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/HashMultimap.html)|`HashMap`|`HashSet`|
|[`LinkedListMultimap`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/LinkedListMultimap.html)|`LinkedHashMap`|`LinkedList`|
|[`LinkedHashMultimap`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/LinkedHashMultimap.html)|`LinkedHashMap`|`LinkedHashSet`|
|[`TreeMultimap`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/TreeMultimap.html)|`TreeMap`|`TreeSet`|
|[`ImmutableListMultimap`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/ImmutableListMultimap.html)|`ImmutableMap`|`ImmutableList`|
|[`ImmutableSetMultimap`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/ImmutableSetMultimap.html)|`ImmutableMap`|`ImmutableSet`|

If you need more customization, use [`Multimaps.newMultimap(Map, Supplier<Collection>)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multimaps.html#newMultimap-java.util.Map-com.google.common.base.Supplier-)or the [list](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multimaps.html#newListMultimap-java.util.Map-com.google.common.base.Supplier-) and [set](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multimaps.html#newSetMultimap-java.util.Map-com.google.common.base.Supplier-) versions to use a custom collection, list, or set implementation to back your multimap.

### ==**Multiset**==

Set that allows us to store values in multiple instances. It might be treated as:

- `ArrayList<E>` without an ordering constraint: ordering does not matter.
- `Map<E, Integer>`, with elements and counts.

As an advantage it have method `count(Object)` returns the count associated with that element.

|   |   |
|---|---|
|Method|Description|
|[`count(E)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multiset.html#count-java.lang.Object-)|Count the number of occurrences of an element that have been added to this multiset.|
|[`elementSet()`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multiset.html#elementSet--)|View the distinct elements of a `Multiset<E>` as a `Set<E>`.|
|[`entrySet()`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multiset.html#entrySet--)|Similar to `Map.entrySet()`, returns a `Set<Multiset.Entry<E>>`, containing entries supporting `getElement()` and `getCount()`.|
|[`add(E, int)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multiset.html#add-java.lang.Object-int-)|Adds the specified number of occurrences of the specified element.|
|[`remove(E, int)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multiset.html#remove-java.lang.Object-int--)|Removes the specified number of occurrences of the specified element.|
|[`setCount(E, int)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/Multiset.html#setCount-E-int-)|Sets the occurrence count of the specified element to the specified nonnegative value.|
|`size()`|Returns the total number of occurrences of all elements in the `Multiset`.|

Note that`Multiset<E>` is not a `Map<E, Integer>`, though that might be part of a Multiset implementation. Multiset is a true Collection type, and satisfies all of the associated contractual obligations.

**Implementations**:

|   |   |   |
|---|---|---|
|Map|Corresponding Multiset|Supports `null` elements|
|`HashMap`|[`HashMultiset`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/HashMultiset.html)|Yes|
|`TreeMap`|[`TreeMultiset`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/TreeMultiset.html)|Yes|
|`LinkedHashMap`|[`LinkedHashMultiset`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/LinkedHashMultiset.html)|Yes|
|`ConcurrentHashMap`|[`ConcurrentHashMultiset`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/ConcurrentHashMultiset.html)|No|
|`ImmutableMap`|[`ImmutableMultiset`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/ImmutableMultiset.html)|No|

### ==**ClassToInstanceMap**==

Map with classes as key values.

In addition to extending the `Map` interface, `ClassToInstanceMap` provides the methods [`T getInstance(Class<T>)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/ClassToInstanceMap.html#getInstance-java.lang.Class-) and [`T putInstance(Class<T>, T)`](https://guava.dev/releases/snapshot/api/docs/com/google/common/collect/ClassToInstanceMap.html#putInstance-java.lang.Class-java.lang.Object-) , which eliminate the need for unpleasant casting while enforcing type safety.

### ==**RangeSet**==

A `RangeSet` describes a set of _disconnected, nonempty_ ranges. When adding a range to a mutable `RangeSet`, any connected ranges are merged together, and empty ranges are ignored. For example:

```Java
RangeSet<Integer> rangeSet = TreeRangeSet.create();
rangeSet.add(Range.closed(1, 10)); // {[1, 10]}
rangeSet.add(Range.closedOpen(11, 15)); // disconnected range: {[1, 10], [11, 15)}
rangeSet.add(Range.closedOpen(15, 20)); // connected range; {[1, 10], [11, 20)}
rangeSet.add(Range.openClosed(0, 0)); // empty range; {[1, 10], [11, 20)}
rangeSet.remove(Range.open(5, 10)); // splits [1, 10]; {[1, 5], [10, 10], [11, 20)}
```

`RangeSet` implementations support an extremely wide range of views, including:

- `complement()`: views the complement of the `RangeSet`. `complement` is also a `RangeSet`, as it contains disconnected, nonempty ranges.
- `subRangeSet(Range<C>)`: returns a view of the intersection of the `RangeSet` with the specified `Range`. This generalizes the `headSet`, `subSet`, and `tailSet` views of traditional sorted collections.
- `asRanges()`: views the `RangeSet` as a `Set<Range<C>>` which can be iterated over.
- `asSet(DiscreteDomain<C>)` (`ImmutableRangeSet` only): Views the `RangeSet<C>` as an `ImmutableSortedSet<C>`, viewing the elements in the ranges instead of the ranges themselves. (This operation is unsupported if the `DiscreteDomain` and the `RangeSet` are both unbounded above or both unbounded below.)

In addition to operations on its views, `RangeSet` supports several query operations directly, the most prominent of which are:

- `contains(C)`: the most fundamental operation on a `RangeSet`, querying if any range in the `RangeSet` contains the specified element.
- `rangeContaining(C)`: returns the `Range` which encloses the specified element, or `null` if there is none.
- `encloses(Range<C>)`: straightforwardly enough, tests if any `Range` in the `RangeSet` encloses the specified range.
- `span()`: returns the minimal `Range` that `encloses` every range in this `RangeSet`.

### ==**RangeMap**==

`RangeMap` is a collection type describing a mapping from disjoint, nonempty ranges to values. Unlike `RangeSet`, `RangeMap`never "coalesces" adjacent mappings, even if adjacent ranges are mapped to the same values.

```Java
RangeMap<Integer, String> rangeMap = TreeRangeMap.create();
rangeMap.put(Range.closed(1, 10), "foo"); // {[1, 10] => "foo"}
rangeMap.put(Range.open(3, 6), "bar"); // {[1, 3] => "foo", (3, 6) => "bar", [6, 10] => "foo"}
rangeMap.put(Range.open(10, 20), "foo"); // {[1, 3] => "foo", (3, 6) => "bar", [6, 10] => "foo", (10, 20) => "foo"}
rangeMap.remove(Range.closed(5, 11)); // {[1, 3] => "foo", (3, 5) => "bar", (11, 20) => "foo"}
```

==**BloomFilter**==

Bloom filters are a probabilistic data structure, allowing you to test if an object is _definitely_ not in the filter, or was _probably_ added to the Bloom filter. The [Wikipedia page](http://en.wikipedia.org/wiki/Bloom_filter) is fairly comprehensive, and we recommend [this tutorial](http://llimllib.github.io/bloomfilter-tutorial/).

```Java
BloomFilter<Person> friends = BloomFilter.create(personFunnel, 500, 0.01);
for (Person friend : friendsList) {
  friends.put(friend);
}
// much later
if (friends.mightContain(someone)) {
  // the probability that someone reached this place if they aren't a friend is
  // 1% we might, for example, start asynchronously loading things for someone
  // while we do a more expensive exact check
}
```

### ==Utils, for collection creating:==

- ==**Lists**== - utility class for List. It provides a few methods for new List instances creating. For example:
    
    ```Java
    List<String> reversed = Lists.reverse(names);
    List<String> names = Lists.newArrayList("John", "Adam", "Jane");
    ```
    
- ==**Maps**== - utility class for Maps. It provides a few methods for new Map instances creating. Also, Guava has a few variations, like ImmutableMap, SortedMap, Table, e.g. For example:
    
    ```Java
    Map<String, String> aNewMap = Maps.newHashMap();
    Map<String, Integer> salary = ImmutableMap.<String, Integer> builder().put("1", 1000).build();
    Table<String,String,Integer> distance = HashBasedTable.create();
    ```
    
- ==**Sets**== - utility class for List. It provides a few methods for new Set instances creating. For example:
    
    ```Java
    Set<String> aNewSet = Sets.newHashSet();
    ```
    
- ==**Immutable -**== Immutable versions of standard collections. Alternative to Collections.unmodifiable  
      
    

```Java
public static final ImmutableSet<Color> GOOGLE_COLORS =
   ImmutableSet.<Color>builder()
       .addAll(WEBSAFE_COLORS)
       .add(new Color(0, 191, 255))
       .build();
```

### Caching

Guava provides a wide ==**caching functionality**==, you can add rule via CacheLoader and after that, use CacheBuilder with loader. Also note that we're using the _getUnchecked()_ operation, which computes and loads the value into the cache if it doesn't already exist. Moreover, we can add some restrictions (time, size, e.g) and manage or cache.

```Java
@Test
public void whenEntryIdle_thenEviction() throws InterruptedException {
	CacheLoader<String, String> loader;
	loader = new CacheLoader<String, String>() {

	@Override
	public String load(String key) {
		return key.toUpperCase();
	}
};

	LoadingCache<String, String> cache;
	cache = CacheBuilder.newBuilder().build(loader);
	
	assertEquals(0, cache.size());
	assertEquals("HELLO", cache.getUnchecked("hello"));
	assertEquals(1, cache.size());
}
```

### Concurrency

- Guava extends the Future interface of the JDK with ==**ListenableFuture**==.  
    A traditional Future represents the result of an asynchronous computation: a computation that may or may not have finished producing a result yet. A Future can be a handle to an in-progress computation, a promise from a service to supply us with a result.  
    A   
    ==**ListenableFuture**== allows you to register callbacks to be executed once the computation is complete, or if the computation is already complete, immediately. This simple addition makes it possible to efficiently support many operations that the basic Future interface cannot support.
- ==**RateLimiter**== - special class to restrict the rate at which some processing happens. If we create a ==**RateLimiter**== with 1 permits - it means that process can issue at most 1 permits per second.
    
    ```Java
    @Test
    public void givenLimitedResource_whenRequestOnce_thenShouldPermitWithoutBlocking() {
        // given
        RateLimiter rateLimiter = RateLimiter.create(1);
    
        // when
        long startTime = ZonedDateTime.now().getSecond();
        rateLimiter.acquire(1);
        doSomeLimitedOperation();
        long elapsedTimeSeconds = ZonedDateTime.now().getSecond() - startTime;
    
        // then
        assertThat(elapsedTimeSeconds <= 1);
    }
    ```
    

  

### Math

Math provides us a few classes with special mathematical operations:

- ==**IntMath**== - operation on int values.
- ==**LongMath**== - operations on long values.
- ==**BigIntegerMath**== - operations on BigIntegers.
- ==**DoubleMath**== - operations on double values.

```Java
@Test
public void whenFactorialInt_shouldReturnTheResultIfInIntRange() {
    int result = IntMath.factorial(5);
 
    assertEquals(120, result);
}
```

### I/O

Guava provides special classes to simplify work with files:

```Java
@Test
public void whenReadMultipleLinesUsingFiles_thenRead() throws IOException {
    File file = new File("test.txt");
    List<String> result = Files.readLines(file, Charsets.UTF_8);

    assertThat(result, contains("John", "Jane", "Adam", "Tom"));
}
```

Another examples:

- **CountingOutputStream** - byte counter.
- **CharSink** - special class for string writing to files.
    
    ```Java
    CharSink sink = Files.asCharSink(file, Charsets.UTF_8);
    
    sink.writeLines(names, " ");
    ```
    
- **ByteSink** - allows writing bytes to file.
    
    ```Java
    ByteSink sink = Files.asByteSink(file);
    sink.write(expectedValue.getBytes());
    ```
    
- **CharSource/ByteSource**
    
    ```Java
    File file2 = new File("test2.txt");
    CharSource source = Files.asCharSource(file, Charsets.UTF_8);
    String result = source.read();
    ```
    
- **CharStream/ByteStream**
    
    ```Java
    FileReader reader = new FileReader("test.txt");
    String result = CharStreams.toString(reader);
    ```
    
- **Resources -** позволяет читать фалы из classpath
    
    ```Java
    URL url = Resources.getResource("test.txt");
    String result = Resources.toString(url, Charsets.UTF_8);
    ```
    

### Data

==**EventBus**== allows to publish-subscribe communication between components. Subscribers register via special annotations. When some event called, EventBus finds subscribers capable of receiving this type of event and notifies them of the event.

```Java
public class EventListener {
	private static int eventsHandled;

	@Subscribe
	public void stringEvent(String event) {
		eventsHandled++;
	}
}

EventBus eventBus = new EventBus();
EventListener listener = new EventListener();

eventBus.register(listener);
eventBus.post("String Event");
eventBus.unregister(listener);
```

### Reflection

Guava provides simplest way proxy creation:

```Java
Java CoreFoo foo = (Foo) Proxy.newProxyInstance(Foo.class.getClassLoader(), 
new Class<?>[] {Foo.class}, 
invocationHandler);

Foo foo = Reflection.newProxy(Foo.class, invocationHandler)
```

**TypeToken** - Allows to identify generic class type in runtime, to deal with type erasure.

```Java
TypeToken<List<String>> stringListToken = new TypeToken<List<String>>() {};
```

**Invokable -** wrapper of java.lang.reflect.Method and java.lang.reflect.Constructor. It provides a simpler API on top of a standard Java reflection API

```Java
Method method = CustomClass.class.getMethod("somePublicMethod"); //reflection api
Invokable<CustomClass, ?> invokable = new TypeToken<CustomClass>() {}
	.method(method);//guava api

boolean isPublicStandradJava = Modifier.isPublic(method.getModifiers());
boolean isPublicGuava = invokable.isPublic();
```

### Utilities

Useful classes that reduces boilerplate code

**Objects**

Existed before java.util.Objects, have equal and hashCode methods which are deprecated since Java 7, because of appearance of java.util.Objects.

**MoreObjects**

Have `toStringHelper` method which allows to create to custom values toString for debug purposes

```Java
// Returns "MyObject{x=1}"
   MoreObjects.toStringHelper("MyObject")
       .add("x", 1)
       .toString();
```

**ComparasionChain**

Performs lazy comparation in non verbose way

```Java
public int compareTo(Foo that) {
     return ComparisonChain.start()
         .compare(this.aString, that.aString)
         .compare(this.anInt, that.anInt)
         .compare(this.anEnum, that.anEnum, Ordering.natural().nullsLast())
         .result();
   }
```

**Stopwatch**

Class for time measurement  
  

```Java
   Stopwatch stopwatch = Stopwatch.createStarted();
   doSomething();
   stopwatch.stop(); // optional

   long millis = stopwatch.elapsed(MILLISECONDS);

   log.info("time: " + stopwatch); // formatted string like "12.3 ms"
```

**Joiner and Splitter**

Allows to convert collection into a string with Joiner and string to collection with Splitter

```Java
List<String> names = Lists.newArrayList("John", "Jane", "Adam", "Tom");
String result = Joiner.on(",").join(names);

// "John,Jane,Adam,Tom"

Map<String, Integer> salary = Maps.newHashMap();
salary.put("John", 1000);
salary.put("Jane", 1500);
String result = Joiner.on(" , ").withKeyValueSeparator(" = ")
                                    .join(salary);
//"John = 1000" , "Jane = 1500"
```

```Java
String input = "John=first,Adam=second";
Map<String, String> result = Splitter.on(",")
                                         .withKeyValueSeparator("=")
                                         .split(input);
result.get("John"); // first
result.get("Adam"); // second
```

**Preconditions**

This class provides a list of static methods for checking that a method or a constructor is invoked with valid parameter values. If a precondition fails, a tailored exception is thrown.

```Java
Preconditions.checkArgument(age>10, "message");
Preconditions.checkElementIndex(6, numbers.length - 1, message);
Preconditions.checkNotNull(nullObject, message);
Preconditions.checkState(Arrays.binarySearch(validStates, givenState) > 0, message);
```

**CharMatcher**

Offers basic text processing methods

```Java
String input = "H*el.lo,}12日本人中";
CharMatcher matcher = CharMatcher.ascii();
String result = matcher.removeFrom(input); // 日本人中
result = matcher.retainFrom(input); // H*el.lo,}12
```

**Ordering**

Set of configurable ordering definitions for collections

```Java
Collections.sort(toSort, Ordering.natural().nullsFirst());
Collections.sort(toSort, Ordering.natural().reverse());
Ordering<String> byLength = new OrderingByLenght();
Ordering.natural().isStrictlyOrdered(sorted) // check of collection is sorted
```

**ClassPath**

Scans the source of a ClassLoader and finds all loadable classes and resources

**Hashing**

Utility class for hashing

```Java
String sha256hex = Hashing.sha256()
  .hashString(originalString, StandardCharsets.UTF_8)
  .toString();
```