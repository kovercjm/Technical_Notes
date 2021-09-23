# Basic concept

![image](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/32.1ceoajhcb2oemkl.png)

Node.js is a runtime environment based on Chrome's V8 engine. Event-driven and non-blocking I/O model makes Node.js efficient and lightweight.

# Event-driven

![2019043007.png](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/Fupjtdwfwem3H45Ta1Q0ulVGOFHR.png)

- Each Node.js process has only one main thread processing the program, maintaining an execution context stack;
- When a outer request or I/O execution is arrived, it will be added to Event Queue, which is single-threaded non-blocking I/O task process;
- The tasks will be sent to C++ libuv thread pool and trigger a callback when finished;
- The response or callback will be sent to client or main thread.

## Event loop order

``` text
   ┌───────────────────────────┐
┌─>│           timers          │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     pending callbacks     │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │       idle, prepare       │
│  └─────────────┬─────────────┘      ┌───────────────┐
│  ┌─────────────┴─────────────┐      │   incoming:   │
│  │           poll            │<─────┤  connections, │
│  └─────────────┬─────────────┘      │   data, etc.  │
│  ┌─────────────┴─────────────┐      └───────────────┘
│  │           check           │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
└──┤      close callbacks      │
   └───────────────────────────┘
```

- **timers**: this phase executes callbacks scheduled by `setTimeout()` and `setInterval()`.
- **pending callbacks**: executes I/O callbacks deferred to the next loop iteration.
- **idle, prepare**: only used internally.
- **poll**: retrieve new I/O events; execute I/O related callbacks (almost all with the exception of close callbacks, the ones scheduled by timers, and `setImmediate()`); node will block here when appropriate.
- **check**: `setImmediate()` callbacks are invoked here.
- **close callbacks**: some close callbacks, e.g. `socket.on('close', ...)`.

## Example

``` javascript
setTimeout(()=>{
    console.log('setTimeout')
},0)
const p = new Promise((resolve)=>{
     console.log('Promise')
     resolve()
})
p.then(()=>{
    console.log('Promise callback')
})
process.nextTick(()=>{
    console.log('nextTick')
})
console.log('end of code')
```

Output will be:

> Promise -> end of code -> nextTick -> Promise callback -> setTimeout

## More on event-loop

> https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/

# Multi-thread

One and only main thread makes Node.js don't need context switching. But to make full use of multi-core CPUs, worker_thread library can create threads for computing jobs.