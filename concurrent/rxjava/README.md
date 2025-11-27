# RxJava (Reactive Extensions for Java)

## Overview

RxJava is a Java implementation of Reactive Extensions, a library for composing asynchronous and event-based programs using observable sequences. It was one of the first popular reactive libraries for Java and has been widely adopted, especially in Android development.

## What is RxJava?

RxJava provides:
- Rich set of operators for async operations
- Multiple reactive types for different scenarios
- Powerful composition capabilities
- Scheduler-based concurrency control
- Extensive error handling mechanisms
- Support for backpressure

## Core Types

### Observable<T>
Emits 0 to N items, may not support backpressure.

```java
Observable<Integer> observable = Observable.just(1, 2, 3, 4, 5);
Observable<Long> interval = Observable.interval(1, TimeUnit.SECONDS);
```

**Use when:**
- Backpressure is not needed
- UI events (clicks, text changes)
- Simple event streams

### Flowable<T>
Emits 0 to N items with backpressure support.

```java
Flowable<Integer> flowable = Flowable.range(1, 1000);
```

**Use when:**
- Dealing with large amounts of data
- Backpressure is required
- Reading from databases or files
- Network requests with streaming

### Single<T>
Emits exactly one item or an error.

```java
Single<String> single = Single.just("Hello");
Single<User> user = api.getUser(userId);
```

**Use when:**
- Network requests (REST API calls)
- Database queries returning single result
- Async computations with single result

### Maybe<T>
Emits 0 or 1 item or an error.

```java
Maybe<String> maybe = Maybe.empty();
Maybe<User> user = database.findUserById(id); // might not exist
```

**Use when:**
- Optional results
- Database queries that might return nothing
- Cache lookups

### Completable
Emits no items, only completion or error signal.

```java
Completable completable = Completable.fromAction(() -> {
    // perform action
});
```

**Use when:**
- Operations that don't return a value
- Write operations
- Cleanup tasks

## RxJava Versions

### RxJava 2.x
- Complete rewrite following Reactive Streams specification
- Separate types for backpressure (Observable vs Flowable)
- Requires Java 6+
- No null values allowed

### RxJava 3.x
- Built on Reactive Streams
- Requires Java 8+
- Improved API
- Better performance
- Full support for Java 8 features

## Topics to Cover

1. **Fundamentals**
   - Understanding reactive paradigm
   - Observable, Flowable, Single, Maybe, Completable
   - Creating observables
   - Subscribing and disposing
   - Hot vs Cold observables

2. **Creation Operators**
   - just, from, create
   - defer, interval, timer
   - range, empty, never, error
   - Custom creation

3. **Transformation Operators**
   - map, flatMap, concatMap, switchMap
   - scan, reduce
   - buffer, window
   - groupBy
   - cast, ofType

4. **Filtering Operators**
   - filter, take, skip
   - distinct, distinctUntilChanged
   - debounce, throttle
   - first, last, elementAt
   - takeUntil, takeWhile, skipUntil, skipWhile

5. **Combining Operators**
   - merge, concat
   - zip, combineLatest
   - startWith, withLatestFrom
   - join, switchOnNext

6. **Error Handling**
   - onErrorReturn, onErrorResumeNext
   - retry, retryWhen
   - onErrorComplete
   - Error handling strategies

7. **Schedulers**
   - Schedulers.io() - I/O operations
   - Schedulers.computation() - CPU-intensive work
   - Schedulers.newThread() - New thread for each
   - Schedulers.single() - Single background thread
   - Schedulers.trampoline() - Current thread queue
   - AndroidSchedulers.mainThread() (Android)
   - subscribeOn vs observeOn

8. **Backpressure**
   - Understanding backpressure
   - Backpressure strategies (BUFFER, DROP, LATEST, ERROR)
   - onBackpressureBuffer, onBackpressureDrop, onBackpressureLatest
   - Flowable vs Observable

9. **Subjects**
   - PublishSubject
   - BehaviorSubject
   - ReplaySubject
   - AsyncSubject
   - When to use subjects

10. **Testing**
    - TestObserver, TestSubscriber
    - TestScheduler
    - Virtual time
    - Testing async code

11. **Advanced Topics**
    - Custom operators
    - Processors
    - ConnectableObservable
    - Multicasting
    - Resource management
    - Performance optimization

## Use Cases

- Android applications (UI events, networking)
- Asynchronous data processing
- Event-driven systems
- Real-time data streaming
- Combining multiple async operations
- Handling complex async workflows
- Reactive UI programming

## Maven Dependencies

### RxJava 3
```xml
<dependency>
    <groupId>io.reactivex.rxjava3</groupId>
    <artifactId>rxjava</artifactId>
    <version>3.1.8</version>
</dependency>
```

### RxJava 2
```xml
<dependency>
    <groupId>io.reactivex.rxjava2</groupId>
    <artifactId>rxjava</artifactId>
    <version>2.2.21</version>
</dependency>
```

### Android
```gradle
implementation 'io.reactivex.rxjava3:rxjava:3.1.8'
implementation 'io.reactivex.rxjava3:rxandroid:3.0.2'
```

## RxJava vs Other Reactive Libraries

| Feature | RxJava | Project Reactor | Mutiny |
|---------|--------|-----------------|--------|
| Types | 5 types | 2 types (Mono, Flux) | 2 types (Uni, Multi) |
| Primary Use | Android, General | Spring ecosystem | Quarkus |
| Learning Curve | Moderate | Moderate-Steep | Easy |
| Operators | Very extensive | Extensive | Focused |
| Android Support | Excellent | Limited | Limited |
| Backpressure | Flowable only | Always | Always |

## Key Operators

### Map vs FlatMap
```java
// map: 1-to-1 transformation
Observable.just(1, 2, 3)
    .map(i -> i * 2)
    .subscribe(System.out::println); // 2, 4, 6

// flatMap: 1-to-many transformation (returns Observable)
Observable.just(1, 2, 3)
    .flatMap(i -> Observable.range(1, i))
    .subscribe(System.out::println); // 1, 1, 2, 1, 2, 3
```

### ConcatMap vs SwitchMap
```java
// concatMap: preserves order, waits for each to complete
Observable.just(1, 2, 3)
    .concatMap(i -> Observable.just(i).delay(1, TimeUnit.SECONDS))
    .subscribe(System.out::println);

// switchMap: switches to latest, cancels previous
Observable.interval(100, TimeUnit.MILLISECONDS)
    .switchMap(i -> api.getData(i))
    .subscribe(System.out::println);
```

### Zip vs CombineLatest
```java
// zip: combines items pairwise
Observable.zip(
    Observable.just(1, 2, 3),
    Observable.just("A", "B", "C"),
    (num, letter) -> num + letter
).subscribe(System.out::println); // 1A, 2B, 3C

// combineLatest: emits when any source emits
Observable.combineLatest(
    source1, source2,
    (a, b) -> a + b
).subscribe(System.out::println);
```

## Schedulers Example

```java
Observable.just(1, 2, 3, 4, 5)
    .subscribeOn(Schedulers.io())        // subscription happens on IO thread
    .observeOn(Schedulers.computation()) // downstream operators on computation thread
    .map(i -> i * 2)
    .observeOn(Schedulers.single())      // further downstream on single thread
    .subscribe(System.out::println);
```

## Error Handling Patterns

```java
// Retry with exponential backoff
Observable.fromCallable(() -> api.getData())
    .retryWhen(errors -> errors
        .zipWith(Observable.range(1, 3), (error, attempt) -> attempt)
        .flatMap(attempt -> Observable.timer(
            (long) Math.pow(2, attempt), TimeUnit.SECONDS
        ))
    )
    .onErrorReturn(error -> defaultValue)
    .subscribe();
```

## Best Practices

1. **Always dispose subscriptions**
   - Use CompositeDisposable for multiple subscriptions
   - Dispose in lifecycle methods (Android)
   - Prevent memory leaks

2. **Choose the right type**
   - Use Single for single values
   - Use Maybe for optional values
   - Use Completable for no-value operations
   - Use Flowable when backpressure is needed

3. **Understand threading**
   - subscribeOn affects subscription
   - observeOn affects downstream
   - Multiple observeOn calls are cumulative

4. **Error handling**
   - Always handle errors
   - Use appropriate error operators
   - Don't swallow errors

5. **Testing**
   - Use TestObserver/TestSubscriber
   - Test both success and error paths
   - Use TestScheduler for time-based operations

6. **Performance**
   - Avoid creating unnecessary observables
   - Use appropriate backpressure strategies
   - Be careful with buffering operators

## Common Pitfalls

1. Not disposing subscriptions (memory leaks)
2. Confusing subscribeOn and observeOn
3. Using wrong reactive type
4. Blocking the main thread
5. Not handling errors
6. Creating observables in loops
7. Misunderstanding hot vs cold observables

## Android Integration

```java
// Example with Android
public class MainActivity extends AppCompatActivity {
    private CompositeDisposable disposables = new CompositeDisposable();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        Disposable disposable = api.getUsers()
            .subscribeOn(Schedulers.io())
            .observeOn(AndroidSchedulers.mainThread())
            .subscribe(
                users -> updateUI(users),
                error -> showError(error)
            );

        disposables.add(disposable);
    }

    @Override
    protected void onDestroy() {
        disposables.clear();
        super.onDestroy();
    }
}
```

## Examples Structure

Examples will cover:
- Basic Observable/Flowable creation
- All operator categories
- Scheduler usage
- Error handling patterns
- Backpressure strategies
- Android integration (if applicable)
- Testing examples
- Real-world use cases
- Performance optimization

---

*This folder will contain comprehensive RxJava examples with runnable code and detailed explanations.*
