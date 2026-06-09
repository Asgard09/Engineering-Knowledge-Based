## 1. What is stored in Stack Memory
- Method Call Frames: Every time a method is invoked, a new block (frame) is created on top of the stack.
- Primitive local variables: Types like `int`, `double`, `float`, `boolean`, `char`, `byte`, `short`, and `long`
- Object References: The actual memory address/pointer of an object stored in the heap.
- Method Parameters: Arguments passed into the method.
- Return Addresses: Information telling the JVM where to return control after the method finishes.
#### 1.1. Key properties of Stack Memory
- Thread-local: Each thread has its own dedicated stack. This make local variable inherit thread-safe because they cannot be accessed by other threads.

## 2. What's stored in Heap Memory
- Objects created with new
- Objects fields: 
- Arrays:
- String pool:
## 3. Key Difference 

| Features        | Stack                           | Heap                                 |
| --------------- | ------------------------------- | ------------------------------------ |
| Scope           | Per-thread (Isolate)            | Application wide (Shared)            |
| Object Lifetime | Lives only until the method run | Lives until reclaimed by GC          |
| Storage         | Primitives and Reference        | Full Object and Class Metadata       |
| Access Speed    | Very Fast                       | Slower (Require Pointer Dereference) |
| Common Failure  | StackOverFlowError              | OutOfMemoryError                     |
| Management      | Manual (LIFO) by JVM            | Automatic by GC                      |
# Interview Questions
## Stack Memory and Heap Memory
In Java, **stack memory** and **heap memory** serve different purposes. Stack memory stores local variables, method parameters, and method call information, while heap memory stores objects and their instance data. The stack is automatically managed and much faster because memory is allocated and released when methods are called and completed. In contrast, objects in the heap can be shared across multiple methods, and their memory is managed by the Garbage Collector when they are no longer referenced.
