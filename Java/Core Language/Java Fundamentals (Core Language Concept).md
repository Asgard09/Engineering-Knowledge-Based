# 1. Java Platform Overview
## 1.1. JVM vs JDK vs JRE

| Component                      | Description                                                                                              |
| ------------------------------ | -------------------------------------------------------------------------------------------------------- |
| JVM (Java Virtual Machine)     | Executes Java bytecode. Platform-specific — each OS has its own JVM implementation.                      |
| JRE (Java Runtime Environment) | JVM + core class libraries (java.lang, java.util, java.io….) . Everything needed to run Java programs.   |
| JDK (Java Development Kit)     | JRE + development tools (compiler javac, debugger, profiler). Everything needed to develop Java programs |
## 1.2. Bytecode & "Compile Once, Run Anywhere"
Java source code (.java) compile by javac into bytecode (.class files), which is platform independent. The JVM interprets or JIT compiles byte code into native machine code at run time

```
Source.java → javac → Source.class (bytecode) → JVM → Native execution
```

# 2. Data Types and Variables
## 2.1. Primitive Types
## 2.2. Autoboxing and Unboxing
Java automatically converts between primitives and their wrapper classes:
```Java
// 1. Autoboxing: Primitive 'int' to 'Integer' object
int primitive = 42;  
Integer wrapped = primitive;  
System.out.println("Wrapped Object " + wrapped);  
  
// 2. Unboxing: 'Integer' object to primitive 'int'  
Integer another = Integer.valueOf(100);  
int primitiveResult = another;  
System.out.println("Primitive Value: " + primitiveResult);

// 3. Real-world Collections Example  
ArrayList<Integer> numbers = new ArrayList<>();  
  
// Autoboxing: We pass primitive 5 and 10, Java automatically boxes them into Integer objects  
numbers.add(5);  
numbers.add(10);  
  
// Unboxing: Java automatically unboxes the Integer object to a primitive int for addition  
int sum = numbers.get(0) + numbers.get(1);  
System.out.println("Sum: " + sum);
```

***Pitfall - Integer Cache*
Java caches `Integer` values from **-128 to 127**. Comparisons with `==` work for cached values but fail for larger numbers:
```Java
Integer a = 127;
Integer b = 127;
System.out.println(a == b);  // true (cached)

Integer c = 200;
Integer d = 200;
System.out.println(c == d);  // false (different objects)
System.out.println(c.equals(d));  // true (correct way)
```

# 3. Object-Oriented Programming
## 3.1. Properties of OOP
### 3.1.1. Encapsulation
Definition: Hiding internal details, exposing necessary operations. Use access modifiers to control visibility

| Modifier  | Class | Package | Subclass | World |
| --------- | ----- | ------- | -------- | ----- |
| private   | Yes   | No      | No       | No    |
| (default) | Yes   | Yes     | No       | No    |
| protected | Yes   | Yes     | Yes      | No    |
| public    | Yes   | Yes     | Yes      | Yes   |
Example: You press "Make Espresso" but you do not control water pressure, internal pipes. All those details are hidden in machine
### 3.1.2. Abstraction
Definition: Showing essential features, hiding complexity. Implement by Abstract Class or Interface
Example: A user only sees button like Espresso, Latte. Do not need to understand: heated system, coffee making process
### 3.1.3. Inheritance
Definition: Allows a class can reuse properties and behaviors from anther class
Example: Black Coffee, Milk Coffee, Cheese Coffee
### 3.1.4. Polymorphism
Definition: Same action, different behaviors.
Example: in the makeCofee() function but with Latte and Espresso have different behaviour
## 3.2. Interfaces and Abstract Class

| Features             | Interface         | Abstract Class     |
| -------------------- | ----------------- | ------------------ |
| Purpose              | Define a Contract | Provide Share Base |
| Fields               | Only static final | Any field type     |
| Constructor          | No                | Yes                |
| Multiple Inheritance | implement many    | extend one         |
# 4. Key Language Features
## 4.1. The final Keyword
- ``final`` variable: cannot be reassigned after initialization
- `final` method: Cannot be overridden by subclasses.
- `final` class: Class cannot be extend
## 4.2. The static Keyword
- `static` field: Shared across all instances of a class (class-level)
- `static` method: Called on the class itself, not on instances. Cannot access  `this`
- `static` block: Executed one when the class is loaded.
## 4.3. Value passing in Java (Pass-by-value)

# 5. Exception Handling
## 5.1. Exception Hierarchy
```
Throwable
├── Error (unrecoverable — OutOfMemoryError, StackOverflowError)
└── Exception
    ├── Checked Exceptions (must handle — IOException, SQLException)
    └── RuntimeException (unchecked — NullPointerException, IllegalArgumentException)
```

## 5.2. Checked and Unchecked Exceptions

| Feature         | Checked Exception                                                                         | Unchecked Exception                                              |
| --------------- | ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| Handling Rule   | Must be handled using `try-catch` **or** declared in the method signature using `throws`. | Can be caught, but it is not required by the compiler.           |
| Nature of Error | Usually represents **external conditions** outside the program's direct control.          | Usually represents **programming errors** or flaws in the logic. |
| Example         | `IOException`, `ClassNotFoundException`                                                   | `NullPointerException`, `ArrayIndexOutOfBoundsException`         |

# 6. Generics
## 6.1. Why Generics?
Generics provide compile-time safety without casting:
```Java
// Without generics — requires casting, error-prone
List list = new ArrayList();
list.add("hello");
String s = (String) list.get(0);

// With generics — type-safe, no casting
List<String> list = new ArrayList<>();
list.add("hello");
String s = list.get(0);
```

# 12. The equals() and hashCode() Contract
# 13. Enums
Enum define a fixed set of constants with type safety, replacing magic number and string.

# Interview Question
## What is Java, why it still using widely?
Java is high level, object oriented programming language designed to be platform-independent through the Java Virtual Machine (JVM). One of its biggest strengths is the principle of “Write Once – Run Anywhere”, which allows applications to run on different operating systems without modification. Java remains widely because it provides strong performance, readability, and a mature eco-system of frameworks and tools. Additionally, Java continues to evolve with modern features.
## What difference between .equals() and == ?
 `==` and `equals()` are both used for comparison in Java, but they compare different things. The `==` operator compares primitive values or object references, meaning it checks whether two variables point to the same object in memory. In contrast, `equals()` compares the actual content or logical equality of objects, depending on how the method is implemented. For example, two different `String` objects with the value `"Java"` would return `false` with `==` but `true` with `equals()`.
## Why String is immutable ?
String is immutable in Java, meaning its value cannot be changed after the object is created. This design improves **security**, **thread safety**, and **performance**, especially because Strings are widely used in areas like class loading, file paths, URLs, and database connections. Immutability also allows Java to safely implement the String Pool, where identical String values can be shared to reduce memory usage.
## What difference between String, StringBuilder, StringBuffer?
 `String`, `StringBuilder`, and `StringBuffer` are all used to work with text in Java, but they differ in mutability and thread safety. `String` is immutable, meaning any modification creates a new object. `StringBuilder` is mutable and not thread-safe, making it the fastest choice for string manipulation in single-threaded applications. `StringBuffer` is also mutable but synchronized, so it is thread-safe at the cost of some performance overhead. Therefore, use `String` for fixed text, `StringBuilder` for frequent modifications, and `StringBuffer` only when thread safety is required.
## What's access modifier, explain each type ?
Access modifiers in Java control the visibility and accessibility of classes, methods, variables, and constructors. Java provides four access levels: `public`, `protected`, `default` (no modifier), and `private`.
- **public**: Accessible from anywhere in the application.
- **protected**: Accessible within the same package and by subclasses, even in different packages.
- **default** (package-private): Accessible only within the same package.
- **private**: Accessible only within the same class.
## Method Overriding vs Method Overloading
Method overloading and method overriding are both forms of polymorphism in Java, but they occur in different situations. **Method overloading** happens when multiple methods in the same class have the same name but different parameter lists, allowing different ways to perform a similar operation. **Method overriding** occurs when a subclass provides its own implementation of a method already defined in its parent class, enabling runtime polymorphism. In short, overloading is resolved at compile time, while overriding is resolved at runtime.
## Static Members
A **static member** belongs to the class itself rather than to a specific object instance. This means all objects of the class share the same static variable or method, and it can be accessed without creating an object. Static members are commonly used for constants, utility methods, or data that should be shared across all instances. We should use `static` when the behavior or data is related to the class as a whole and does not depend on an object's state.
## This and Super
`this` and `super` are both reference keywords in Java, but they refer to different objects. **`this`** refers to the current object and is commonly used to access instance variables, call methods, or invoke another constructor within the same class. **`super`** refers to the parent class object and is used to access parent class members or invoke the parent class constructor. In short, `this` works with the current class, while `super` is used to interact with the superclass.
## How many ways to use `final`
The `final` keyword in Java can be applied to **variables, methods, and classes**, and its behavior depends on where it is used. A **final variable** can only be assigned once, making it effectively a constant. A **final method** cannot be overridden by subclasses, while a **final class** cannot be extended. The main purpose of `final` is to prevent unwanted modification and improve code reliability.
## Abstract Class and Interface
Both **abstract classes** and **interfaces** are used to achieve abstraction in Java, but they serve different purposes. An **abstract class** can contain both abstract and concrete methods, as well as instance variables and constructors, making it suitable for sharing common state and behavior among related classes. An **interface** defines a contract that classes must follow and supports multiple inheritance, allowing a class to implement multiple interfaces. In general, I use an abstract class when classes share common implementation, and an interface when I want to define capabilities or behaviors that can be applied to different types of classes.
## What’s Record in Java ?
A **Record** is a special type of class introduced in Java to represent immutable data objects with less boilerplate code. When you define a record, Java automatically generates common methods such as constructors, getters, `equals()`, `hashCode()`, and `toString()`. Records are ideal for data transfer objects (DTOs), API responses, or any object whose primary purpose is to hold data. I use a Record instead of a regular class when the object is immutable and does not require additional state management or complex business logic.
## Checked Exception and Unchecked Exception
In Java, exceptions are divided into **checked exceptions** and **unchecked exceptions** based on when they are validated. **Checked exceptions** are checked by the compiler, so they must be either caught with a `try-catch` block or declared using `throws`, such as `IOException` or `SQLException`. **Unchecked exceptions** occur at runtime and do not require explicit handling, such as `NullPointerException`, `ArrayIndexOutOfBoundsException`, or `IllegalArgumentException`. In general, checked exceptions are used for recoverable situations, while unchecked exceptions usually indicate programming errors or invalid application states.
