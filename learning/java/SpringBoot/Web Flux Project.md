## Reactive Programming

Reactive Programming in Java is a ==programming paradigm== focused on ==**asynchronous data streams**== and **==non-blocking operations==**, enabling ***efficient*** handling of data flows and events. It is particularly useful for building **responsive**, **scalable**, and **resilient** applications, especially in scenarios involving real-time data processing, high concurrency, or event-driven architectures.

### Key Concepts of Reactive Programming in Java:

1. **Asynchronous Data Streams**:
   - Data is treated as a *stream of events* that can be observed and reacted to asynchronously.
   - Example: User inputs, HTTP requests, or database queries can be modeled as streams.

2. **Non-Blocking Operations**:
   - Operations do not block the main thread, allowing the application to remain responsive and handle multiple tasks concurrently.

3. **Backpressure**:
   - A mechanism to handle situations where the producer (source of data) is faster than the consumer (processor of data). It ensures the system does not get overwhelmed by controlling the flow of data.

4. **Reactive Streams**:
   - A standard specification for asynchronous stream processing with backpressure, defined in the **Reactive Streams API** (`java.util.concurrent.Flow`).
   - Libraries like **Project Reactor**, **RxJava**, and **Akka Streams** implement this specification.

### Reactive Programming Libraries in Java:

1. **Project Reactor**:
   - A core library used in **Spring WebFlux** for building reactive applications.
   - Provides `Mono` (for 0 or 1 item) and `Flux` (for 0 to N items) as reactive types.

2. **RxJava**:
   - A popular library for reactive programming, inspired by **ReactiveX**.
   - Provides `Observable`, `Single`, and `Flowable` as reactive types.

3. **Akka Streams**:
   - Part of the Akka toolkit, designed for building highly concurrent and distributed applications using the Actor model.

### Example: Reactive Programming with Project Reactor

Here’s a simple example using ==**Project Reactor**== to handle a stream of data:

```java
import reactor.core.publisher.Flux;

public class ReactiveExample {
    public static void main(String[] args) {
        // Create a Flux (stream of data)
        Flux<String> flux = Flux.just("Hello", "Reactive", "World");

        // Subscribe to the Flux and process each item
        flux.map(String::toUpperCase)  // Transform each item
            .subscribe(System.out::println);  // Print each item
    }
}
```

Output:
```
HELLO
REACTIVE
WORLD
```

### Benefits of Reactive Programming in Java:

1. **Scalability**:
   - Efficiently handles a large number of concurrent requests with minimal resource usage.

2. **Responsiveness**:
   - Ensures the application remains responsive even under heavy load.

3. **Resilience**:
   - Provides built-in mechanisms to handle failures and recover gracefully.

4. **Flexibility**:
   - Works well with modern architectures like microservices and real-time systems.

### Use Cases for Reactive Programming:

- **Real-time applications** (e.g., chat applications, live data feeds).
- **Microservices architectures** (e.g., handling asynchronous communication between services).
- **High-concurrency systems** (e.g., APIs with thousands of simultaneous requests).
- **Event-driven systems** (e.g., IoT, financial trading platforms).

### Conclusion:

Reactive Programming in Java is a powerful paradigm for building modern, high-performance applications. By leveraging asynchronous and non-blocking techniques, it enables developers to create systems that are scalable, responsive, and resilient. Libraries like **Project Reactor**, **RxJava**, and **Akka Streams** provide the tools needed to implement reactive programming effectively in Java.

## Two classes with the main method in same package

Yes, you can have two classes with a `main` method in the same package in Java. However, when you run the program, you will need to specify which class's `main` method you want to execute. Here’s how it works:

### Example:

```java
// File: ClassA.java
package com.example;

public class ClassA {
    public static void main(String[] args) {
        System.out.println("Running ClassA's main method");
    }
}

// File: ClassB.java
package com.example;

public class ClassB {
    public static void main(String[] args) {
        System.out.println("Running ClassB's main method");
    }
}
```

### Running the Program:

1. **Compile the classes:**
   ```bash
   javac com/example/ClassA.java com/example/ClassB.java
   ```

2. **Run a specific class's `main` method:**
   - To run `ClassA`'s `main` method:
     ```bash
     java com.example.ClassA
     ```
     Output:
     ```
     Running ClassA's main method
     ```

   - To run `ClassB`'s `main` method:
     ```bash
     java com.example.ClassB
     ```
     Output:
     ```
     Running ClassB's main method
     ```

### Key Points:
- Each `main` method is an entry point for the program.
- You can have multiple `main` methods in different classes within the same package.
- When running the program, you must specify which class's `main` method to execute by providing the fully qualified class name (e.g., `com.example.ClassA`).

This flexibility allows you to organize your code with multiple entry points if needed, such as for testing or different application modes.

## @Bean annotation in Java methods

The `@Bean` annotation in Java, specifically within the Spring Framework, is used to indicate that a method produces a bean that should be managed by the Spring container. Here's a deeper explanation:

### Key Points:
1. **Bean Definition**:
   - The `@Bean` annotation marks a method as a producer of a bean. A bean is an object that is managed by the Spring IoC (Inversion of Control) container.
   - The method annotated with `@Bean` returns an object that Spring will register as a bean in its application context.

2. **Usage**:
   - Typically used in configuration classes annotated with `@Configuration`.
   - The method name becomes the name of the bean by default, unless explicitly specified using the `name` attribute of the `@Bean` annotation.

3. **Dependency Injection**:
   - Beans created using `@Bean` can be injected into other components using `@Autowired` or other dependency injection mechanisms.

4. **Customization**:
   - You can configure the bean's lifecycle (e.g., `initMethod` and `destroyMethod`) and scope (e.g., `@Scope("prototype")`) using additional attributes.

### Example:
```java
@Configuration
public class AppConfig {

    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```
In this example:
- The `myService()` method is annotated with `@Bean`.
- Spring will call this method and register the returned `MyService` object as a bean in the application context.
- This bean can then be injected into other components.

### Key Attributes of `@Bean`:
- `name`: Specifies the name of the bean (defaults to the method name).
- `initMethod`: Specifies a method to call after the bean is instantiated.
- `destroyMethod`: Specifies a method to call before the bean is destroyed.

### Summary:
The `@Bean` annotation is used to declare a method as a bean producer in Spring, allowing Spring to manage the object's lifecycle and dependencies. It is a core part of Spring's dependency injection and configuration mechanism.
D