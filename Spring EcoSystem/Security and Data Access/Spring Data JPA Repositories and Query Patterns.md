# 1. What's Spring Data JPA
Spring Data JPA is part of the larger Spring Data project. Eliminating the need to write the boilerplate data access code.
- JPA is the interface (a specification) for object-relational mapping in Java.
- Hibernate is the default JPA implementation that handles actual database operations.
- Spring Data JPA sits on top of the JPA provider. It hides the `EntityManager` complexity and provides clean interface for data access.
# 2. Why use Spring Data JPA
## 2.1. Problems It Solves 
| Problem                                    | How Spring Data JPA Fixes It                    |
| ------------------------------------------ | ----------------------------------------------- |
| Repetitive CRUD boilerplate                | Auto-generated proxy implementations at runtime |
| Manual query writing for simple operations | Query derivation from method names              |
| Complex pagination and sorting logic       | Built-in `Pageable` and `Sort` support          |
| Tedious `EntityManager` management         | Automatic session and transaction handling      |
| Verbose DAO layer                          | Single interface replaces the DAO class         |
| Migration workflow consistency             | Works with Flyway/Liquibase                     |
|                                            |                                                 |
## 2.2. Core Benefits
1. Zero boilerplate for CRUD and common list operations.
2. Queries from method name.
3. Custom JPQL/native queries via annotations.
4. First-class pagination and sorting support
5. Auditing integration
6. Tight Spring Boot Integration
# 3. Repository Hierarchy
Spring Data JPA provides a specific hierarchy of interfaces, each adding more specialized functionality:
```
Repository (Marker interface, no methods)
  -> CrudRepository (Adds basic CRUD operations like save, findById, delete)
      -> PagingAndSortingRepository (Adds findAll(Pageable) and findAll(Sort))
          -> JpaRepository (Adds JPA-specific methods like flush(), batch deletes)
```
# 4. Relationships and Fetching
## 4.1. Many to Many
```Java
@Entity
public class Student {
    @ManyToMany
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id")
    )
    private Set<Course> courses = new HashSet<>();
}
```
## 4.2. Fetch Types
|FetchType|Behavior|Default For|
|---|---|---|
|`EAGER`|Loads related entities immediately with parent|`@ManyToOne`, `@OneToOne`|
|`LAZY`|Loads related entities only when accessed|`@OneToMany`, `@ManyToMany`|
# Interview Questions
## What difference between JPA and Hibernate
 **JPA (Java Persistence API)** is a specification that defines a standard way to interact with databases using Java objects, while **Hibernate** is one of the most popular implementations of that specification. JPA provides interfaces and annotations such as `@Entity`, `@Id`, and `EntityManager`, whereas Hibernate provides the actual implementation that performs ORM operations behind the scenes. Using JPA makes the application less dependent on a specific ORM provider, allowing easier switching between implementations if needed. In practice, many Spring Boot applications use JPA as the programming model and Hibernate as the default JPA implementation.
 
## Why use JpaRepository instead of CrudRepository?
Both **`CrudRepository`** and **`JpaRepository`** provide basic CRUD operations, but **`JpaRepository`** offers additional JPA-specific features and is generally more powerful. Since `JpaRepository` extends `PagingAndSortingRepository`, which in turn extends `CrudRepository`, it includes all CRUD functionality plus pagination, sorting, batch operations, and methods like `flush()`. These extra features make it more suitable for real-world applications where efficient data retrieval and management are important. Therefore, I usually choose `JpaRepository` because it provides more capabilities without any additional complexity.

## Eager and Lazy fetch ?
 **EAGER** and **LAZY** are fetching strategies in JPA that determine when related entities are loaded from the database. **EAGER fetching** loads the associated entity immediately when the parent entity is retrieved, while **LAZY fetching** delays loading until the relationship is actually accessed. LAZY is generally preferred because it reduces unnecessary database queries and improves performance, especially when dealing with large object graphs. However, EAGER can be useful when related data is always needed together with the parent entity.
