# Java Flow API (Reactive Streams)

## Overview

The Flow API was introduced in Java 9 as part of the `java.util.concurrent.Flow` class. It provides a standardized way to handle asynchronous stream processing with non-blocking back pressure, implementing the Reactive Streams specification.

## What is the Flow API?

The Flow API is Java's built-in support for reactive programming. It defines a set of interfaces for:
- Publishing items asynchronously
- Subscribing to item streams
- Managing back pressure
- Processing items reactively

## Core Interfaces

### 1. Publisher<T>
Produces items and sends them to subscribers.

```java
public interface Publisher<T> {
    void subscribe(Subscriber<? super T> subscriber);
}
```

### 2. Subscriber<T>
Receives and processes items from a publisher.

```java
public interface Subscriber<T> {
    void onSubscribe(Subscription subscription);
    void onNext(T item);
    void onError(Throwable throwable);
    void onComplete();
}
```

### 3. Subscription
Represents a connection between Publisher and Subscriber.

```java
public interface Subscription {
    void request(long n);
    void cancel();
}
```

### 4. Processor<T,R>
Acts as both a Subscriber and a Publisher (for transformation).

```java
public interface Processor<T,R> extends Subscriber<T>, Publisher<R> {
}
```

## Key Concepts

### Back Pressure
Mechanism allowing subscribers to control the rate at which they receive items from publishers, preventing overwhelming the subscriber.

### Asynchronous Processing
Items can be processed asynchronously without blocking threads.

### Stream Transformation
Processors can transform data streams between publishers and subscribers.

## Topics to Cover

1. **Fundamentals**
   - Understanding Reactive Streams specification
   - Publisher-Subscriber pattern
   - Subscription lifecycle
   - Back pressure mechanisms

2. **Creating Publishers**
   - Simple custom publishers
   - SubmissionPublisher (built-in implementation)
   - Multi-subscriber scenarios
   - Error handling in publishers

3. **Creating Subscribers**
   - Custom subscriber implementations
   - Handling onNext, onError, onComplete
   - Request strategies
   - Cancellation

4. **Processors**
   - Building transformation pipelines
   - Filtering and mapping
   - Combining streams
   - Error recovery

5. **Back Pressure Strategies**
   - Buffer-based back pressure
   - Drop strategies
   - Latest strategies
   - Custom back pressure handling

6. **Integration**
   - Integration with reactive libraries (Reactor, RxJava)
   - Using with CompletableFuture
   - Virtual threads and Flow API
   - Database reactive drivers

7. **Best Practices**
   - Thread safety considerations
   - Resource management
   - Error handling patterns
   - Testing reactive streams

## Use Cases

- Real-time data streaming
- Event-driven architectures
- Handling large data sets with limited memory
- Network I/O with back pressure
- Asynchronous database operations
- Message processing systems

## SubmissionPublisher

Java provides `SubmissionPublisher<T>` as a ready-to-use implementation:

```java
try (SubmissionPublisher<String> publisher = new SubmissionPublisher<>()) {
    publisher.subscribe(subscriber);
    publisher.submit("Hello");
    publisher.submit("World");
}
```

## Requirements

- Java 9+
- Understanding of asynchronous programming
- Familiarity with functional interfaces

## Comparison with Other Reactive Libraries

| Feature | Flow API | Project Reactor | RxJava |
|---------|----------|-----------------|--------|
| Part of JDK | Yes | No | No |
| Operators | Limited | Extensive | Extensive |
| Learning Curve | Moderate | Steep | Steep |
| Use Case | Standard API | Spring ecosystem | Android, general |

## Examples Structure

Examples will include:
- Basic publisher-subscriber implementations
- Back pressure demonstrations
- Error handling scenarios
- Real-world streaming applications
- Performance benchmarks

---

*This folder will contain practical examples and comprehensive explanations of Java Flow API.*
