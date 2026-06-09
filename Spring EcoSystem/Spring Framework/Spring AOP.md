> **Aspect-Oriented Programming** is a program paradigm that separates **cross-cutting concern** logic that spans multiple layers of an application (logging, security, caching, metrics, transactions) - from the core business logic. In Spring, AOP is implemented via  proxies:  Spring wraps your beans in a proxy object that intercepts method calls and applies the cross-cutting behavior before, after, or around the actual method execution.

# 1. Beginner View
Imagine you are building a restaurant ordering system. Every time a waiter takes an order, you need to:
1. **Log** the start and end of the action.
2. **Check** that the waiter is authenticated.
3. **Measure** how long the operation took.
**Without AOP:** You add logging, auth checks, and timing code inside every single service method. If you have 50 methods, you repeat this boilerplate 50 times. If the log format changes, you update 50 places.
```Java
// ❌ Cross-cutting concerns scattered everywhere
public Order createOrder(CreateOrderCommand cmd) {
    log.info("START createOrder: {}", cmd);           // logging
    authService.assertAuthenticated();                // security
    long start = System.currentTimeMillis();          // metrics

    Order order = new Order(cmd); // ← actual business logic (1 line)
    orderRepository.save(order);

    long duration = System.currentTimeMillis() - start;
    metricsService.record("createOrder", duration);  // metrics
    log.info("END createOrder: {}ms", duration);     // logging
    return order;
}
```
**With AOP:** You write the logging, auth, and timing logic _once_ in a dedicated **Aspect** class. Spring automatically applies it to every method you specify — without touching the methods themselves.
```Java
// ✅ Business logic is clean
public Order createOrder(CreateOrderCommand cmd) {
    Order order = new Order(cmd);
    orderRepository.save(order);
    return order;
}

// Cross-cutting logic lives here — applied automatically
@Aspect
@Component
public class LoggingAspect {
    @Around("execution(* com.example.service.*.*(..))")
    public Object logExecutionTime(ProceedingJoinPoint pjp) throws Throwable {
        log.info("START {}", pjp.getSignature().getName());
        long start = System.currentTimeMillis();
        Object result = pjp.proceed();
        log.info("END {} in {}ms", pjp.getSignature().getName(),
                System.currentTimeMillis() - start);
        return result;
    }
}
```
# 2. Core Concepts
Before writing any AOP code, you must understand the five building blocks. Every other concept flows from these.

|Concept|Definition|Analogy|
|---|---|---|
|**Aspect**|A class containing cross-cutting logic|The security guard at the door|
|**Advice**|The action the Aspect takes (`@Before`, `@After`, `@Around`)|What the guard actually does (checks ID, logs entry)|
|**Pointcut**|An expression that matches which method(s) to intercept|The rule "check everyone entering the VIP lounge"|
|**Join Point**|A specific method execution that matched the Pointcut|The moment Alice walks through the door|
|**Weaving**|The process of applying the Aspect to the target object|Installing the guard at the door|
![[Pasted image 20260604213700.png|center]]