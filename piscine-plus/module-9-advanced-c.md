# Module 9: Advanced C - Optimization and Systems

**Piscine+ - Advanced C Programming Curriculum**
Duration: 3-4 days | Language: C | Difficulty: Expert

---

## Learning Objectives

By the end of this module, you will be able to:

1. Use advanced bit manipulation tricks for performance
2. Understand and use inline assembly in C
3. Profile code with gprof, perf, and valgrind/callgrind
4. Apply optimization techniques: loop unrolling, cache optimization, branch prediction
5. Understand memory alignment and padding
6. Use compiler intrinsics and builtins
7. Write multi-threaded programs with pthreads
8. Analyze performance bottlenecks systematically

---

## Phase 1: Bit Manipulation Mastery (Day 1)

### 1.1 Essential Bit Tricks

    // Check if a number is even
    (n & 1) == 0

    // Check if a number is a power of 2
    (n & (n - 1)) == 0  // true for powers of 2 (and 0)

    // Count set bits (popcount)
    int popcount(unsigned int n) {
        int count = 0;
        while (n) { count += n & 1; n >>= 1; }
        return count;
    }
    // Or use builtin: __builtin_popcount(n)

    // Get the lowest set bit
    n & (-n)  // isolates the lowest set bit

    // Clear the lowest set bit
    n & (n - 1)  // clears the lowest set bit

    // Swap two variables without temp
    a ^= b; b ^= a; a ^= b;

    // Get absolute value without branching
    int abs(int n) {
        int mask = n >> 31;
        return (n ^ mask) - mask;
    }

### 1.2 Bit Fields and Flags

    // Define flags
    #define FLAG_READ    0x01  // 0000 0001
    #define FLAG_WRITE   0x02  // 0000 0010
    #define FLAG_EXECUTE 0x04  // 0000 0100
    #define FLAG_ALL     0x07  // 0000 0111

    // Set flag
    flags |= FLAG_READ;

    // Clear flag
    flags &= ~FLAG_READ;

    // Toggle flag
    flags ^= FLAG_READ;

    // Check flag
    if (flags & FLAG_READ) { /* read is set */ }

### Exercise Set 1: Bit Manipulation

#### Exercise 1.1 - Bit Tricks Library (Easy to Medium)

Problem: Implement a library of bit manipulation functions:
- popcount, trailing_zeros, leading_zeros
- is_power_of_two, next_power_of_two, prev_power_of_two
- reverse_bits, rotate_left, rotate_right
- swap_bits, set_bit, clear_bit, toggle_bit, get_bit

Acceptance Criteria:
- [ ] All functions work correctly for 32-bit and 64-bit integers
- [ ] Uses __builtin_* where available
- [ ] Handles edge cases: 0, UINT_MAX, negative numbers
- [ ] Each function has at least 5 test cases

Big-O Analysis:
- All operations: O(1) time, O(1) space

#### Exercise 1.2 - Bitwise Compression (Medium - Real-World Project)

Problem: Implement a bit-packed array that stores booleans using 1 bit per element instead of 1 byte.
- bit_array_create(size)
- bit_array_set(arr, index, value)
- bit_array_get(arr, index)
- bit_array_free(arr)
- Calculate memory savings vs bool array

Acceptance Criteria:
- [ ] Uses 1 bit per boolean (8x memory savings)
- [ ] Handles arbitrary sizes
- [ ] No memory leaks
- [ ] Benchmark: compare memory usage with bool array

#### Exercise 1.3 - Bloom Filter (Hard - Real-World Project)

Problem: Implement a Bloom filter for fast set membership testing:
- Uses multiple hash functions
- Bit array for storage
- Supports add and check operations
- Calculate false positive rate

Acceptance Criteria:
- [ ] Uses 3+ hash functions
- [ ] Handles large datasets (1M+ elements)
- [ ] Reports false positive rate
- [ ] No memory leaks

---

## Phase 2: Memory and Performance (Day 2)

### 2.1 Memory Alignment

    // Struct with padding
    struct Bad {
        char c;     // 1 byte + 3 padding
        int i;      // 4 bytes
        char d;     // 1 byte + 3 padding
    };  // Total: 12 bytes

    // Reordered for better packing
    struct Good {
        int i;      // 4 bytes
        char c;     // 1 byte
        char d;     // 1 byte + 2 padding
    };  // Total: 8 bytes

    // Use __attribute__((packed)) to remove padding (performance penalty)
    struct __attribute__((packed)) Packed {
        char c;
        int i;
    };  // Total: 5 bytes (but slower access)

### 2.2 Cache Optimization

    // Row-major traversal (cache-friendly)
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            matrix[i][j] = 0;

    // Column-major traversal (cache-unfriendly)
    for (int j = 0; j < cols; j++)
        for (int i = 0; i < rows; i++)
            matrix[i][j] = 0;  // Much slower for large matrices

### 2.3 Loop Optimization

    // Loop unrolling (manual)
    for (int i = 0; i < n; i += 4) {
        arr[i] = 0;
        arr[i+1] = 0;
        arr[i+2] = 0;
        arr[i+3] = 0;
    }
    // Compiler can do this with -O3 -funroll-loops

### Exercise Set 2: Memory and Performance

#### Exercise 2.1 - Struct Optimizer (Medium)

Problem: Given a struct definition, write a tool that:
- Calculates the size with padding
- Suggests a reordered version with minimal padding
- Shows the memory savings

Acceptance Criteria:
- [ ] Correctly calculates alignment and padding
- [ ] Handles nested structs
- [ ] Handles unions
- [ ] Shows before/after comparison

#### Exercise 2.2 - Matrix Cache Benchmark (Medium - Real-World Project)

Problem: Write two matrix multiplication functions:
1. Cache-friendly (blocked/tiled multiplication)
2. Naive (row-major without blocking)
Benchmark both on large matrices (1000x1000+).

Acceptance Criteria:
- [ ] Correct results from both implementations
- [ ] Timing comparison shows significant difference
- [ ] Explains why cache-friendly is faster
- [ ] Tests with different block sizes (16, 32, 64)

---

## Phase 3: Profiling and Optimization (Day 3)

### 3.1 Profiling Tools

    # Compile with profiling
    gcc -pg -O2 program.c -o program
    ./program
    gprof program gmon.out > profile.txt

    # Using perf (Linux)
    perf record ./program
    perf report

    # Using valgrind/callgrind
    valgrind --tool=callgrind ./program
    callgrind_annotate callgrind.out.*

### 3.2 Compiler Optimization Levels

| Level | What it does |
|-------|-------------|
| -O0 | No optimization (default for debugging) |
| -O1 | Basic optimizations |
| -O2 | Standard optimizations (recommended) |
| -O3 | Aggressive optimizations (may increase code size) |
| -Os | Optimize for size |
| -Ofast | -O3 + non-standard math optimizations |

### 3.3 Common Optimizations

- Dead code elimination
- Constant folding and propagation
- Loop unrolling
- Function inlining
- Common subexpression elimination
- Strength reduction (e.g., x*4 -> x<<2)
- Branch prediction hints

### Exercise Set 3: Profiling

#### Exercise 3.1 - Profile and Optimize (Medium to Hard)

Problem: Write a program that does heavy computation (e.g., matrix multiplication, sorting large arrays). Profile it, identify the bottleneck, and optimize.

Acceptance Criteria:
- [ ] Profile output shows where time is spent
- [ ] At least 2 optimizations applied
- [ ] Before/after timing comparison
- [ ] Explanation of each optimization

#### Exercise 3.2 - Benchmark Suite (Hard - Real-World Project)

Problem: Build a benchmarking suite that:
- Runs multiple algorithms with different input sizes
- Measures CPU time, wall time, and memory usage
- Generates a report with charts (CSV output for external tools)
- Compares -O0, -O1, -O2, -O3 performance

---

## Phase 4: Multi-threading with pthreads (Day 4)

### 4.1 pthreads Basics

    #include <pthread.h>

    void *worker(void *arg) {
        int *result = malloc(sizeof(int));
        *result = *(int *)arg * 2;
        return result;
    }

    int main() {
        pthread_t thread;
        int input = 42;
        pthread_create(&thread, NULL, worker, &input);
        int *result;
        pthread_join(thread, (void **)&result);
        printf("Result: %d\n", *result);
        free(result);
        return 0;
    }

### 4.2 Mutex and Synchronization

    pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;
    int counter = 0;

    void *increment(void *arg) {
        for (int i = 0; i < 1000000; i++) {
            pthread_mutex_lock(&lock);
            counter++;
            pthread_mutex_unlock(&lock);
        }
        return NULL;
    }

### Exercise Set 4: Multi-threading

#### Exercise 4.1 - Parallel Sum (Medium)

Problem: Write a program that sums a large array using multiple threads. Each thread sums a portion and the main thread combines results.

Acceptance Criteria:
- [ ] Correct sum (matches single-threaded result)
- [ ] No race conditions (use mutex or separate partial sums)
- [ ] Speedup compared to single-threaded version
- [ ] Handles different thread counts (1, 2, 4, 8)

#### Exercise 4.2 - Thread Pool (Hard - Real-World Project)

Problem: Build a thread pool that:
- Has a fixed number of worker threads
- Accepts tasks from a queue
- Workers pick up tasks and execute them
- Supports graceful shutdown
- Uses condition variables for task notification

Acceptance Criteria:
- [ ] No race conditions
- [ ] No deadlocks
- [ ] Graceful shutdown (all tasks complete before exit)
- [ ] No memory leaks
- [ ] Benchmark: show speedup vs single-threaded

---

## Self-Evaluation and Peer Evaluation

### Self-Evaluation Checklist

- [ ] All bit manipulation functions are correct
- [ ] Profiling results are analyzed and explained
- [ ] Optimizations show measurable improvement
- [ ] Multi-threaded programs have no race conditions
- [ ] No memory leaks in any program
- [ ] Big-O analysis includes cache complexity

### Peer Evaluation Form

| Criterion | Score (1-5) | Notes |
|-----------|-------------|-------|
| Bit manipulation is correct | | |
| Profiling is thorough | | |
| Optimizations are effective | | |
| Thread safety is ensured | | |
| Code quality | | |

---

## Summary

### What You Learned

- Advanced bit manipulation tricks and patterns
- Memory alignment, padding, and cache optimization
- Profiling with gprof, perf, and callgrind
- Compiler optimization levels and their effects
- Multi-threaded programming with pthreads
- Thread pools, mutexes, and condition variables

### What's Next

Final Exams - Test everything you have learned across all 10 modules with a comprehensive practical assessment.

---

Piscine+ - Module 9: Advanced C - Optimization and Systems
Built with Hamster