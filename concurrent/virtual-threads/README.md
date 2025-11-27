# Virtual Threads (Project Loom)

## Overview

Virtual threads are a lightweight implementation of threads introduced in Java 19 as a preview feature and finalized in Java 21. They are part of Project Loom and dramatically change how we write concurrent applications in Java.

## What are Virtual Threads?

Virtual threads are threads that are managed by the JVM rather than the operating system. They are:
- **Lightweight**: You can create millions of them without overwhelming system resources
- **Cheap to create and destroy**: Unlike platform threads
- **Suitable for high-throughput concurrent applications**: Especially I/O-bound workloads
- **Easy to use**: Same Thread API as platform threads

## Key Concepts

### Platform Threads vs Virtual Threads

**Platform Threads (Traditional)**:
- Mapped 1:1 to OS threads
- Expensive to create (typically limited to thousands)
- Heavyweight (each requires significant memory)
- Good for CPU-intensive tasks

**Virtual Threads**:
- Many-to-many mapping to OS threads
- Can create millions easily
- Lightweight (minimal memory overhead)
- Excellent for I/O-intensive tasks

### How Virtual Threads Work

Virtual threads are scheduled by the JVM on a pool of platform threads (carrier threads). When a virtual thread blocks on I/O, it is unmounted from its carrier thread, allowing another virtual thread to run.

## Creating Virtual Threads

```java
// Method 1: Using Thread.ofVirtual()
Thread vThread = Thread.ofVirtual().start(() -> {
    System.out.println("Hello from virtual thread!");
});

// Method 2: Using builder
Thread vThread2 = Thread.ofVirtual()
    .name("my-virtual-thread")
    .start(() -> {
        // task
    });

// Method 3: Using ExecutorService
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    executor.submit(() -> {
        // task
    });
}
```

## Topics to Cover

1. **Basics**
   - Creating virtual threads
   - Lifecycle and scheduling
   - Carrier threads and mounting/unmounting

2. **Best Practices**
   - When to use virtual threads vs platform threads
   - Avoiding thread locals with virtual threads
   - Pinning issues and how to avoid them

3. **Structured Concurrency**
   - StructuredTaskScope
   - Managing related tasks together
   - Error handling and cancellation

4. **Performance**
   - Benchmarking virtual threads
   - Scalability comparisons
   - Memory usage patterns

5. **Migration Guide**
   - Converting from platform threads
   - Converting from thread pools
   - Handling existing blocking APIs

6. **Common Pitfalls**
   - Synchronized blocks (pinning)
   - Thread locals
   - ThreadPool anti-patterns with virtual threads

## Use Cases

- High-throughput web servers
- Database connection handling
- Microservices with many I/O operations
- Concurrent request processing
- File I/O intensive applications

## Requirements

- **Preview**: Java 19, 20 (with `--enable-preview`)
- **Stable**: Java 21+

## Examples Structure

Each example will demonstrate:
- Basic usage
- Performance comparisons
- Real-world scenarios
- Best practices

---

*This folder will contain runnable examples and detailed explanations for virtual threads.*
