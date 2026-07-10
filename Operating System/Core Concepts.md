# 1. Process and Threads
A process is an independence program in execution with it’s own system resource and memory space. A thread i a smallest unit of execution within process share same memory and resource.
# 2. Concurrency and Parallelism
Concurrency and Parallelism are both technique to execute multiple task, but they differ in how the tasks are executed.
- Concurrency: means multiple task make progress during same period of time, but not necessarily at the same time.
- Parallelism: means multiple task execute exact the same time on different CPU cores.
# 4. Context Switching
Context Switching is the process use to pause and save current state of process or thread, and restore the state of another process or thread and resume its execution. It is mechanism to enable concurrency. but it has a cost, it can overhead because,  saving and restoring take time
# 3. Deadlocks 
A deadlocks occurs when process or thread waiting resource from another process or thread, causing none of them to make progress. A common way to prevent is use lock timeouts, minimize lock holding time.
