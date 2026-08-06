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


### Final vs Sealed
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

## Abstract Modifier in Java

### **Abstract Class**
- Declared with the `abstract` keyword.  
- Cannot be instantiated directly.  
- Must be subclassed to provide concrete implementations.  
- Opposite of `final`:  
  - `final` → cannot be subclassed.  
  - `abstract` → must be subclassed.  

**Example:**
```java
abstract class Shape {
    abstract void draw(); // abstract method
    void info() {         // concrete method
        System.out.println("I am a shape");
    }
}
```

### **Abstract Method**
- Declared without a body; ends with `;`.  
- Must be implemented by subclasses unless they are also abstract.  
- Example:
  ```java
  abstract class Shape {
      abstract void draw(); // no body
  }
  class Circle extends Shape {
      @Override
      void draw() {
          System.out.println("Drawing circle");
      }
  }
  ```

### **Rules for Abstract Classes**
1. If a class has any abstract methods, it must be declared abstract.  
2. If a class inherits abstract methods but does not implement them, it must be declared abstract.  
3. If a class claims to implement an interface but does not provide implementations, it must be declared abstract.  
4. A class can be abstract even if it has **no abstract methods** (used to prevent instantiation).  

### **Abstract vs Final**
| **Aspect** | **Abstract** | **Final** |
|------------|--------------|-----------|
| Instantiation | Cannot be instantiated | Can be instantiated |
| Subclassing | Must be subclassed | Cannot be subclassed |
| Methods | May contain abstract methods | Methods cannot be overridden |
| Purpose | Defers implementation | Prevents modification |

### **Abstract + Interfaces**
- Abstract classes can partially implement interfaces.  
- If not all methods are implemented, the class must remain abstract.  
- Example:
  ```java
  interface Drawable {
      void draw();
  }
  abstract class Shape implements Drawable {
      // no implementation → must be abstract
  }
  ```
- Public → All interface methods are implicitly public.
- You cannot declare them as private, protected, or default (package-private).
- Abstract → All interface methods are implicitly abstract (unless they are default or static methods introduced in Java 8).
This means they have no body and must be implemented by the implementing class.

### Observations
- Abstract methods **cannot be private, static, or final**.
  - Private methods are not visible to subclasses.
  - Since abstract methods must be implemented by subclasses, making them private would make them inaccessible.  
  - Static methods belong to the class, not to instances.
  - Abstract methods are meant to be overridden by subclasses at the instance level.
- Abstract classes can have constructors (used during subclass instantiation).  
- Abstract classes can mix abstract and concrete methods.  
- You can declare an abstract class without abstract methods (to prevent instantiation).  
- Abstract classes are often used in **template design patterns**.  

## Static
- Can be applied to nested classes, methods, variables, free floating code-block (static initializer)
- Static variables are initialized at class load time. A class has only one copy of these variables.
- Static variables in Java are stored in the JVM’s method area (Metaspace in Java 8+), not in the stack. They are initialized at class load time, and only one copy exists per class, shared across all instances.
- Static methods can access only static variables. (They have no this)
- Access by class name is a recommended way to access static methods/variables.
- Static initializer code is run at class load time.
- Static methods may not be overridden to be non-static.
- Non-static methods may not be overridden to be static.
- Abstract methods may not be static.
- Local variables cannot be declared as static.
- Actually, static methods are not participating in the usual overriding mechanism of invoking the methods based on the class of the object at runtime. Static method binding is done at compile time, so the method to be invoked is determined by the type of reference variable rather than the actual type of the object it holds at runtime.
 Let’s say a sub-class has a static method which ‘overrides’ a static method in a parent class. If you have a reference variable of parent class type and you assign a child class object to that variable and invoke the static method, the method invoked will be the parent class method, not the child class method. The following code explains this.


p
