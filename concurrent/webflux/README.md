# Spring WebFlux

## Overview

Spring WebFlux is a reactive web framework introduced in Spring 5. It provides a non-blocking, reactive programming model for building web applications with better resource utilization and scalability. WebFlux is built on Project Reactor and is part of the Spring reactive stack.

## What is Spring WebFlux?

WebFlux is designed for:
- Non-blocking reactive applications
- High concurrency with fewer threads
- Backpressure support
- Functional and annotation-based programming models
- Efficient resource utilization

## Architecture

### Two Programming Models

#### 1. Annotated Controllers (Traditional)
```java
@RestController
@RequestMapping("/users")
public class UserController {
    @GetMapping("/{id}")
    public Mono<User> getUser(@PathVariable String id) {
        return userService.findById(id);
    }

    @GetMapping
    public Flux<User> getAllUsers() {
        return userService.findAll();
    }
}
```

#### 2. Functional Endpoints (Router Functions)
```java
@Configuration
public class UserRouter {
    @Bean
    public RouterFunction<ServerResponse> route(UserHandler handler) {
        return RouterFunctions.route()
            .GET("/users/{id}", handler::getUser)
            .GET("/users", handler::getAllUsers)
            .POST("/users", handler::createUser)
            .build();
    }
}
```

## Core Components

### 1. WebClient
Reactive HTTP client replacing RestTemplate.

```java
WebClient client = WebClient.create("http://api.example.com");

Mono<User> user = client.get()
    .uri("/users/{id}", userId)
    .retrieve()
    .bodyToMono(User.class);
```

### 2. ServerRequest & ServerResponse
Functional programming model for handling requests and responses.

### 3. Reactive Repositories
Integration with reactive data access (R2DBC, Reactive MongoDB, etc.).

```java
public interface UserRepository extends ReactiveCrudRepository<User, String> {
    Flux<User> findByLastName(String lastName);
}
```

## Key Concepts

### Non-Blocking I/O
- Uses Netty (or other async servers) for non-blocking operations
- Threads are never blocked waiting for I/O
- Better resource utilization

### Reactive Streams
- Built on Project Reactor (Mono and Flux)
- Full backpressure support
- Asynchronous data flow

### Event Loop
- Similar to Node.js event loop
- Small number of threads handling many connections
- Excellent for I/O-bound applications

## Topics to Cover

1. **Getting Started**
   - WebFlux vs Spring MVC
   - Setting up a WebFlux application
   - Choosing Netty, Tomcat, or Jetty
   - Project structure

2. **Controllers and Handlers**
   - Annotated controllers
   - Functional endpoints
   - Request and response handling
   - Path variables and request parameters
   - Request body handling

3. **WebClient**
   - Creating WebClient instances
   - GET, POST, PUT, DELETE requests
   - Headers and authentication
   - Error handling
   - Retry and timeout strategies
   - Exchange vs Retrieve

4. **Reactive Data Access**
   - R2DBC (Reactive Relational Database Connectivity)
   - Reactive MongoDB
   - Reactive Redis
   - Reactive Cassandra
   - Repository patterns

5. **Server-Sent Events (SSE)**
   - Streaming data to clients
   - Real-time updates
   - Event stream handling

6. **WebSocket Support**
   - WebSocket handlers
   - Bidirectional communication
   - Message handling

7. **Error Handling**
   - Global error handling
   - @ExceptionHandler
   - onError operators
   - Custom error responses

8. **Security**
   - Spring Security Reactive
   - Authentication and authorization
   - JWT with reactive endpoints
   - Method security
   - CORS configuration

9. **Testing**
   - WebTestClient
   - Testing controllers
   - Testing functional endpoints
   - Mocking reactive services
   - Integration testing

10. **Performance and Optimization**
    - Thread pool configuration
    - Connection pooling
    - Caching strategies
    - Monitoring and metrics

11. **Streaming**
    - Chunked responses
    - File uploads/downloads
    - Streaming large datasets
    - Backpressure handling

## Use Cases

- High-throughput REST APIs
- Real-time data streaming applications
- Microservices with reactive communication
- Applications with many concurrent connections
- Event-driven architectures
- Chat applications
- Live dashboards
- IoT data collection

## Maven Dependencies

```xml
<dependencies>
    <!-- Spring WebFlux -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-webflux</artifactId>
    </dependency>

    <!-- Reactive MongoDB (optional) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-mongodb-reactive</artifactId>
    </dependency>

    <!-- R2DBC (optional) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-r2dbc</artifactId>
    </dependency>

    <!-- Testing -->
    <dependency>
        <groupId>io.projectreactor</groupId>
        <artifactId>reactor-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

## WebFlux vs Spring MVC

| Aspect | WebFlux | Spring MVC |
|--------|---------|------------|
| Programming Model | Reactive, non-blocking | Imperative, blocking |
| Concurrency | Event loop, few threads | Thread per request |
| Best For | I/O-bound, high concurrency | CPU-bound, traditional apps |
| Learning Curve | Steep | Moderate |
| Data Access | Reactive drivers (R2DBC) | JDBC, JPA |
| Scalability | Excellent for I/O | Good with proper tuning |
| Thread Usage | Minimal | High under load |

## When to Use WebFlux?

**Use WebFlux when:**
- High number of concurrent connections
- I/O-bound operations (network, database)
- Streaming requirements
- Microservices with reactive communication
- Modern cloud-native applications

**Stick with Spring MVC when:**
- Existing blocking dependencies (JDBC, JPA)
- Team unfamiliar with reactive programming
- CPU-bound operations
- Simple CRUD applications
- Legacy system integration

## Best Practices

1. **Avoid Blocking Calls**
   - Never use blocking I/O in reactive chains
   - Use reactive database drivers
   - Wrap blocking calls with `subscribeOn(Schedulers.boundedElastic())`

2. **Error Handling**
   - Always handle errors in reactive chains
   - Use appropriate error operators
   - Provide meaningful error responses

3. **Backpressure**
   - Be aware of backpressure
   - Use appropriate buffering strategies
   - Don't overwhelm downstream systems

4. **Testing**
   - Use WebTestClient for integration tests
   - Use StepVerifier for reactive chain testing
   - Test error scenarios

5. **Monitoring**
   - Add metrics and monitoring
   - Use Spring Boot Actuator
   - Monitor thread pools and connection pools

6. **Resource Management**
   - Properly close resources
   - Use try-with-resources where applicable
   - Avoid resource leaks in reactive chains

## Common Patterns

### 1. Parallel Execution
```java
Flux.merge(
    service1.getData(),
    service2.getData(),
    service3.getData()
)
```

### 2. Sequential Execution
```java
service1.getData()
    .flatMap(data -> service2.process(data))
    .flatMap(result -> service3.save(result))
```

### 3. Timeout and Retry
```java
webClient.get()
    .retrieve()
    .bodyToMono(Data.class)
    .timeout(Duration.ofSeconds(5))
    .retry(3)
```

## Examples Structure

Examples will cover:
- REST API development
- WebClient usage
- Reactive database access
- Server-Sent Events
- WebSocket implementation
- Error handling patterns
- Testing strategies
- Real-world microservices
- Performance optimization

---

*This folder will contain practical Spring WebFlux examples with complete applications and detailed explanations.*
