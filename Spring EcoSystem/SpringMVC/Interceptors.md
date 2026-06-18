# 1. Definition
An interceptor is a component of Spring MVC, it will intercept HTTP requests before and after controller execution. It sits between Dispatcher Servlet and controller
# 2. Why it matters ?
## 2.1. The world before
Every controller repeat same code (authentication, logging, authorization, metrics,….). 
## 2.2. How it solved well?
Interceptors allow all common logic to be centralized by implements `HandlerInterceptor` , overriding `preHandler`  `postHandler`  `afterCompletion()` method.
