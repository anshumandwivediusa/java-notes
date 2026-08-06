# Exception Handling in Java

## 1. **Definition**
- Exception handling is the mechanism to **handle runtime errors** gracefully without crashing the program.  
- It separates **error-handling code** from **normal business logic**.

## 2. **Hierarchy**
- Root class: `Throwable`  
  - **Error** → Serious issues (e.g., `OutOfMemoryError`), non-recoverable not meant to be handled.  
  - **Exception** → Recoverable problems. Compile‑time checked → must be handled (`try-catch`) or declared (`throws`)
    - **Checked exceptions** → Compiler wants the developer to handle or declare (`IOException`, `SQLException`).  
      - ClassNotFoundException: Thrown by Class.forName(), ClassLoader.loadClass(), or similar methods when the JVM cannot find the class definition at runtime.
    - **Unchecked exceptions** → Runtime exceptions (`NullPointerException`, `ArrayIndexOutOfBoundsException`).

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

- **try-catch** → handle exception locally.
- **try-finally** → cleanup, exception propagates.
- **try-catch-finally** → both handle and cleanup.
- **try-with-resources** → preferred for auto-closing resources.

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


## 7. Multiple Catch Blocks

### **Definition**
- A single `try` block can be followed by **multiple `catch` blocks**.  
- Each `catch` handles a different type of exception.  
- Only **one catch executes** — the first matching type.

### **Rules**
- Order matters → **specific exceptions first, general last** (`Exception` at the end).  
- If a general catch (`Exception`) comes before specific ones, compiler error (unreachable code).  
- Only one catch block runs per thrown exception.  
- Multiple exceptions can be handled separately or combined using **multi‑catch (Java 7+)**.

### ⚙️ Example: Multiple Catch
```java
try {
    int[] arr = new int[3];
    arr[5] = 10; // ArrayIndexOutOfBoundsException
    int result = 10 / 0; // ArithmeticException
} catch (ArithmeticException e) {
    System.out.println("Arithmetic Error: " + e.getMessage());
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Array Error: " + e.getMessage());
} catch (Exception e) {
    System.out.println("General Exception: " + e.getMessage());
}
```

**Output:**
```
Array Error: Index 5 out of bounds for length 3
```

### Example: Multi‑Catch (Java 7+)
```java
try {
    int result = 10 / 0;
} catch (ArithmeticException | NullPointerException e) {
    System.out.println("Error: " + e.getMessage());
}
```

### Exam‑Ready Comparison
| **Pattern** | **Usage** | **Notes** |
|-------------|-----------|-----------|
| **Multiple catch** | Different blocks for different exceptions | Specific first, general last |
| **Multi‑catch** | One block for multiple exceptions | Cleaner, but cannot distinguish exception type inside |



## 8. Notes
- **finally always executes** (even if exception occurs), except when JVM exits (`System.exit(0)`).  
- **try-with-resources** (Java 7+) → Auto-closes resources implementing `AutoCloseable`.  
- **Multiple catch blocks** → Handle different exceptions separately.  
- **Multi-catch (Java 7+)** → `catch (IOException | SQLException e)`.  
- **Re-throwing exceptions** → Useful for propagating errors up the call stack.  
