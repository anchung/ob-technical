---
{"dg-publish":true,"permalink":"/books/2024/java-in-a-nutshell/chapter-6-java-s-approach-to-memory-and-concurrency/","title":"Chapter 6 - Java's Approach to Memory and Concurrency","tags":["book","java"]}
---

> [!summary] Topics
> 
- Intro to Java's memory management
- Basic **mark-and-sweep** GC algorithm
- How the Hotspot JVM optimizes GC according to the lifetime of the object
- Java's concurrency primitives
- Data visibility and mutability

## Memory Leak
- Java Objects are created in the heap, and a reference to them is stored in a Java local variable (field) when an object is created
- Local variable live in the method's stack frame
- GC cannot clean up the objects as long as the object is still being referenced somewhere.

## Heap Structure
![Pasted image 20241004140408.png|640](/img/user/ASSETS/Pasted%20image%2020241004140408.png)
### Weak generational Hypothesis
![Pasted image 20241004140801.png](/img/user/ASSETS/Pasted%20image%2020241004140801.png)

- Heap is divided into separate memory spaces based on the WGH above.
- New objects are created in the space called **Eden**.
- On each GC run, we locate only the live objects and move them to a different space (*evacuation*)
- Short-lived objects are usually collected by the **Evacuating Collector**
- Long-lived objects are usually collected by the **Compacting Collector**

## HotSpot JVM
The HotSpot JVM is a relatively complex piece of code, made up an interpreter, JIT compiler, and a user-space memory management sub-system. It's composed of a mixure of C, C++, and a fairly large amount of platform-specific assembly code.

HotSpot manages the JVM heap itself, more-or-less completely in user space; does NOT need to perform system calls to allocate or free memory.

- The *Java Heap* is a contiguous block of memory, which is reserved at JVM startup.
- As the application runs, memory pools are resized as needed
- The heap is divided into two generations: young and old.
- *Young generation* is made up of **Eden** and **Survivor** spaces. *Old generation* is just one memory space (**tenured**)

> JVM provides a command-line switch to control how long the collector will aim to pause: `-XX:MaxGCPauseMillis=200`

## Finalization
Finalization has been deprecated and will be removed in a future release.

## Concurrency
Runtime-managed concurrency will be discussed briefly in [[Chapter 8\|Chapter 8]]
This chapter is focusing on the low-level concurrency mechanisms that the Java platform provides.

### Thread Lifecycle
Java tries hard to abstract the operating system's view of threads details away and has an enum called `Thread.State`. It provides an overview of the lifecycle of a thread
- NEW
  The thread has been created, but its `start()` method has not yet been called
  
- RUNNABLE
  The thread is running or is available to run when the operating system schedules it
  
- BLOCKED
  The thread is not running because it is waiting to acquire a lock so it can enter a `synchronized` method or block.
  
- WAITING
  The thread is not running because it has called `Object.wait()` or `Thread.join()`
  
- TIMED_WAITING
  The thread is NOT running because it has called `Thread.sleep()` or has called `Object.wait()` or `Thread.join()` with a timeout value.
  
- TERMINATED
  The thread has completed execution. Its `run()` method has exited normally or by throwing an exception.

![Pasted image 20241004144838.png](/img/user/ASSETS/Pasted%20image%2020241004144838.png)

### Exclusion and Protecting State
Any code that *modifies*, or *reads* state that can become inconsistent must be protected. To achieve this, the Java platform provides only one mechanism: **exclusion**

`synchronized` keyword is used for a thread to acquire a *monitor/lock* of the object first in order to execute a synchronized method or block.

