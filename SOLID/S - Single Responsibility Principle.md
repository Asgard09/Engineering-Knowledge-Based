Every class should do **exactly one thing** and do it well. If a class is handling multiple unrelated responsibilities, then it has multiple reasons to change — that's a problem.
# Bad Example
``` Java
// This class does WAY too much
public class UserService {

    public void registerUser(String username, String email) {
        // 1. Validate input
        if (username == null || email == null) {
            throw new IllegalArgumentException("Invalid input");
        }

        // 2. Save to database
        System.out.println("Saving user to DB: " + username);

        // 3. Send welcome email
        System.out.println("Sending email to: " + email);

        // 4. Log the event
        System.out.println("LOG: User registered - " + username);
    }
}
```
# Good Example
```Java
// Handles only validation
public class UserValidator {
    public void validate(String username, String email) {
        if (username == null || email == null) {
            throw new IllegalArgumentException("Invalid input");
        }
    }
}
```
