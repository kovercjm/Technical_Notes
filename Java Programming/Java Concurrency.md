# Concurrency Lock

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

# Lock to handle Blocking

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

# Java concurrency problems

- Atomicity - one procedure cannot be interrupted by other thread
  - Only assignment in Java is atomicial procedure
  - synchronized block can make sure atomicity
- Visibility - one thread make changes that can be visible to other immediately
  - sychronized, volatile and final can make sure visibility
- Ordering - procedure between threads don't effected by memory re-ording
  - synchronized and volatile can make sure ordering
  - Happens-Before relationship:
    - Single thread rule: Each action in a single thread happens-before every action in that thread that comes later in the program order.
    - Monitor lock rule: An unlock on a monitor lock (exiting synchronized method/block) happens-before every subsequent acquiring on the same monitor lock.
    - Volatile variable rule: A write to a volatile field happens-before every subsequent read of that same field. Writes and reads of volatile fields have similar memory consistency effects as entering and exiting monitors (synchronized block around reads and writes), but without actually aquiring monitors/locks.
    - Thread start rule: A call to Thread.start() on a thread happens-before every action in the started thread. Say thread A spawns a new thread B by calling threadA.start(). All actions performed in thread B's run method will see thread A's calling threadA.start() method and before that (only in thread A) happened before them.
    - Thread join rule: All actions in a thread happen-before any other thread successfully returns from a join on that thread. Say thread A spawns a new thread B by calling threadA.start() then calls threadA.join(). Thread A will wait at join() call until thread B's run method finishes. After join method returns, all subsequent actions in thread A will see all actions performed in thread B's run method happened before them.
    - Transitivity: If A happens-before B, and B happens-before C, then A happens-before C.
