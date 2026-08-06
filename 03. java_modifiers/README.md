# Modifiers

## Modifiers in Java
- **Definition** → Keywords that give the compiler information about classes, methods, and variables.  
- **Types**:  
  - **Access Modifiers** → `public`, `protected`, `private`, *default (package-private)*.  
  - **Non-Access Modifiers** → `static`, `final`, `abstract`, `synchronized`, `volatile`, `transient`, `native`, `strictfp`.  

### Access Modifiers
- **Public** → Accessible everywhere.  
- **Private** → Accessible only within the same class. Not visible to subclasses.  
- **Protected** → Accessible in the same package + subclasses (even in different packages, but only via subclass reference).  
- **Default (Package-private)** → Accessible only within the same package by any or child classes.  

### Key Rules
- **Class Level**:  
  - Top-level classes can only be `public` or `default`.  
  - Inner classes can be `public`, `protected`, or `private`.  
- **Members**:  
  - Variables, methods, and constructors can use all access modifiers.  
  - If a class is not accessible, its members are not accessible even if declared `public`.  
- **Inheritance**:  
  - Methods cannot be overridden with *more restrictive* access.  
  - Allowed direction:  
    ```
    private → default → protected → public
    ```
  - Example: A `protected` method in parent can be overridden as `public` in child, but not as `private`.  

### Access Modifier Table
| **Modifier** | **Same Class** | **Same Package** | **Subclass (diff package)** | **Other Packages** |
|----------------|----------------|----------------|----------------|----------------|
| **Private** | ✅ | ❌ | ❌ | ❌ |
| **Default** | ✅ | ✅ | ❌ | ❌ |
| **Protected** | ✅ | ✅ | ✅ (via subclass ref) | ❌ |
| **Public** | ✅ | ✅ | ✅ | ✅ |


### Example
```java
class Parent {
    private int a = 10;
    int b = 20;          // default
    protected int c = 30;
    public int d = 40;
}

class Child extends Parent {
    void print() {
        // System.out.println(a); // ❌ private not accessible
        System.out.println(b);   // ✅ default (same package)
        System.out.println(c);   // ✅ protected
        System.out.println(d);   // ✅ public
    }
}
```

## Final Modifier in Java

### **Final Class**
- Cannot be subclassed.  
- Example:
  ```java
  public final class String {
      // String is final, cannot be extended
  }
  ```
- Use case: Security, immutability, preventing inheritance.


### **Final Method**
- Cannot be overridden in subclasses.  
- Example:
  ```java
  class Parent {
      public final void display() {
          System.out.println("Final method");
      }
  }
  class Child extends Parent {
      // ❌ Cannot override display()
  }
  ```


### **Final Variable**
- Value cannot be reassigned once initialized.  
- Must be assigned at declaration, in an initializer block, or in every constructor.  
- Example:
  ```java
  final int x = 10; // must be initialized
  ```


### **Static Final**
- Must be initialized in a **static block** or at **declaration**.  
- Example:
  ```java
  static final int MAX;
  static {
      MAX = 100;
  }
  ```


### **Instance Final**
- Must be initialized in constructor or instance initializer.  
- Example:
  ```java
  final int id;
  public MyClass(int id) {
      this.id = id; // must assign here
  }
  ```


### **Blank Final**
- Declared but not initialized; can be assigned only once later.  
- Example:
  ```java
  final int value; // blank final
  public MyClass(int v) {
      value = v; // assigned once
  }
  ```


### **Final Parameter**
- Read-only inside the method.  
- Example:
  ```java
  void process(final int data) {
      // data = 20; ❌ compiler error
      System.out.println(data);
  }
  ```


### **Local Final**
- Useful in anonymous classes or lambdas (must be effectively final).  
- Example:
  ```java
  void test() {
      final int num = 5;
      Runnable r = () -> System.out.println(num);
      r.run();
  }
  ```


## Sealed Classes (Java 15+)

### **Definition**
- Restrict which classes can extend or implement them.  
- Declared with `sealed` keyword + `permits`.

### **Example**
```java
sealed class Shape permits Circle, Rectangle { }

final class Circle extends Shape { }       // cannot be extended further
non-sealed class Rectangle extends Shape { } // open for extension
```

### **Subclass Options**
- `final` → cannot be extended further.  
- `sealed` → restricts further inheritance.  
- `non-sealed` → removes restriction, allows open extension.


## 📊 Final vs Sealed
| **Aspect** | **Final** | **Sealed** |
|------------|-----------|------------|
| Inheritance | Prevents subclassing entirely | Restricts subclassing to specific classes |
| Flexibility | Rigid | Controlled flexibility |
| Use Case | Immutable classes, security | Domain modeling, exhaustive type hierarchies |
| Introduced | Java 1.0 | Java 15 (preview), Java 17 (standard) |


👉 **Observation for exams:**  
- `final` = immutability & restriction.  
- `sealed` = controlled inheritance.  
- Both are about **limiting change**, but sealed gives more flexibility in defining *who* can extend.  

