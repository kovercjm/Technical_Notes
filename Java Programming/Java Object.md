# Common Functions

- getClass()
- hashCode()
- equals()
- clone() -> clone with address only
- toString()
- notify() -> awake thread on it?
- notifyAll()
- wait() -> sync lock
- finalize() -> called when GC-ed

# Overwrite

## Overwrite equals() and hashCode()

When see two objects with same value as 'equal object', can overwrite `equals()` function to do so.

But, when a Set or a Map to check if two objects are 'equal', it checks `hashCode()` function first, and then `equals()` function.

So, it often needs to overwrite `hashCode()` as well as `equals()` function.
