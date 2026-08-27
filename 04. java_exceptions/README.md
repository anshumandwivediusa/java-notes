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

- **Unchecked exceptions are usually programming errors → fix the code logic instead of catching them everywhere.**

- **Checked exceptions represent recoverable conditions → must be handled explicitly.**

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



## 8. Error Propogation resposibility principal

### Using `throw`
- **Purpose**: Actively signal an exceptional condition from within a method.  
- **Scenario**:
  - Input validation (e.g., division by zero).  
  - Business rule violation (e.g., `CartEmptyException`).  
  - Custom exceptions for domain logic.  
- **Example**:
```java
if (b == 0) {
    throw new ArithmeticException("Division by zero");
}
```
👉 Use when the method detects an error it cannot handle locally.



### Using `try–catch`
- **Purpose**: Handle exceptions locally, recover, or log.  
- **Scenario**:
  - Boundary layers (UI, controllers, API endpoints).  
  - Operations where recovery is possible (file I/O, parsing, DB rollback).  
  - Logging and user-friendly error messages.  
- **Example**:
```java
try {
    int value = Integer.parseInt("abc");
} catch (NumberFormatException e) {
    System.out.println("Invalid number format!");
}
```
👉 Use when you can meaningfully handle or recover from the exception.



#### Using `throws`
- **Purpose**: Declare that a method may propagate exceptions to its caller.  
- **Scenario**:
  - Library/utility methods that cannot decide how to handle errors.  
  - Checked exceptions (e.g., `IOException`, `SQLException`).  
  - APIs where caller must handle or propagate further.  
- **Example**:
```java
public void readFile(String path) throws IOException {
    Files.readAllLines(Path.of(path));
}
```


### Conceptual Sense
- **`throw`** → *I’m raising an error now.*  
- **`try–catch`** → *I’ll handle the error here.*  
- **`throws`** → *I might throw an error, caller must handle it.*  


✅ In short:  
- Use **`throw`** inside methods when detecting errors.  
- Use **`try–catch`** at boundaries where recovery/logging is possible.  
- Use **`throws`** in method signatures to declare possible exceptions for the caller.  



## Java Exception Handling Notes

### Key Rules
- **finally always executes** → even if exception occurs, except when JVM exits (`System.exit(0)`), fatal error, or abrupt thread kill.
- **try-with-resources (Java 7+)** → auto‑closes resources implementing `AutoCloseable`.
- **Multiple catch blocks** → handle different exceptions separately.
- **Multi-catch (Java 7+)** → `catch (IOException | SQLException e)`.
- **Re-throwing exceptions** → useful for propagating errors up the call stack.


### Difference: try-catch vs try-finally
- **try-catch** → handles exception locally.
- **try-finally** → ensures cleanup, but exception propagates if not caught.
- Use **try-catch** when you want to handle errors.
- Use **try-finally** when you only care about cleanup (closing resources).


### Exception in try-finally without catch
- Exception is thrown in `try`.
- `finally` executes.
- Exception still propagates to the caller.


### Role of finally block
- Always executes (cleanup code).
- Runs whether exception occurs or not.
- Used for closing resources, releasing locks, etc.


### Does finally always execute?
- ✅ Yes, except:
  - JVM exits (`System.exit(0)`).
  - Fatal error halts execution. 
  - Thread killed abruptly.


### System.exit(0) inside try
- JVM terminates immediately.  
- `finally` block does **not** execute.  


### Can finally throw exception?
- ✅ Yes, but not recommended.  
- If `finally` throws an exception, it can **mask** the original exception.  


### Execution order in try-catch-finally
- `try` → executes first.
- If exception → `catch` executes.
- `finally` → executes last, always.


### Return in catch vs finally
- If both have return statements → `finally` return overrides `catch` return.
- Dangerous practice, avoid returning from `finally`.


### Advantages of try-with-resources
- Auto‑closes resources (`AutoCloseable`).
- Cleaner, less boilerplate than `try-finally`.
- Prevents resource leaks.
- Multiple resources can be declared in one `try`.


### Interface for try-with-resources
- Resource must implement **`AutoCloseable`** (or `Closeable`).


### Multiple resources in try-with-resources
- ✅ Allowed.
- Declared with semicolons:
  ```java
  try (BufferedReader br = ...; FileWriter fw = ...) { ... }
  ```


### Checked vs Unchecked exceptions
- **Checked** → must be handled or declared (`IOException`, `SQLException`).
- **Unchecked** → runtime errors, not mandatory (`NullPointerException`, `ArithmeticException`).
- `try-catch` is required for checked exceptions.


### Multiple exceptions in one catch
- ✅ Supported since Java 7.
- Syntax:
  ```java
  catch (IOException | SQLException e) { ... }
  ```


### Nested try blocks
- ✅ Allowed.
- Inner `try` handles specific exceptions.
- Outer `try` can handle broader exceptions.
- Control flows outward if inner block doesn’t catch.


### Best Practices
- Avoid empty catch blocks → hides errors.
- Always log exceptions → helps debugging.
- Prefer `try-with-resources` → cleaner, safer resource management.
- Catch specific exceptions first, general last.
- Don’t use exceptions for normal control flow.


### Quick Recap
- **try-catch** → handle.
- **try-finally** → cleanup.
- **try-catch-finally** → both.
- **try-with-resources** → modern, preferred.
- **finally** → always executes (except JVM exit).
- **Best practice** → log, avoid empty catch, prefer auto‑closing resources.
- **try-finally** → cleanup.
- **try-catch-finally** → both.
- **try-with-resources** → modern, preferred.
- **finally** → always executes (except JVM exit).
- **Best practice** → log, avoid empty catch, prefer auto‑closing resources.
