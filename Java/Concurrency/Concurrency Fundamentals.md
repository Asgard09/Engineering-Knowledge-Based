## 1. Threads and Processes
#### 1.1. Beginner Concepts
Multithreading is famously difficult to learn. Imaging a massive, professional kitchen:
- The process: This is the entire kitchen building. it include its own isolated walls, 5 ovens, fridge ( heap memory). If it burns down, the restaurant is totally fine. 
	- Program + Execution = Process
- The threads: These are the individual chefs are working inside the kitchen. They all share the exact same ovens, and fridge (Shared Memory). They have their own professional cutting board (The Call Stack).
	- The Danger: Because the chefs use share same fridge. If they takes same resources at the exact same time (millisecond), you get a kitchen disaster (A Race Condition).


