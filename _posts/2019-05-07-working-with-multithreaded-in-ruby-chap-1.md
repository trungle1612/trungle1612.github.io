---
layout: post
title: Working with multirheaded in Ruby Chap 1
---
#  In this chap 1 I will explain some terms when working with `Concurrency` in `Ruby`
### [ 1. Process ](#process)
## [ 2. Thread ](#thread)
## [ 3. GIL ](#gil)
## [ 4. Concurrency ](#concurrency)
## [ 5. Parallelism ](#parallelism)
## [ 5. Context Switch ](#context-switch)

---
# Process
  - #### Definition

      - In computing, a process is the `instance` of a `computer program` that is being executed by one or many threads. It's contains the program code and its activity. ([Wiki](<https://en.wikipedia.org/wiki/Process_(computing)>))

  - #### Pro
    - Don't share memory => Can't mutate data from one process in another.
    - Much easier to code and debug
    - Forking child process may help avoid unwanted memory leaks. Once the process finishes, it releases all the resource.
    
  - #### Cons
    - Since process don't share memory, they use a lot of memory.
    - Process are slow to create and destroy.

---
# Thread
  - #### Definition

      - A Thread of execution is the smallest sequence of programmed instruction that can be managed independently by a scheduler, which is typically a part of the operating system. ([Wiki](<https://en.wikipedia.org/wiki/Thread_(computing)>))

  - #### Pro
    - Uses less memory than process, it's possible to run thousand of threads. 
    - They are also fast to create and destroy
    - Threads are useful when there are slow blocking I/O operations.
    - Can acess the memory area from other threads if necessary.
    
  - #### Cons
    - Requires very careful synchronization to avoid race-conditions, usually by using locking primitives, which sometimes may lead to deadlocks.
    - All this makes it quire difficult to write, test and debug thread-safe code.
    - The more threads you spawn, the more time and resources they'll be spending by **switching the context** and spending less time doing the actual job.

---
# GIL(Global Interpreter Lock)
  - A **global interpreter lock** (**GIL**) is a mechanism used in computer-language interpreters to synchronize the execution of threads so that only one native thread can execute at a time. An interpreter that uses GIL always allows exactly one thread to execute at a time, even if run on a multi-core processor. Some popular interpreters that have GIL are CPython and Ruby MRI ([Wiki](<https://en.wikipedia.org/wiki/Global_interpreter_lock>)).

---
# Concurrency
  - Progarmming as the compostion of independently executing process
  - Concurrency is about dealing with lots of thing at once

---
# Parallelism
  - Progarmming as the simultaneous executions of (possibly related) computations.
  - Parallelism is about doing with lots of thing at once

---
# Context Switch
  - When the operating system pauses the execution of one thread to resume execution of another it's called `context switch`

---
# NOTE
  - Only one `unit` of Ruby code can execute at any given time.
  - Ruby is a `Shared memory` language
  - All available are references

