# HashMap

![img](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/e4a19398.png)

By default, HashMap has a capacity of 16 and load factor of 0.75. When reaching the threshold, HashMap will resize itself, which costs lots of resources and time.
As threshold is `loadFactor * capacity`, so it's recommended to use the following formula to set the default capacity for better performance.
> initialCapacity = expectedSize / 0.75 + 1

Java HashMap is storing data by arrays and linked list. The index of array is calculated by hash functions, and when two elements are to be stored in the same index of array, the new one will be at the head of linked list and points to the old one.

When a linked list has more than 8 nodes, the list will be transferred into a red-black tree.

HashMap is not thread-safe, and the thread-safe version can use ConcurrentHashMap. When concurrently used HashMap, can cause dead loop by `get()`, because when resizing two threads can be creating the same thread at the same time and then caused a loop in linked list.
