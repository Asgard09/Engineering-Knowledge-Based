## 1. Threads and Processes
#### 1.1. Beginner Concepts
Multithreading is famously difficult to learn. Imaging a massive, professional kitchen:
- The process: This is the entire kitchen building. it include its own isolated walls, 5 ovens, fridge ( heap memory). If it burns down, the restaurant is totally fine. 
	- Program + Execution = Process
- The threads: These are the individual chefs are working inside the kitchen. They all share the exact same ovens, and fridge (Shared Memory). They have their own professional cutting board (The Call Stack).
	- The Danger: Because the chefs use share same fridge. If they takes same resources at the exact same time (millisecond), you get a kitchen disaster (A Race Condition).
#### 1.2. Process vs Thread

| Aspect          | Process                                 | Thread                                            |
| --------------- | --------------------------------------- | ------------------------------------------------- |
| Definition      | The program has been launched.          | Lightweight unit of execution in a process        |
| Memory          | Isolated address space                  | Shares heap with other threads; has own stack     |
| Communication   | IPC (sockets, pipes, shared memory)     | Shared variables (requires synchronization)       |
| Cost            | Expensive to create/switch              | Cheaper to create/switch                          |
| Crash isolation | One process crash doesn't affect others | One thread crash can bring down the whole process |
