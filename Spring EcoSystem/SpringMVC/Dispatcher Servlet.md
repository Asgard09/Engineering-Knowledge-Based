# 1. Definition
The `DispatcherServlet` is the front-controller (route request to handlers) of Spring MVC. It it the single entry point for all HTTP request
# 2. Why it matters
## 2.1. The world before
Lots of duplicate code for all Servlet (Authentication, Logging, Exception Handling…)
```XML
UserServlet
 ├─ Authentication
 ├─ Logging
 └─ Exception handling

OrderServlet
 ├─ Authentication
 ├─ Logging
 └─ Exception handling

ProductServlet
 ├─ Authentication
 ├─ Logging
 └─ Exception handling
```
## 2.2. How `DispatcherServlet` solve well?
With the front-controller pattern request will be sent to Front Controller (Dispatcher Servlet) before sent to actual handler.
Responsibilities:
1. Receive all incoming requests.
2. Find the correct controller (through by `HandlerMapping`).
3. Invoke the controller methods (Spring decides when to execute based on request) –> IOC.
4. Use [[View Resolvers]] to resolve the view.
5. Send back response to the client.
