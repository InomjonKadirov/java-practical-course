# Concurrent Programming in Java

## Overview

This folder contains practical information, detailed explanations, runnable code examples, and comprehensive data related to concurrent programming in Java. The content is designed to provide hands-on learning experience with Java's concurrency features.

## Purpose

Concurrent programming is essential for building high-performance applications that can handle multiple tasks simultaneously. This collection aims to:

- Provide clear explanations of concurrency concepts
- Demonstrate practical implementations
- Offer runnable code examples
- Share best practices and common pitfalls
- Include exercises and real-world scenarios

## Topics Covered

### 1. Fundamentals
- Threads and the Thread class
- Runnable interface
- Thread lifecycle and states
- Thread synchronization
- Race conditions and critical sections

### 2. Synchronization Mechanisms
- synchronized keyword
- Locks and ReentrantLock
- ReadWriteLock
- Semaphores
- CountDownLatch and CyclicBarrier
- Phaser

### 3. Concurrent Collections
- ConcurrentHashMap
- CopyOnWriteArrayList
- BlockingQueue implementations
- ConcurrentLinkedQueue
- Thread-safe collections overview

### 4. Executor Framework
- Executor and ExecutorService
- ThreadPoolExecutor
- ScheduledExecutorService
- Fork/Join framework
- CompletableFuture

### 5. Advanced Topics
- Atomic variables (AtomicInteger, AtomicLong, etc.)
- Volatile keyword
- Thread-local variables
- Memory model and happens-before relationship
- Deadlocks, livelocks, and starvation
- Performance considerations

### 6. Modern Concurrency (Java 8+)
- Parallel Streams
- CompletableFuture and asynchronous programming
- Virtual Threads (Project Loom - Java 19+)
- Structured Concurrency

## Specialized Topics

This folder contains dedicated subfolders for advanced concurrent and reactive programming topics:

### [Virtual Threads](./virtual-threads/)
Project Loom's lightweight threads introduced in Java 19 (preview) and finalized in Java 21. Learn how to create millions of threads efficiently for high-throughput I/O-bound applications.

### [Flow API](./flow-api/)
Java's built-in reactive streams API (java.util.concurrent.Flow) introduced in Java 9. Implements the Reactive Streams specification with Publisher, Subscriber, Subscription, and Processor interfaces.

### [Project Reactor](./project-reactor/)
Fully non-blocking reactive programming foundation with Mono and Flux. The reactive library powering Spring WebFlux and the Spring reactive stack.

### [Spring WebFlux](./webflux/)
Spring's reactive web framework built on Project Reactor. Learn to build non-blocking, reactive REST APIs and web applications with high concurrency.

### [Mutiny](./mutiny/)
Modern reactive programming library designed for Quarkus. Features an intuitive event-driven API with Uni and Multi for building resilient applications.

### [RxJava](./rxjava/)
Reactive Extensions for Java with extensive operators and scheduler support. Widely used in Android development and general Java applications.

Each subfolder contains:
- Comprehensive README with concepts and examples
- Runnable code samples
- Best practices and common pitfalls
- Integration guides and real-world use cases

## Structure

Each topic will be organized with:
- **Explanation**: Theoretical background and concepts
- **Examples**: Working code samples
- **Exercises**: Practice problems with solutions
- **Best Practices**: Tips and recommendations
- **Common Mistakes**: Pitfalls to avoid

## Getting Started

All examples are designed to be:
- **Runnable**: Can be compiled and executed directly
- **Well-commented**: Clear explanations in code
- **Progressive**: Building from simple to complex
- **Practical**: Real-world applicable scenarios

## Contributing

When adding new content:
1. Ensure code is properly tested
2. Include clear documentation
3. Provide context and use cases
4. Follow Java naming conventions
5. Add meaningful comments

## Resources

Additional learning materials and references will be added as the content grows.

---

**Note**: This is a living document that will be updated as new content is added to this folder.
