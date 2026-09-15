# Module 6: Recursion and Divide-and-Conquer

**Piscine+ - Advanced C Programming Curriculum**
Duration: 3-4 days | Language: C | Difficulty: Advanced

---

## Learning Objectives

By the end of this module, you will be able to:

1. Think recursively: identify base cases and recursive cases
2. Implement classic recursive algorithms (factorial, Fibonacci, power)
3. Use backtracking to solve constraint problems (N-Queens, maze)
4. Apply divide-and-conquer to solve problems efficiently
5. Optimize recursion with memoization and tail recursion
6. Analyze recursive complexity using recurrence relations

---

## Phase 1: Recursion Fundamentals (Day 1)

### 1.1 The Three Rules of Recursion

1. Base case: every recursive function must have at least one stopping condition
2. Progress toward base case: each recursive call must reduce the problem size
3. Recursive call must be correct: assume the recursive call works and build on it

### 1.2 Classic Examples

    // Factorial: O(n) time, O(n) space (recursion stack)
    int factorial(int n) {
        if (n <= 1) return 1;          // base case
        return n * factorial(n - 1);   // recursive case
    }

    // Fibonacci (naive): O(2^n) time, O(n) space
    int fib(int n) {
        if (n <= 1) return n;
        return fib(n - 1) + fib(n - 2);
    }

    // Power: O(log n) time, O(log n) space
    double power(double base, int exp) {
        if (exp == 0) return 1;
        if (exp < 0) return 1.0 / power(base, -exp);
        double half = power(base, exp / 2);
        return (exp % 2 == 0) ? half * half : half * half * base;
    }

### 1.3 Recursion vs Iteration

| Criterion | Recursion | Iteration |
|-----------|----------|----------|
| Readability | Often cleaner | Can be verbose |
| Performance | Function call overhead | No overhead |
| Stack usage | O(depth) | O(1) |
| Risk | Stack overflow | No risk |

### Exercise Set 1: Basic Recursion

#### Exercise 1.1 - Classic Recursive Functions (Easy)

Problem: Implement recursively: factorial, Fibonacci (naive), power (fast exponentiation), GCD (Euclidean), string length, string reverse, print number in any base.

Acceptance Criteria:
- [ ] Every function has a clear base case
- [ ] No infinite recursion
- [ ] Big-O analysis for time and space

#### Exercise 1.2 - Recursive Data Processing (Medium)

Problem: Write recursive functions to:
- Sum an array recursively
- Find max element recursively
- Check if array is sorted recursively
- Binary search recursively
- Reverse an array recursively

Acceptance Criteria:
- [ ] Each function takes array and indices (not just a pointer)
- [ ] Handles empty arrays
- [ ] Big-O analysis provided

#### Exercise 1.3 - Tower of Hanoi (Medium)

Problem: Solve the Tower of Hanoi puzzle for n disks. Print each move.

Acceptance Criteria:
- [ ] Correct number of moves (2^n - 1)
- [ ] No disk placed on a smaller disk
- [ ] Handles n = 0 and n = 1

Big-O Analysis:
- Time: O(2^n) - exponential, unavoidable
- Space: O(n) - recursion depth

---

## Phase 2: Backtracking (Day 2)

### 2.1 Backtracking Pattern

Backtracking is a systematic way to try all possibilities and undo choices that lead to failure.

    void backtrack(parameters) {
        if (is_solution(parameters)) {
            process_solution(parameters);
            return;
        }
        for (each possible choice) {
            make_choice(parameters);
            backtrack(parameters);  // recurse
            undo_choice(parameters);  // backtrack
        }
    }

### 2.2 Classic Backtracking Problems

- N-Queens: place N queens on an NxN board so no two attack each other
- Maze solving: find a path from start to end
- Sudoku solver: fill a 9x9 grid following Sudoku rules
- Permutations: generate all permutations of a set
- Combinations: generate all k-combinations of n elements
- Subset sum: find subsets that sum to a target

### Exercise Set 2: Backtracking

#### Exercise 2.1 - Permutations and Combinations (Easy to Medium)

Problem: Generate all permutations and k-combinations of an array using backtracking.

Acceptance Criteria:
- [ ] Permutations: n! results, no duplicates
- [ ] Combinations: C(n,k) results
- [ ] Handles k = 0 and k = n
- [ ] No memory leaks

Big-O Analysis:
- Permutations: O(n! * n) time (n! permutations, O(n) to print each)
- Combinations: O(C(n,k) * k) time

#### Exercise 2.2 - N-Queens (Medium to Hard)

Problem: Solve the N-Queens problem. Count all solutions for a given N.

Acceptance Criteria:
- [ ] Correct solution count for N = 1 to 12
- [ ] Prints the board for each solution (optional)
- [ ] Uses backtracking (not brute force)
- [ ] Optimized: uses arrays to track columns and diagonals

Big-O Analysis:
- Time: O(N!) worst case (but pruning makes it much faster in practice)
- Space: O(N) for the recursion stack and tracking arrays

#### Exercise 2.3 - Maze Solver (Hard - Real-World Project)

Problem: Read a maze from a file and find the shortest path using backtracking or BFS.

Maze format:
- '#' = wall, ' ' = path, 'S' = start, 'E' = end

Acceptance Criteria:
- [ ] Reads maze from file
- [ ] Finds a path from S to E
- [ ] Prints the maze with the solution path marked
- [ ] Handles mazes with no solution
- [ ] Finds the shortest path (use BFS, not just any path)

---

## Phase 3: Divide and Conquer (Day 3)

### 3.1 The Divide-and-Conquer Paradigm

1. Divide: split the problem into smaller subproblems
2. Conquer: solve each subproblem recursively
3. Combine: merge the solutions

### 3.2 Classic Problems

- Merge sort (already covered in Module 5)
- Quick sort (already covered in Module 5)
- Maximum subarray (Kadane's vs divide-and-conquer)
- Closest pair of points
- Matrix multiplication (Strassen's algorithm)
- Convex hull

### Exercise Set 3: Divide and Conquer

#### Exercise 3.1 - Maximum Subarray (Medium)

Problem: Find the contiguous subarray with the largest sum.

Implement two approaches:
1. Divide-and-conquer: O(n log n)
2. Kadane's algorithm: O(n)

Acceptance Criteria:
- [ ] Both approaches give correct results
- [ ] Handles all-negative arrays (returns the maximum single element)
- [ ] Handles empty arrays
- [ ] Performance comparison between the two

Big-O Analysis:

| Approach | Time | Space |
|---------|------|-------|
| Divide and conquer | O(n log n) | O(log n) |
| Kadane's | O(n) | O(1) |

#### Exercise 3.2 - Closest Pair of Points (Hard)

Problem: Given n points in a 2D plane, find the pair with the minimum distance.

Acceptance Criteria:
- [ ] Divide-and-conquer: O(n log n) time
- [ ] Brute force: O(n^2) time (for comparison)
- [ ] Handles duplicate points
- [ ] Benchmark both approaches

#### Exercise 3.3 - Strassen's Matrix Multiplication (Hard)

Problem: Implement Strassen's algorithm for matrix multiplication and compare with the standard O(n^3) approach.

---

## Phase 4: Optimization (Day 4)

### 4.1 Memoization

    // Fibonacci with memoization: O(n) time, O(n) space
    int memo[1000] = {0};
    int fib_memo(int n) {
        if (n <= 1) return n;
        if (memo[n] != 0) return memo[n];
        return memo[n] = fib_memo(n - 1) + fib_memo(n - 2);
    }

### 4.2 Tail Recursion

    // Tail-recursive factorial (can be optimized to iteration by compiler)
    int factorial_tail(int n, int accumulator) {
        if (n <= 1) return accumulator;
        return factorial_tail(n - 1, n * accumulator);
    }

### 4.3 Recursion Depth and Stack Overflow

    // Default stack size: ~8MB on Linux
    // Each frame: ~32-64 bytes (varies)
    // Max depth: ~100,000-250,000
    // Solution: convert to iteration or increase stack size

### Exercise Set 4: Optimization

#### Exercise 4.1 - Memoized Fibonacci (Easy)

Problem: Implement Fibonacci with memoization. Compare timing with naive recursive version for n = 40.

Acceptance Criteria:
- [ ] Memoized version runs in O(n) time
- [ ] Naive version takes noticeable time for n >= 40
- [ ] Timing comparison printed

#### Exercise 4.2 - Dynamic Programming Introduction (Medium)

Problem: Solve these classic DP problems:
1. Coin change: minimum number of coins to make a sum
2. Longest common subsequence (LCS)
3. 0/1 Knapsack problem

Acceptance Criteria:
- [ ] Uses memoization or tabulation
- [ ] Correct results for all test cases
- [ ] Big-O analysis for time and space

#### Exercise 4.3 - Sudoku Solver (Hard - Real-World Project)

Problem: Build a Sudoku solver using backtracking. Read a 9x9 grid from a file and solve it.

Acceptance Criteria:
- [ ] Solves any valid Sudoku puzzle
- [ ] Detects unsolvable puzzles
- [ ] Prints the solution in a formatted grid
- [ ] Handles invalid input (non-9x9, invalid characters)

---

## Self-Evaluation and Peer Evaluation

### Self-Evaluation Checklist

- [ ] Every recursive function has a base case
- [ ] No stack overflow for reasonable inputs
- [ ] Big-O analysis includes both time and space
- [ ] Memoization used where appropriate
- [ ] Backtracking solutions are correct and efficient

### Peer Evaluation Form

| Criterion | Score (1-5) | Notes |
|-----------|-------------|-------|
| Base cases are correct | | |
| Backtracking is properly implemented | | |
| Divide-and-conquer solutions are efficient | | |
| Memoization is used where needed | | |
| Big-O analysis is correct | | |

---

## Summary

### What You Learned

- Recursion fundamentals: base case, recursive case, progress
- Classic recursive algorithms: factorial, Fibonacci, power, GCD
- Backtracking: N-Queens, maze solving, permutations, Sudoku
- Divide-and-conquer: max subarray, closest pair, Strassen's
- Optimization: memoization, tail recursion, DP

### What's Next

Module 7: File I/O and System Calls - You will learn file descriptors, read/write system calls, pipes, and redirection, building the foundation for a mini shell.

---

Piscine+ - Module 6: Recursion and Divide-and-Conquer
Built with Hamster