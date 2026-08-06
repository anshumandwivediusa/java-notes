# Exception Handling in Java

## 1. **Definition**
- Exception handling is the mechanism to **handle runtime errors** gracefully without crashing the program.  
- It separates **error-handling code** from **normal business logic**.

## 2. **Hierarchy**
- Root class: `Throwable`  
  - **Error** → Serious issues (e.g., `OutOfMemoryError`), non-recoverable not meant to be handled.  
  - **Exception** → Recoverable problems. Compile‑time checked → must be handled (`try-catch`) or declared (`throws`)
    - **Checked exceptions** → Must be handled or declared (`IOException`, `SQLException`).  
      - ClassNotFoundException: Thrown by Class.forName(), ClassLoader.loadClass(), or similar methods when the JVM cannot find the class definition at runtime.
    - **Unchecked exceptions** → Runtime exceptions (`NullPointerException`, `ArrayIndexOutOfBoundsException`).
      - NoClassDefFoundError: Thrown when the JVM tries to load a class that was present at compile time but missing at runtime.

<p align="center">
  <img width="500" height="350" alt="image" src="https://github.com/user-attachments/assets/c74105a8-4ad1-4ada-a20a-0cc4960a35b9" />
</p>

## 3. **Keywords**
- **try** → Block of code to monitor for exceptions.  
- **catch** → Handles the exception.  
- **finally** → Always executes (cleanup code).  
- **throw** → Used to explicitly throw an exception.  
- **throws** → Declares exceptions a method may throw.


## 4. Example
```java
public class ExceptionDemo {
    public static void main(String[] args) {
        try {
            int result = 10 / 0; // ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Error: " + e.getMessage());
        } finally {
            System.out.println("Cleanup code executed");
        }
    }
}
```

**Output:**
```
Error: / by zero
Cleanup code executed
```


## 5. Checked vs Unchecked Exceptions
| **Aspect** | **Checked** | **Unchecked** |
|------------|-------------|---------------|
| Compile-time check | ✅ Must be handled/declared | ❌ No compile-time check |
| Examples | IOException, SQLException | NullPointerException, ArithmeticException |
| Handling | try-catch or throws | Optional |


## 6. Best Practices
- **Catch specific exceptions** (not just `Exception`).  
- **Never swallow exceptions silently**.  
- **Use finally or try-with-resources** for cleanup.  
- **Custom exceptions** → Create meaningful domain-specific exceptions.  
- **Avoid overusing checked exceptions** → Can clutter code.  


## 7. Custom Exception Example
```java
class InvalidAgeException extends Exception {
    public InvalidAgeException(String msg) {
        super(msg);
    }
}

public class Test {
    static void validateAge(int age) throws InvalidAgeException {
        if (age < 18) throw new InvalidAgeException("Age must be 18+");
    }

    public static void main(String[] args) {
        try {
            validateAge(15);
        } catch (InvalidAgeException e) {
            System.out.println("Caught: " + e.getMessage());
        }
    }
}
```


## .7. Extra Notes for Exams
- **finally always executes** (even if exception occurs), except when JVM exits (`System.exit(0)`).  
- **try-with-resources** (Java 7+) → Auto-closes resources implementing `AutoCloseable`.  
- **Multiple catch blocks** → Handle different exceptions separately.  
- **Multi-catch (Java 7+)** → `catch (IOException | SQLException e)`.  
- **Re-throwing exceptions** → Useful for propagating errors up the call stack.  


Would you like me to also prepare a **diagram showing the Exception hierarchy (Throwable → Error/Exception → Checked/Unchecked)** so you can visualize it clearly for exams?
