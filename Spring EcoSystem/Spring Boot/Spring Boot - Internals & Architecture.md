# 1. How Auto-Configuration Works
When `@EnableAutoConfiguration` is present (included in `@SpringBootApplication`), Spring Boot:
1. Reads `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` (Spring Boot 3.x) or `META-INF/spring.factories` (2.x) from every JAR on the class path
> The `AutoConfiguration.imports`  stores **a list of names**—specifically, the names of Java classes that know how to configure those dependencies if they happen to be there.
2. Loads the listed auto-configuration classes
3. Evaluates conditional annotations on each class
4. Registers beans only if conditions are met