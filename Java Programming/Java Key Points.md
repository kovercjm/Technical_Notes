# Overwrite equals() and hashCode()

When see two objects with same value as 'equal object', can overwrite `equals()` function to do so.

But, when a Set or a Map to check if two objects are 'equal', it checks `hashCode()` function first, and then `equals()` function.

So, it often needs to overwrite `hashCode()` as well as `equals()` function.

# final

- `final` class cannot be extended
- `final` function can be extended, but cannot be overwrite
- `final` parameter must be given an initial value, and will be stored in JVM method area
