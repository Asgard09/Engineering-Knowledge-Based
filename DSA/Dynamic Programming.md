# 1. Concept
Solve each subproblem once, store its result, repeat when needed.
# 2. Why it matters?
## 2.1. The world before
With Fibonacci problem when want to calculate fib(n) we need adding fib(n+1) and fib(n+2). But fib(n+1) = fib(n+2) + fib(n+3) –> fib(n+2) is repeated, waste time for repeating calculated.
## 2.2. How it solve well ?
With DP we can store the fib(n+2) and reusing it when needed.
# 3. Mental Models
Always think about DP state first, DP state independence from input state like array. If they differ define the mapping rule between them
