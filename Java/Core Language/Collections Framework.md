## 1. Collections Hierarchy Overview
```
Iterable
└── Collection
    ├── List  (ordered, allows duplicates)
    │   ├── ArrayList
    │   ├── LinkedList
    │   ├── Vector (legacy, synchronized)
    │   └── CopyOnWriteArrayList (concurrent)
    ├── Set  (no duplicates)
    │   ├── HashSet
    │   ├── LinkedHashSet
    │   └── TreeSet (sorted)
    └── Queue / Deque
        ├── PriorityQueue
        ├── ArrayDeque
        ├── LinkedList
        └── BlockingQueue (concurrent)
            ├── ArrayBlockingQueue
            ├── LinkedBlockingQueue
            └── DelayQueue

Map  (key-value pairs, not part of Collection)
├── HashMap
├── LinkedHashMap
├── TreeMap (sorted)
├── Hashtable (legacy, synchronized)
└── ConcurrentHashMap (concurrent)
```

# 2. List implementation
## 2.1. ArrayList
An ArrayList is a resizable array, elements are stored contiguously in memory, each element has an index.
Key characteristics:
- O(1) random access by index
- O(n) insert/remove in the middle
Expansion mechanism: When the internal array is full, the `ArrayList` create a new array 1.5xcurrent size and copies elements over:

| Step            | Size | Capacity |
| --------------- | ---- | -------- |
| Start           | 0    | 0        |
| Add 1st element | 1    | 10       |
> Array List is backed by Array, array store element in contiguous memories locations. Because, each slot has a fixed size, the JVM can compute address of any index by offset calculation.
## 2.2. Linked List
Backed by doubly linked list, 
## 2.4. CopyOnWriteArrayList
Think of `CopyOnWriteArrayList` as a "read-optimized, thread-safe cousin" of the standard `ArrayList`.
In a multi-threaded Java environment, if one thread tries to read a standard `ArrayList` while another thread is modifying it (adding or removing items), Java will instantly throw a `ConcurrentModificationException`.
`CopyOnWriteArrayList` fixes this problem using a very literal strategy: every time you modify the list, it creates a brand new copy of the underlying array.
# 3. Map implementation
## 3.1. HashMap Internals
`HashMap` uses an array of buckets where each bucket is a linked list.
![[Pasted image 20260522195757.png]]
**Core mechanics:**
1. Hash function: Spreads keys across buckets. it depends on `hashCode()` function. Almost Wrapper class use value to hash	```
```java
private int getBucketIndex(K key) {  
    if (key == null) return 0;  
    return Math.abs(key.hashCode() % capacity);  
}
```
2. The Entry Objects are stored in linked list because two different key can produce the same index when calculating `key.hashCode() % capacity` (this is called a collision). When the collision happen the new Entry will linked to the existing one, forming a chain inside a bucket, If an `Entry` with the same key already exists, the hash table simply overrides the old value instead of creating a new node.
3. When linked list exceeds **8 nodes** (and capacity ≥ 64), it converts to a [[Red-black Tree]] for O(log n) lookup
4. When a tree shrinks below **6 nodes**, it converts back to a linked list.

**Load Factor and expansion**
1. Default load factor: 0.75
2. Threshold = capacity * load factor
3. When size > threshold –> the array double capacity and all entries will be rehashed.

**Important** : `hashMap` is not thread safe –> use `ConcurrentHashMap`

## 3.2. TreeMap
Backed by a [[Red-black Tree]]. Entries are sorted by key
## 3.3. ConcurrentHashMap 
Is a thread-safe implementation of the Map interface in Java. It allows multiple threads to read, update, and access data simultaneously without causing data inconsistency or locking the entire map.
> The map was divided into fix number (usually 16) of segments. Each segment is independent. If Thread A wrote to Segment 1 and Thread B wrote to Segment 2, they could run completely in parallel.

## 3.4. HashMap vs HashTable vs ConcurrentHashMap
| Feature     | HashMap              | Hashtable          | ConcurrentHashMap      |
| ----------- | -------------------- | ------------------ | ---------------------- |
| Thread-safe | ❌                    | ✅ (synchronized)   | ✅ (CAS + synchronized) |
| Null keys   | ✅ (one null key)     | ❌                  | ❌                      |
| Performance | Best (single-thread) | Poor (coarse lock) | Best (concurrent)      |
| Recommended | Single-threaded use  | Never (legacy)     | Multi-threaded use     |
# 9. Common Pitfalls in Interview
## 9.1. equals() without hashCode()
In Java’s contract, two object are equal according to `equals()` method will return same `hashCode`. If only `equals()`  –> two objects are consider equal but have different hash code. This can cause unexpected behavior in hash based collections.

# Interview Questions
## What’s Collection Frameworks in Java
The **Java Collections Framework** is a set of interfaces and classes that provide standardized ways to store, manage, and manipulate groups of objects. It includes common data structures such as `List`, `Set`, `Queue`, and `Map`, along with implementations like `ArrayList`, `LinkedList`, `HashSet`, and `HashMap`. The framework offers reusable algorithms, improves code consistency, and helps developers choose the most suitable data structure for a specific use case. It is widely used in Java applications because it simplifies data handling and improves productivity.
## ArrayList vs LinkedList
`ArrayList` and `LinkedList` are both implementations of the `List` interface, but they use different internal data structures. **ArrayList** is backed by a dynamic array, providing fast random access with O(1) time complexity, while **LinkedList** is based on a doubly linked list, making insertions and deletions more efficient when modifying elements in the middle of the list. However, LinkedList has slower access time because it must traverse nodes sequentially. In practice, I usually choose `ArrayList` because read operations are more common and it generally offers better overall performance.
## HashSet vs TreeSet
`HashSet` and `TreeSet` are both implementations of the `Set` interface that store unique elements, but they differ in ordering and performance. **HashSet** uses a hash table internally and does not guarantee any ordering of elements, providing average O(1) time complexity for add, remove, and contains operations. **TreeSet** uses a Red-Black Tree and automatically stores elements in sorted order, but its operations have O(log n) time complexity. I typically use `HashSet` when fast lookups are important and `TreeSet` when I need the elements to remain sorted.
## HashMap vs TreeMap
 `HashMap` and `TreeMap` are both implementations of the `Map` interface used to store key-value pairs, but they differ in how they organize data. **HashMap** uses a hash table internally and does not guarantee any ordering of keys, providing average O(1) performance for put, get, and remove operations. **TreeMap** uses a Red-Black Tree and automatically stores keys in sorted order, with O(log n) time complexity for its operations. I typically use `HashMap` for fast lookups and `TreeMap` when I need the keys to be sorted or perform range-based operations.
## HashSet vs HashMap
`HashSet` and `HashMap` are both based on hash tables and provide average O(1) performance for basic operations, but they serve different purposes. **HashSet** stores only unique values, while **HashMap** stores key-value pairs where each key is unique and maps to a value. Internally, `HashSet` is actually implemented using a `HashMap`, where each element in the set is stored as a key and a dummy object is used as the value. I use `HashSet` when I only need uniqueness, and `HashMap` when I need to associate data with a key.
## Comparable and Comparator
Both **Comparable** and **Comparator** are used for sorting objects in Java, but they differ in where the sorting logic is defined. **Comparable** defines the natural ordering of a class by implementing the `compareTo()` method inside the class itself. **Comparator** defines custom sorting logic in a separate class or lambda expression using the `compare()` method, allowing multiple sorting strategies. I use Comparable when there is a single natural ordering, and Comparator when I need different ways to sort the same object.