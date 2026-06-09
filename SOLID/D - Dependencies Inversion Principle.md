Don't let your important business logic classes depend directly on concrete implementations (like a specific database driver, a specific email provider, etc.).
# Bad Example
```Java
// Low-level class — a specific implementation
public class MySQLUserRepository {
    public void save(String username) {
        System.out.println("Saving to MySQL: " + username);
    }
}
```

```Java
// High-level class depends DIRECTLY on the low-level MySQLUserRepository
public class UserService {

    // Tightly coupled to MySQL! ❌
    private MySQLUserRepository repository = new MySQLUserRepository();

    public void registerUser(String username) {
        // some business logic...
        repository.save(username);
    }
}
```

# Good Example
```Java
// The abstraction — both sides depend on this
public interface UserRepository {
    void save(String username);
}
```

```Java
// Low-level: MySQL implementation
public class MySQLUserRepository implements UserRepository {
    @Override
    public void save(String username) {
        System.out.println("Saving to MySQL: " + username);
    }
}
```

```Java
// Low-level: PostgreSQL implementation (easy to add!)
public class PostgreSQLUserRepository implements UserRepository {
    @Override
    public void save(String username) {
        System.out.println("Saving to PostgreSQL: " + username);
    }
}
```

```Java
// High-level: UserService now depends on the INTERFACE, not a concrete class
public class UserService {

    private final UserRepository repository; // interface, not concrete class ✅

    // Dependency is INJECTED — not created inside
    public UserService(UserRepository repository) {
        this.repository = repository;
    }

    public void registerUser(String username) {
        // business logic...
        repository.save(username);
    }
}
```

