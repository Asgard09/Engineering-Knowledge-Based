# 1. Spring Framework Modules
| Module             | Purpose                                                                    |
| ------------------ | -------------------------------------------------------------------------- |
| **Core Container** | IoC and Dependency Injection (`BeanFactory`, `ApplicationContext`)         |
| **AOP**            | Aspect-Oriented Programming for cross-cutting concerns (logging, security) |
| **Data Access**    | JDBC abstraction, ORM integration, transaction management                  |
| **Web**            | Spring MVC, WebFlux (reactive), REST support                               |
| **Security**       | Authentication, authorization, protection against common vulnerabilities   |
| **Test**           | Support for unit and integration testing with JUnit and Mockito            |
| **Messaging**      | Support for JMS, AMQP, Kafka integration                                   |
| **Cloud**          | Tools for building microservices and distributed systems                   |
# 2. Core Concepts
## 2.1. Inversion Of Control (IOC)
IoC is a **design principle** where the framework control the flow of a program rather than developer code. Instead of objects create their own dependencies, the **IoC** container creates them and injects them where needed.
## 2.2. Dependency Injection
DI is the mechanism Spring uses to implement IoC. 
***Three Types Of DI***

| Type                      | Mechanism                                                  | Pros                                                                                                                                                                                                                       | Cos                                                                                                                                                                                                                                                                                                                                                        |
| ------------------------- | ---------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Constructor Injection** | Dependencies passed via constructor parameters             | 1. Immutability: Declaring dependencies as final. Once the application start they cannot change, or set to be null.                                                                                                        | 1. Verbose: require boiler plate code to write out the constructor.<br>2. Circular Dependencies: If `ServiceA` needs `ServiceB`, and `ServiceB` needs `ServiceA`, Constructor Injection will crash the application on startup                                                                                                                              |
| **Setter Injection**      | Dependencies set through setter methods after construction | 1. Flexibility: You can change or swap out the dependency at runtime if needed.<br>2. Solves Circular Dependencies: Because objects are created before their setters are called, it can resolve circular dependency loops. | Mutability: Because there is a public setter, anyone can accidentally change the dependency later, breaking thread safety.                                                                                                                                                                                                                                 |
| **Field Injection**       | Spring injects directly into fields via `@Autowired`       | Extremely Clean: It requires the least amount of code. No constructors, no setters—just one simple annotation.                                                                                                             | 1. Hidden Dependencies: The class hides what it needs to function. Looking at the class definition doesn't immediately tell you what must be passed in.<br>2.Tightly Coupled to Spring: You cannot easily create this class in a plain unit test without using heavy Spring reflection tools<br>3.Immutability Is Lost: Fields cannot be declared `final`. |

## 2.3. Spring Beans
Spring Beans is a object created and managed by Spring IoC Container. Beans are the building blocks of a Spring application.
## 2.4. Bean Scopes
| Scope                   | Description                            | Use Case                             |
| ----------------------- | -------------------------------------- | ------------------------------------ |
| **Singleton** (default) | One instance per application context   | Shared services, configuration       |
| **Prototype**           | New instance every time it's requested | User-specific data, stateful objects |
| **Request**             | One instance per HTTP request          | Web request-scoped data              |
| **Session**             | One instance per HTTP session          | User session data                    |
| **Global Session**      | One instance per global session        | Portlet applications                 |

# 3. IoC Container Types
|Container|Description|When to Use|
|---|---|---|
|**BeanFactory**|Basic container with lazy bean initialization|Low-memory environments, simple applications|
|**ApplicationContext**|Advanced container with event propagation, AOP integration, internationalization|**Preferred** for most modern applications|

# Interview Questions 
## What’s DI ?
**Dependency Injection (DI)** is a design pattern where an object's dependencies are provided from the outside rather than being created by the object itself. This reduces coupling between components, making the application easier to maintain, test, and extend. In Spring, DI is implemented through the **IoC (Inversion of Control) Container**, which creates and manages beans and automatically injects their dependencies. Spring supports constructor injection, setter injection, and field injection, with constructor injection generally being the recommended approach.

## @Component, @Service, @Repository, and @Controller
`@Component`, `@Service`, `@Repository`, and `@Controller` are all Spring stereotypes used to register classes as beans in the Spring container, but they represent different layers of the application. **`@Component`** is a generic stereotype for any Spring-managed bean, while **`@Service`** is intended for business logic, **`@Repository`** for data access logic, and **`@Controller`** for handling web requests. Although they behave similarly in terms of bean registration, using the appropriate annotation improves code readability and follows the layered architecture principle. Additionally, `@Repository` provides automatic exception translation for persistence-related exceptions.

## @Bean vs @Component
Both `@Bean` and `@Component` are used to register objects as Spring beans, but they differ in how the bean is created. **`@Component`** is applied directly to a class and relies on component scanning, allowing Spring to automatically detect and instantiate it. **`@Bean`** is applied to a method inside a `@Configuration` class, giving developers explicit control over how the bean is created and configured. I typically use `@Component` for classes I own and `@Bean` when configuring third-party libraries or when custom initialization logic is required.
