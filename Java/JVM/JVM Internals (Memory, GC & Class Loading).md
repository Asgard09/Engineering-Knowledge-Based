# 1. JVM Architecture Overview
![[Pasted image 20260518172159.png]]
# 2. Runtime Memory Areas
## 2.1. Heap (Shared, GC-managed)
Beginner Concepts: The “warehouse” and the “Desk”
- The Heap (The warehouse): This is a massive, shared storage facility where every object you create (new User(), new ArrayList<>()) permanently lives, but required Garbage Collector to clean up abandoned items.
- The Stack (The desk): Every thread gets its own tiny, private desk. You cannot put a giant ArrayList on the desk. You can put tiny primitive (int, char, float) or Remote Controls (Pointers/References) on the desk. When a method end, the entire stack will be clean.
Store all objects instance and arrays. Divide into generation for GC efficiency
```java
Heap
├── Young Generation
│   ├── Eden Space        (~80% of young gen)
│   ├── Survivor 0 (S0)  (~10%)
│   └── Survivor 1 (S1)  (~10%)
└── Old Generation (Tenured)
```
- Eden: new objects allocated here.
- Survivors: Objects have survived after a minor GC move between S0 and S1. 
- Old Generation: Long-lived objects promoted from Young Generation after surviving multiple GC Cycle (default: 15 Cycle)
>  
>  We need both S0 and S1 because objects have different sizes, the empty space left behind in S0 by dead objects become unpredictable "trap". The incoming live object from eden won't fit correctly into those "trap". it will be appended to the end, causing wasted memory.
> 

> Golden Rule: One of the survivor space (S0 or S1) must be empty before BG begin

## 2.2. Method Area
## 2.3. VM Stack (Per-thread)
Each thread [[Concurrency Fundamentals#1.2. Process vs Thread]] has it own stack. Each method call creates a stack frame containing:
- Local variable array: method parameters and local variables
- Operand stack: intermediate computation values
- Frame data: constant pool reference, return address.
# 3. Object Lifecycle
## 3.1. Object Creation
When the JVM encounters `new` instruction:
1. Class loading check
2. Memory Allocation: 
	1. Bump the pointer: if heap is compacted, just move the pointer forward
	2. Free list: if heap is fragmented, find a suitable gap
3. Initialize to Zero: Set all field to default values
4. Set object Header: Store class pointer, hash code, GC age, lock info
5. Execute `init` : Run the constructor
## 3.2. Object Memory Layout
```
┌─────────────────────────────────────────┐
│              Object Header              │
│  ┌───────────────┐  ┌───────────────┐  │
│  │  Mark Word    │  │  Class Pointer│  │
│  │  (hash, GC    │  │  (pointer to  │  │
│  │  age, lock)   │  │  Class meta)  │  │
│  └───────────────┘  └───────────────┘  │
├─────────────────────────────────────────┤
│           Instance Data                 │
│  (fields from this class + parents)     │
├─────────────────────────────────────────┤
│           Padding (alignment)           │
└─────────────────────────────────────────┘
```

# 4. Garbage Collection
## 4.1. How GC Identifies Garbage
***Reference Counting*** : Each object has a counter incremented / decremented when references are added/removed. Object are garbage when count = 0.
## 4.4. Generational Collection
Most object died young. The JVM exploits this:

| Generation | Algorithms                 | Trigger      | Name                |
| ---------- | -------------------------- | ------------ | ------------------- |
| Young      | Copying (Eden to Survivor) | Eden Full    | Minor GC / Young GC |
| Old        | Mark-Compact or Mark-Sweep | Old gen full | Major GC / Old GC   |
| Both       | Full heap collection       | Various      | Full GC             |
***Minor GC Flows***
1. New object allocated in Eden
2. Eden fills up ->  (Minor GC Triggered)
3. Live objects in Eden + active Survivor → copy them to the empty Survivor
4. Ages incremented; objects exceeding threshold (default 15) → promoted to Old gen
5. If Survivor can't hold all survivors -> Overflow to Old Gen
# 6. Class Loading
## 6.1. Class Loading Process
```
Loading → Verification → Preparation → Resolution → Initialization
│          │              │              │             │
│          │              │              │             └─ Execute <clinit>
│          │              │              │                (static initializers)
│          │              │              └─ Resolve symbolic
│          │              │                 references to direct
│          │              └─ Allocate memory for
│          │                 static fields (set defaults)
│          └─ Verify bytecode correctness
│             (format, semantics, bytecode, symbol)
└─ Read .class file into memory,
   create Class object
```


# Interview Questions
## JVM, JRE, JDK
JVM, JRE, and JDK** are different components of the Java ecosystem with distinct responsibilities. The **JVM (Java Virtual Machine)** executes Java bytecode and provides platform independence. The **JRE (Java Runtime Environment)** includes the JVM and the libraries required to run Java applications, while the **JDK (Java Development Kit)** includes the JRE plus development tools such as the Java compiler (`javac`), debugger, and other utilities. In short, the JVM runs Java code, the JRE provides the runtime environment, and the JDK provides everything needed to develop and run Java applications.

## What’s ClassLoader, how many types?
A **ClassLoader** is a component of the JVM responsible for loading Java classes into memory at runtime. It loads `.class` files only when they are needed, which helps optimize memory usage and application startup time.
- Bootstrap ClassLoader: Loads core Java classes such as `java.lang.String` and `java.util.*`.
- Platform ClassLoader: Loads Java platform libraries and modules.
- Application ClassLoader: Loads classes from the application's classpath.