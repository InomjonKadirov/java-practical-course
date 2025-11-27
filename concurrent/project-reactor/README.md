# Project Reactor

## Overview

Project Reactor is a fully non-blocking reactive programming foundation for the JVM, with efficient demand management (back pressure). It is the foundation of the Spring reactive stack and implements the Reactive Streams specification.

## What is Project Reactor?

Project Reactor provides:
- Rich set of operators for asynchronous stream processing
- Two main publisher types: Mono and Flux
- Extensive error handling capabilities
- Schedulers for controlling execution context
- Integration with Spring Framework (especially WebFlux)

## Core Components

### Mono<T>
Represents a stream of 0 or 1 element.

```java
Mono<String> mono = Mono.just("Hello");
Mono<String> empty = Mono.empty();
Mono<String> error = Mono.error(new RuntimeException("Error"));
```

### Flux<T>
Represents a stream of 0 to N elements.

```java
Flux<Integer> flux = Flux.just(1, 2, 3, 4, 5);
Flux<Integer> range = Flux.range(1, 100);
Flux<Long> interval = Flux.interval(Duration.ofSeconds(1));
```

## Key Concepts

### Reactive Streams
- Implements Publisher, Subscriber, Subscription interfaces
- Full support for back pressure
- Non-blocking I/O operations

### Operators
Rich set of operators for:
- Transformation (map, flatMap, transform)
- Filtering (filter, take, skip)
- Combining (merge, zip, concat)
- Error handling (onErrorReturn, onErrorResume, retry)
- Time-based operations (timeout, delay, buffer)

### Schedulers
Control execution context:
- `Schedulers.immediate()`: Current thread
- `Schedulers.single()`: Single reusable thread
- `Schedulers.parallel()`: Fixed pool for CPU-bound work
- `Schedulers.boundedElastic()`: Elastic pool for I/O-bound work

### Hot vs Cold Publishers
- **Cold**: Creates new data for each subscriber (like a movie on-demand)
- **Hot**: Shares data among subscribers (like a live broadcast)

## Topics to Cover

1. **Fundamentals**
   - Mono and Flux basics
   - Creating publishers
   - Subscribing to streams
   - Understanding operators
   - Lazy evaluation

2. **Transformation Operators**
   - map, flatMap, flatMapSequential
   - transform, transformDeferred
   - cast, ofType
   - switchMap

3. **Filtering Operators**
   - filter, filterWhen
   - take, takeLast, takeUntil, takeWhile
   - skip, skipLast, skipUntil, skipWhile
   - distinct, distinctUntilChanged

4. **Combining Operators**
   - merge, mergeSequential, mergeWith
   - zip, zipWith
   - concat, concatWith
   - combineLatest

5. **Error Handling**
   - onErrorReturn, onErrorResume
   - onErrorMap, onErrorContinue
   - retry, retryWhen
   - timeout
   - doOnError

6. **Back Pressure**
   - request(n) strategy
   - onBackpressureBuffer
   - onBackpressureDrop
   - onBackpressureLatest
   - onBackpressureError

7. **Schedulers and Threading**
   - publishOn vs subscribeOn
   - Different scheduler types
   - Parallel processing with parallel()
   - Custom schedulers

8. **Testing**
   - StepVerifier
   - TestPublisher
   - Virtual time
   - Assertions

9. **Context**
   - Context propagation
   - ThreadLocal alternatives
   - Context manipulation

10. **Advanced Topics**
    - Custom operators
    - Sinks (programmatic publishers)
    - Processors
    - ConnectableFlux
    - Performance tuning

## Use Cases

- Reactive web applications (Spring WebFlux)
- Microservices communication
- Real-time data streaming
- Event-driven systems
- Asynchronous database access (R2DBC)
- Message-driven applications
- High-throughput systems

## Maven Dependency

```xml
<dependency>
    <groupId>io.projectreactor</groupId>
    <artifactId>reactor-core</artifactId>
    <version>3.6.0</version>
</dependency>

<!-- For testing -->
<dependency>
    <groupId>io.projectreactor</groupId>
    <artifactId>reactor-test</artifactId>
    <version>3.6.0</version>
    <scope>test</scope>
</dependency>
```

## Gradle Dependency

```gradle
implementation 'io.projectreactor:reactor-core:3.6.0'
testImplementation 'io.projectreactor:reactor-test:3.6.0'
```

## Key Differences from RxJava

| Feature | Project Reactor | RxJava |
|---------|----------------|---------|
| Primary Use | Spring ecosystem | Android, general JVM |
| Publishers | Mono, Flux | Single, Maybe, Observable, Flowable |
| Context | Built-in Context | No built-in context |
| Spring Integration | Native | Via adapters |
| Java Version | Java 8+ | Java 6+ (RxJava 2), Java 8+ (RxJava 3) |

## Best Practices

1. Prefer Mono for 0-1 elements
2. Use appropriate schedulers for workload type
3. Handle errors explicitly
4. Avoid blocking operations
5. Use StepVerifier for testing
6. Understand operator fusion
7. Be mindful of memory with buffering operators
8. Use Context instead of ThreadLocal

## Learning Path

1. Start with simple Mono/Flux creation
2. Learn basic operators (map, filter)
3. Understand subscription and execution
4. Master error handling
5. Learn schedulers and threading
6. Practice with real-world examples
7. Deep dive into advanced operators
8. Performance optimization

## Examples Structure

Examples will include:
- Basic Mono and Flux operations
- Operator demonstrations
- Error handling patterns
- Scheduler usage
- Real-world applications
- Performance comparisons
- Testing examples

---

*This folder will contain comprehensive Project Reactor examples with runnable code and detailed explanations.*
