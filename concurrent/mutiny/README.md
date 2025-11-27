# Mutiny

## Overview

Mutiny is a modern reactive programming library designed for building resilient and efficient applications. It is the reactive programming library used by Quarkus, designed with a focus on developer experience, performance, and integration with the Quarkus ecosystem.

## What is Mutiny?

Mutiny provides:
- Intuitive API for reactive programming
- Two core types: Uni and Multi
- Event-driven programming model
- Integration with Quarkus and Vert.x
- Structured concurrency support
- Built-in helpers for common patterns

## Core Components

### Uni<T>
Represents an asynchronous operation that emits **0 or 1 item** or a failure.

```java
Uni<String> uni = Uni.createFrom().item("Hello");
Uni<String> delayed = Uni.createFrom().item("World")
    .onItem().delayIt().by(Duration.ofSeconds(1));
```

**Think of Uni as:**
- CompletableFuture replacement
- Single asynchronous computation
- Similar to Mono in Project Reactor

### Multi<T>
Represents an asynchronous operation that emits **0 to N items**, a failure, or a completion signal.

```java
Multi<Integer> multi = Multi.createFrom().items(1, 2, 3, 4, 5);
Multi<Long> ticks = Multi.createFrom().ticks().every(Duration.ofSeconds(1));
```

**Think of Multi as:**
- Stream of multiple items
- Similar to Flux in Project Reactor
- Observable/Flowable in RxJava

## Key Concepts

### Event-Driven API
Mutiny uses an event-driven approach with explicit event handlers:
- `onItem()`: When an item is emitted
- `onFailure()`: When a failure occurs
- `onCompletion()`: When the stream completes
- `onSubscription()`: When subscription happens
- `onCancellation()`: When subscription is cancelled

### Lazy Evaluation
Operations are lazy and only execute when subscribed.

### Built for Quarkus
- Native integration with Quarkus
- RESTEasy Reactive
- Hibernate Reactive
- Reactive messaging
- Reactive routes

## Mutiny vs Other Reactive Libraries

| Feature | Mutiny | Project Reactor | RxJava |
|---------|--------|-----------------|--------|
| Core Types | Uni, Multi | Mono, Flux | Single, Observable, Flowable |
| Primary Framework | Quarkus | Spring | Android, General |
| API Style | Event-driven | Operator-based | Operator-based |
| Learning Curve | Easy | Moderate-Steep | Moderate-Steep |
| Null Handling | Explicit | Not allowed | Varies |

## Topics to Cover

1. **Fundamentals**
   - Uni and Multi basics
   - Creating Uni and Multi
   - Subscribing and consuming
   - Event-driven model
   - Lazy vs eager evaluation

2. **Uni Operations**
   - Creating Unis from various sources
   - Transforming items (map, flatMap)
   - Combining Unis (combine, join)
   - Error handling
   - Timeouts and delays
   - Fallback strategies

3. **Multi Operations**
   - Creating Multis from various sources
   - Transformation operators
   - Filtering and selection
   - Combining streams
   - Grouping and collecting
   - Back pressure handling

4. **Error Handling**
   - onFailure().recoverWithItem()
   - onFailure().retry()
   - onFailure().transform()
   - onFailure().invoke()
   - Timeout handling

5. **Concurrency and Parallelism**
   - runSubscriptionOn()
   - emitOn()
   - Parallel processing
   - Thread management
   - Infrastructure (executors)

6. **Integration with Quarkus**
   - RESTEasy Reactive endpoints
   - Hibernate Reactive
   - Reactive messaging (Kafka, AMQP)
   - Reactive database clients
   - Reactive routes with Vert.x

7. **Testing**
   - AssertSubscriber
   - Testing Uni
   - Testing Multi
   - Virtual time
   - Failure scenarios

8. **Advanced Patterns**
   - Caching and memoization
   - Pagination
   - Retry with exponential backoff
   - Circuit breaker
   - Rate limiting
   - Hot vs cold streams

9. **Streams and Back Pressure**
   - Overflow strategies
   - Buffering
   - Demand management
   - Custom back pressure

10. **Context Propagation**
    - Maintaining context across async boundaries
    - Integration with CDI
    - Request context

## Use Cases

- Quarkus microservices
- Reactive REST APIs
- Event-driven applications
- Reactive database access
- Message-driven systems
- Real-time data processing
- High-performance APIs
- Cloud-native applications

## Maven Dependencies

```xml
<dependencies>
    <!-- Mutiny Core -->
    <dependency>
        <groupId>io.smallrye.reactive</groupId>
        <artifactId>mutiny</artifactId>
        <version>2.5.0</version>
    </dependency>

    <!-- For Quarkus (already included) -->
    <dependency>
        <groupId>io.quarkus</groupId>
        <artifactId>quarkus-resteasy-reactive</artifactId>
    </dependency>

    <!-- Hibernate Reactive -->
    <dependency>
        <groupId>io.quarkus</groupId>
        <artifactId>quarkus-hibernate-reactive-panache</artifactId>
    </dependency>
</dependencies>
```

## Gradle Dependencies

```gradle
implementation 'io.smallrye.reactive:mutiny:2.5.0'
```

## Key Features

### 1. Intuitive API
```java
// Uni example
Uni.createFrom().item("hello")
    .onItem().transform(String::toUpperCase)
    .onItem().delayIt().by(Duration.ofSeconds(1))
    .subscribe().with(
        item -> System.out.println(item),
        failure -> System.err.println(failure)
    );

// Multi example
Multi.createFrom().range(1, 10)
    .onItem().transform(i -> i * 2)
    .select().where(i -> i > 5)
    .subscribe().with(
        item -> System.out.println(item)
    );
```

### 2. Null Safety
Mutiny allows `null` items and has explicit handling:
```java
Uni.createFrom().nullItem()
    .onItem().ifNull().continueWith("default")
```

### 3. Shortcuts and Helpers
```java
// await() for blocking (testing/debugging)
String result = uni.await().atMost(Duration.ofSeconds(5));

// toMulti() conversion
Multi<String> multi = uni.toMulti();
```

### 4. Retry and Recovery
```java
uni.onFailure().retry()
    .withBackOff(Duration.ofSeconds(1))
    .atMost(3)
    .onFailure().recoverWithItem("fallback")
```

## Best Practices

1. **Never Block**
   - Avoid blocking operations
   - Use reactive database drivers
   - Use reactive HTTP clients

2. **Error Handling**
   - Always handle failures
   - Use appropriate recovery strategies
   - Log errors properly

3. **Testing**
   - Use AssertSubscriber
   - Test both success and failure paths
   - Test cancellation scenarios

4. **Resource Management**
   - Clean up resources properly
   - Use onTermination() for cleanup
   - Cancel subscriptions when not needed

5. **Performance**
   - Be mindful of buffering
   - Use appropriate overflow strategies
   - Monitor back pressure

6. **Quarkus Integration**
   - Leverage CDI properly
   - Use @Blocking annotation when needed
   - Understand execution model

## Quarkus Example

```java
@Path("/users")
public class UserResource {

    @Inject
    UserService userService;

    @GET
    @Path("/{id}")
    public Uni<User> getUser(@PathParam("id") Long id) {
        return userService.findById(id);
    }

    @GET
    public Multi<User> getAllUsers() {
        return userService.findAll();
    }

    @POST
    public Uni<Response> createUser(User user) {
        return userService.create(user)
            .onItem().transform(created ->
                Response.status(201).entity(created).build()
            );
    }
}
```

## Event-Driven API Examples

```java
// Uni events
uni
    .onSubscription().invoke(() -> log("Subscribed"))
    .onItem().invoke(item -> log("Received: " + item))
    .onFailure().invoke(failure -> log("Failed: " + failure))
    .onCancellation().invoke(() -> log("Cancelled"));

// Multi events
multi
    .onSubscription().invoke(() -> log("Subscribed"))
    .onItem().invoke(item -> log("Item: " + item))
    .onFailure().invoke(failure -> log("Failed: " + failure))
    .onCompletion().invoke(() -> log("Completed"))
    .onCancellation().invoke(() -> log("Cancelled"));
```

## Examples Structure

Examples will include:
- Basic Uni and Multi operations
- Quarkus REST endpoints
- Hibernate Reactive integration
- Reactive messaging
- Error handling patterns
- Testing examples
- Real-world microservices
- Performance optimization
- Integration patterns

---

*This folder will contain comprehensive Mutiny examples with Quarkus integration and detailed explanations.*
