---
layout: post
title: Multithreads in Ruby Chap 1
---
# Process & Thread
- Process
  - ## Pro
  - Don't share memory => Can't mutate data from one process in another.
  - Much easier to code and debug
  - Forking child process may help avoid unwanted memory leaks. Once the process finishes, it releases all the resource.
  - ## Cons
  - Since process don't share memory, they use a lot of memory.
  - From ruby 2.0  `fork` uses OS **Copy-On-Write** which allow processes to share memory as long as it doesn't have different values.
  - Process are slow to create and destroy.
  - ## Examples:
  - `Unicorn` server - it loads the applications, forks the master process to spawn multiple worker which accept HTTP request.
  - `Resque` for background processing, it runs a worker, which executes each job sequentially in forked child process.
- Thread
  - Only one thread can be executing at any given time within a single process.
  - GIL:
    - Avoids race conditions with C extensions, no need to worry about thread-safety.
    - Easier to implement, no need to make Ruby data structure thread-safe
    - GIL and blocking I/O
      - Thread releases GIL when it hits blocking I/O operations such as HTTP requests, DB queries, writing/reading from disk and even `sleep`
  - ## Pro
  - Uses less memory than process, it's possible to run thoundands of threads. 
  - They are also fast to create and destroy
  - Threads are useful when there are slow blocking I/O opearations.
  - Can acess the memory area from other threads if necessary.
  - ## Cons
  - Requires very careful synchronization to avoid race-conditions, usually by using locking primitives, which sometimes may lead to deadlocks.
  - All this makes it quire difficult to write, test and debug thread-safe code.
  - The more threads you spawn, the more time and resources they'll be spending by switching the context and spending lesstime doing the actual job.


