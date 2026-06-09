# 1. What's Spring Boot
Spring Boot is an opinionated framework built on top of the Spring Framework. It eliminate most of the boilerplate configuration rather than Spring Framework, letting developer focus on business logic instead of infrastructure config.
# 2. How does Spring Boot help developers
## 2.1. Starter Dependencies
```XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
This pull in Spring MVC, Jackson, embedded Tomcat, and validation 
***Common Starter***

| Starter                          | What it provides                      |
| -------------------------------- | ------------------------------------- |
| `spring-boot-starter-web`        | REST/MVC, embedded Tomcat, Jackson    |
| `spring-boot-starter-data-jpa`   | JPA, Hibernate, Spring Data           |
| `spring-boot-starter-security`   | Spring Security defaults              |
| `spring-boot-starter-test`       | JUnit 5, Mockito, AssertJ, MockMvc    |
| `spring-boot-starter-actuator`   | Health, metrics, info endpoints       |
| `spring-boot-starter-validation` | Bean Validation (Hibernate Validator) |
## 2.2. Auto-Configuration
Spring Boot examines the class path at startup and automatically configures beans:
- `DataSource` if H2/MySQL/PostgreSQL driver is detected
- `EntityManagerFactory` if JPA is on the classpath
- `DispatcherServlet` if Spring MVC is present
- `SecurityFilterChain` if Spring Security is present
# 3. Externalized Configuration
Spring Boot supports a powerful property resolution order:
1. Command-line arguments
2. `SPRING_APPLICATION_JSON` (inline JSON)
3. OS environment variables
4. `application-{profile}.properties` / `.yml`
5. `application.properties` / `.yml`
6. `@PropertySource` annotations
7. Default properties
# 4. The `@SpringBootApplication` Annotation
This single annotation combines three powerful annotations:
``` Java
@SpringBootApplication
// Equivalent to:
// @SpringBootConfiguration  — marks this as a configuration class
// @EnableAutoConfiguration  — enables auto-configuration
// @ComponentScan           — scans the current package and sub-packages
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```
# 5. Spring Boot vs Spring Framework

| Aspects       | Spring Framework          | Spring Boot                        |
| ------------- | ------------------------- | ---------------------------------- |
| Configuration | Manual (XML or Java)      | Auto-Configuration                 |
| Server        | External                  | Embedded                           |
| Dependencies  | Manual version management | Starter POMs with managed versions |
# Interview Question
## What difference between Spring and Spring Boot
 **Spring** is a comprehensive framework that provides core features such as dependency injection, transaction management, and web development support. **Spring Boot** is built on top of Spring and simplifies application development by providing auto-configuration, embedded servers, and starter dependencies. With Spring, developers need to configure many components manually, whereas Spring Boot follows a convention-over-configuration approach to reduce setup time. In practice, I use Spring Boot for most modern projects because it allows me to build and deploy applications much faster while still leveraging all the capabilities of the Spring Framework.
 
 > **Manual dependency management** means developers must explicitly define and manage the libraries and versions required by the application. In traditional Spring projects, when adding a feature such as Spring MVC, you often need to specify multiple related dependencies and ensure their versions are compatible. Spring Boot simplifies this by providing **starter dependencies**, which automatically include the required libraries with tested and compatible versions. As a result, developers spend less time managing dependencies and avoid version 

## What’s Spring Boot starter ?
**Spring Boot Starters** are predefined dependency packages that bundle together all the libraries commonly needed for a specific feature or use case. Instead of manually adding and managing multiple dependencies, developers can include a single starter such as `spring-boot-starter-web` or `spring-boot-starter-data-jpa`. This simplifies dependency management, ensures version compatibility, and reduces configuration effort. As a result, developers can focus more on business logic and less on project setup.

