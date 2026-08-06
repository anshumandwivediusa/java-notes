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


Would you like me to also prepare a **visual diagram of access levels (private → default → protected → public)** so you can quickly recall the hierarchy during interviews?
