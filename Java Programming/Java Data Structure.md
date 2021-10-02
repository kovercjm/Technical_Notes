# ArrayList & LinkedList

- ArrayList
  - based on array
  - resize to 1.5x volume when full
  - move other elements when insert / delete
- LinkedList
  - need traverse when insert / delete

# HashMap

![img](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/e4a19398.png)

By default, HashMap has a capacity of 16 and load factor of 0.75. When reaching the threshold, HashMap will resize itself, which costs lots of resources and time.
As threshold is `loadFactor * capacity`, so it's recommended to use the following formula to set the default capacity for better performance.
> initialCapacity = expectedSize / 0.75 + 1

Java HashMap is storing data by arrays and linked list. The index of array is calculated by hash functions, and when two elements are to be stored in the same index of array, the new one will be at the head of linked list and points to the old one.

When a linked list has more than 8 nodes, the list will be transferred into a red-black tree.

HashMap is not thread-safe, and the thread-safe version can use ConcurrentHashMap.

In JDK 1.7, when concurrently used HashMap, can cause dead loop by `get()`, because when resizing (`HashMap#transfer()`) two threads can be creating the same thread at the same time and then caused a loop in linked list.

In JDK 1.8, the above bug is fixed, but when concurrently `put()` in a HashMap, the `HashMap#putVal()` may let one input re-write another thread's input.

# ConcorrentHashMap

Using CAS for creating new linked list and synchronized for modifying a linked list or Red-Black tree.

# CopyOnWriteArrayList

Concorrent read on old list; copy a list for modify and point to the new one if finished modification.

Problem: can cost lots of memory and can only maintain Eventual Consistency but not Real-time Consistency.

