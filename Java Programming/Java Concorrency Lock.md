# Lock Character

- Fair / Unfair Lock:
  - Multiple threads acquired a lock according (or not) to the order in which they applied
  - Unfair lock has larger throughput than fair lock
- Reentrant / Non-reentrant (recursive / non-recursive) Lock: 
  - Can (or not) entry same resource if locked by same object
  - Reentrant lock can avoid deadlock in some way
- Shared / Exclusive (read / write) (S / X) Lock:
  - Exclusive lock can only be acquired by one thread
- Pessimistic / Optimistic Lock:
  - Assume data will (or not) be changed when processed
  - Optimistic Lock can only lock one element
    - Compare and Set (CAS): compare with old value, set if is the same; re-calculate if not the same
      - ABA Problem: cannot detect change like A->B->A
      - High CPU cost when lots of conflict
    - Version Control: comare with version

# Handle Blocking

- Spinlock: repeatedly checking til available
  - CAS, costs more CPU time and less switching between threads
- Mutual exclusion (Mutex): awake til available
  - thread will hold and to be notified
- Condition Variable
  - a waiting queue of threads will be notified when condition is changed

# JVM handle synchronized

- Unlocked - without synchronized
- Biased Locking - only acquired by one thread
- Lightweight Locking - only acquired by two thread, will spinning to acquire
- Adaptive Spinning - adaptive control of CAS times before upgraded to heavyweight locking; default 10 times
- Heavyweight Locking - blocking all other threads

# Common Lock Comparison

| synchronized | ReentrantLock           | ReentrantReadWriteLock  |
| ------------ | ----------------------- | ----------------------- |
| Unfair       | Unfair (default) / Fair | Unfair (default) / Fair |
| Reentrant    | Reentrant               | Reentrant               |
| Exclusive    | Exclusive               | Read - S / Write - X    |
| Pessimistic  | Pessimistic             | Pessimistic             |
