# 1. Concepts
Is a mechanism that automatically configures Spring beans base on dependencies, current configuration in application.
# 2. Why it matters?
## 2.1. The world before
Before Spring Boot (Spring Framework era): one application need a lots of manual configuration:
- Dispatcher Servlet
- View Resolver
- Component Scan
- Data Source
- Transaction Manager
## 2.2. How Spring Auto Config solve well
When `@EnableAutoConfiguration` is present (included in `@SpringBootApplication`), Spring Boot:
1. Read `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`
2. Load the listed auto-config classes
3. Evaluates conditional annotation on each class
4. Register beans only if conditions are met
## 2.3. Example Datasource Auto-Configuration
```Java
@AutoConfiguration
@ConditionalOnClass({ DataSource.class, EmbeddedDatabaseType.class })
@ConditionalOnMissingBean(type = "io.r2dbc.spi.ConnectionFactory")
@EnableConfigurationProperties(DataSourceProperties.class)
public class DataSourceAutoConfiguration {

    @Configuration
    @ConditionalOnMissingBean(DataSource.class)
    @ConditionalOnProperty(name = "spring.datasource.url")
    static class PooledDataSourceConfiguration {
        // Creates HikariCP DataSource with properties from application.yml
    }
}
```
**What this means:** Spring Boot only creates a `DataSource` if:
- `DataSource` class is on the classpath ✓
- No R2DBC `ConnectionFactory` exists ✓
- No custom `DataSource` bean is already defined ✓
- `spring.datasource.url` property is set ✓
# 3. Pitfalls
