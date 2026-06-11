# 1. Concept
This is the way use to decouple Java threads from OS thread. When once Virtual Thread has blocked –> JVM will suspend it, and free carried thread, so another VT can run on it.
# 2. Why it matters ?
## 2.1. The world before VTs
Before VTs each thread has a mapping 1:1 with OS thread, it’s means, each thread has:
- Memory call stack (~1MB) : 10000 threads * 1MB = 10GB
- Context Switching cost
–> Creating thousand threads became expensive
## 2.2. How did we solve this before
Use Thread Pools:
- Reusing expensive OS threads –> request will waiting for thread in thread pools completed 
- The Problem: each application need to tune thread pools
	- Too small: Request wait in queue
	- Too large: Memory wasting, Context Switching Cost
Use reactive programming: 
- The idea: don’t block thread
- The problem: Harder code, debugging, many things to learn
## 2.3. How VM solve well
The VTs are a lightweight JVM-managed threads, multiple VTs can work with a small number of OS threads, when facing with blocking operation instead of blocking OS thread —> JVM will unmount Virtual Thread, let the OS work with another VTs. When the blocking operation completed –> the VTs rescheduled and continues execution on available OS thread
# 3. Pitfalls
- The JVM cannot create more CPU: VM improves concurrency not parallelism. ex: 8 CPU cores, 10000 VTs –> only 8 VTs can executes simultaneously
- Some operations prevent unmount: block when holding sync monitor, native methods
- Unlimited threads can kill the system: The JVM might be survive, but the downstream service can’t
