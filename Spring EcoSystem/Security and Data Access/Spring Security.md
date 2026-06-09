# 1. What is Spring Security
Spring Security is a authentication and access control framework for Java application
# 2. Why use Spring Security

| Security needs      | Spring Provides                                                                    |
| ------------------- | ---------------------------------------------------------------------------------- |
| Authentication      | Supports form login, HTTP Basic, OAuth2, SAML, LDAP, [[JWT]], and custom providers |
| Authorization       | URL-based and method-level access control with roles and permissions               |
| [[CSRF]] Protection | Enabled by default for stateful applications                                       |
| Session Management  | Session fixation protection, concurrent session control                            |
| Password Storage    | `BCryptPasswordEncoder` and other secure hashing algorithms                        |
| [[CORS]]            | Configurable cross-origin resource sharing                                         |
| Security Header     | X-Content-Type-Options, X-Frame-Options, HSTS, etc.                                |
# 3. How does Spring Security Works
## 3.1. The Security Filter Chain
Spring Security works through a filter chain that intercepts every HTTP request before it reaches 
```
HTTP Request
    │
    ▼
┌───────────────────────────────────┐
│   SecurityFilterChain             │
│  ┌─────────────────────────────┐  │
│  │ CorsFilter                  │  │
│  │ CsrfFilter                  │  │
│  │ AuthenticationFilter        │  │
│  │ ExceptionTranslationFilter  │◄─┼── Catches Security Exceptions
│  │ AuthorizationFilter         │◄─┼── (Replaced FilterSecurityInterceptor in Spring Sec 6)
│  └─────────────────────────────┘  │
└───────────────────────────────────┘
    │
    ▼
DispatcherServlet (Spring MVC)
    │
    ▼
Controller / Endpoint
```
## 3.2. Authentication Architecture
```
AuthenticationFilter
    │
    ▼
AuthenticationManager
    │
    ▼
AuthenticationProvider (can have multiple)
    │
    ▼
UserDetailsService (loads user data)
    │
    ▼
PasswordEncoder (verifies password)
    │
    ▼
SecurityContext (stores authenticated principal)
```
## 3.3. End to end Authentication Flow
1. Clients send HTTP request.
2. DelegatingFilterProxy intercepts the servlet request and delegates it to the Spring-managed FilterChainProxy
3. FilterChainProxy determine which SecurityFilterChain to invoke base on the request URL.
4. The SecurityFilterChain includes multiple filter, eventually executing Authentication Filter (`UsernamePasswordAuthenticationFilter`)
5. The Authentication Filter extracts credentials from the request, and creates unauthenticated Authentication Object. 
6. This Authentication Object is passed into Authentication Manager (the default implementation is ProviderManager).
7. The ProviderManager delegates to one or more AuthenticationProviders (e.g., `DaoAuthenticationProvider`)
8. The `AuthenticationProvider` use a `UserDetailsService` to load the user's record.
9. The `UserDetailsService` returns a `UserDetails` object (containing password and authorities).
10. The `AuthenticationProvider` validates the credentials. If successful, it creates fully, authenticated Authentication Object, and return back to the `Provider Manager`, which return it back to the `Authentication Filter`
11. Finally, the `AuthenticationFilter` stores this authenticated object in `SecurityContextHolder` where it can be used for authorization decisions later