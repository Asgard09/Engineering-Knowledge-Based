# 1. The Bottle neck: Platform Thread
Every time you called `new Thread()` –> Java create Platform Thread. 
## 1.1. The Problem
OS Thread are incredibly heavy.
1. They require ~1MB of RAM just for their call stack. If you create 10000 threads –> burned 10GB of RAM
2. Generating Thread involves trapping in the OS kernel 
3. Switching  between threads takes 1-5 microseconds
Traditional server use Thread Pools limit 200 threads –> the 201st user will wait until one of the 200 complete.
## 1.2. The Numbers
|Resource|Platform Thread|Virtual Thread|
|---|---|---|
|**Memory**|~1MB per thread (fixed stack)|~1KB initially, grows as needed|
|**Creation cost**|~1ms (OS kernel call)|~1μs (JVM-managed)|
|**Context switch**|1–5μs (OS scheduler)|~200ns (JVM scheduler)|
|**Max count**|~5,000–10,000 (limited by RAM)|~1,000,000+ (limited by heap)|
|**Scheduling**|OS kernel|JVM ForkJoinPool|
# 2. What is Virtual Thread
Virtual threads are managed entirely by the **Java Virtual Machine (JVM)**, not the OS. They are extraordinarily cheap to create, consume only bytes of memory, and you can comfortably create **millions** of them on a standard laptop.
***Imagine a giant customer service Call Center.**
- **Platform Threads:** You hire 200 employees (OS Threads). Every time the phone rings, an employee picks up. The customer says, "Hold on, let me find my credit card." The employee is forced to hold the phone to their ear in total silence for 5 minutes (Blocking I/O). Meanwhile, 10,000 other customers are getting a busy signal because all 200 employees are waiting on hold.
- **Virtual Threads:** You still have 200 employees (Carrier Threads), but now they have millions of active phone lines (Virtual Threads). When a customer says, "Hold on," the employee instantly puts that line on hold (Unmounting) and immediately answers the next ringing phone. When the first customer finally finds their credit card, the employee grabs that line back (Mounting) and continues the transaction.
