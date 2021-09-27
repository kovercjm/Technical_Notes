# Defination

- Process
  - a instance of computer program
  - a container of threads
- Thread
  - the smallest sequence of programmed instructions that can be managed by OS
  - shared resources with other threads in same process
- Coroutine
  - a computer program components that allows execution to be suspended and resumed
  - Multi-coroutine in one thread can only make use of one core of CPU

# Comparison

| Process      | Thread                  | Coroutine                |
| ------------ | ----------------------- | ------------------------ |
| System level | System level            | Language level           |
| Synchronized | Synchronized            | Asynchronized            |
|              | Preemptive multitasking | Cooperative multitasking |

# Multitask

- When several threads run concurrently, the OS will schedule the context switching automatically. It may involve mutex to control the procedure or avoid deadlock.
- As coroutine is cooperative, it will yield itself for other coroutine to process actively.

# Concurrency Control

- Optimistic Lock: (can only protect one value or table)
  - Compare and Set (CAS): compare with old value, set if is the same; re-calculate if not the same
    - ABA Problem: cannot detect change like A->B->A
    - High CPU cost when lots of conflict
  - Version Control: comare with version
- Pessimistic Lock: 
  - Spinlock: repeatedly checking for available lock
  - Mutual exclusion (Mutex): awake for available lock 
  - Condition Variable: check only when condition changed
  - Readers-writer Lock: allow multi reader and lock if has one writer

# Thread Kinds

- User Thread - invisiable to system kernel
  - Pro: Program controls the switch, it is simple;
  - Pro: Swtiching will cost less;
  - Con: Will be blocking the process when one thread is blocked;
  - Con: Cannot make full use of multi-core of CPUs.
- KSE (Kernel Scheduling Entity)
  - Pro: Switch controls by kernel, takes advantage of multi-core;
  - Pro: Will not block the process when one thread is blocked;
  - Con: Switching will cost more;
  - Con: Kernel threads are limited.

# Thread Model

- M:1 - M user thread to 1 KSE - Python
  - Easy to control threads, fast in switching;
  - One thread will block others;
  - Controlled by program
- 1:1 - 1 user thread to 1 KSE - Java
  - Threads will not block others;
  - Switching will cost much;
  - As kernel threads are limited, lots of threads will affect performance
- M:N - M user thead to N KSE - Go

