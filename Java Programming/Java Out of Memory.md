# Common situation of Java OOM

- java.lang.OutOfMemoryError: Java heap space

  - JVM has less memory than a new created object needs
  - Soluton: adjust `-Xms` and `-Xmx`
- java.lang.OutOfMemoryError: ?

  - Object created but never used and cannot be GC
  - Solution: rewrite `hashCode()` function as well as `equals()` function
- java.lang.OutOfMemoryError: GC overhead limit exceeded

  - GC time over 98% and reclaim lower than 2% memory
  - Solution: adjust `-XX:SurvivorRatio` and `-XX:NewRatio`
- java.lang.StackOverflowError
  - Too deep recursion
  - Solution: adjust `-Xss` or reduce recursion


## Overwrite equals() and hashCode()

When see two objects with same value as 'equal object', can overwrite `equals()` function to do so.

But, when a Set or a Map to check if two objects are 'equal', it checks `hashCode()` function first, and then `equals()` function.

So, it often needs to overwrite `hashCode()` as well as `equals()` function.
