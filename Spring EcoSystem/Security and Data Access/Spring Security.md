# 1. Definition
Spring Security is a powerful authentication (who you are) and authorization (what are you allowed to do) frame work for Java application. It secured request before reaching controllers. 
# 2. Why it matter?
## 2.1. The world before
Without standard frame work, every team need to implement security logic from scratch –> different, and cause vulnerabilities.
## 2.2. How it solve well
Use `SecurityFilterChain` to centralize authentication, authorization, session management, attack protection ([[CSRF]] , [[CORS]]),  password encoding
The `SecurityFilterChain` will intercept requests before reaching `DispatcherServlet`, it include: 
`CorsFilter` `CsrfFilter` `AuthenticationFilter` `ExceptionTranslationFilter` `AuthorizationFilter`
## 2.3. Authentication workflow
1. The client send request.
2. `SecurityFilterChain` intercepted that request.
3. The framework checks for valid credentials (username and passwor d or a [[JWT]] token).
4. If credential is valid –> create authentication object and stores it in `SecurityContext` (allowing application identify the currently authenticated user during request processing).
5. If invalid –> block request automatically.