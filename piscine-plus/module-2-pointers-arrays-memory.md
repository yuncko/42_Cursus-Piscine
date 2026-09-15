# Module 2: Pointers, Arrays and Memory

**Piscine+ - Advanced C Programming Curriculum**
Duration: 3-4 days | Language: C | Difficulty: Intermediate

---

## Learning Objectives

By the end of this module, you will be able to:

1. Declare, dereference, and manipulate pointers correctly
2. Understand the relationship between arrays and pointers
3. Implement string functions using char arrays
4. Use dynamic memory allocation: malloc, calloc, realloc, free
5. Detect and fix memory leaks using valgrind
6. Work with double pointers and function pointers
7. Analyze time and space complexity of pointer-based solutions

---

## Phase 1: Pointers Fundamentals (Day 1)

### 1.1 What is a Pointer?

A pointer is a variable that stores the memory address of another variable.

    int x = 42;
    int *ptr = &x;    // ptr holds the address of x

    printf("x = %d\n", x);           // 42
    printf("&x = %p\n", &x);         // address of x
    printf("ptr = %p\n", ptr);       // same as &x
    printf("*ptr = %d\n", *ptr);     // 42 (dereferencing)
    printf("&ptr = %p\n", &ptr);     // address of ptr itself

### 1.2 Pointer Arithmetic

    int arr[5] = {10, 20, 30, 40, 50};
    int *p = arr;     // same as &arr[0]

    printf("%d\n", *p);      // 10
    printf("%d\n", *(p+1));  // 20
    printf("%d\n", *(p+2));  // 30

    p++;                  // p now points to arr[1]
    printf("%d\n", *p);    // 20

    // Pointer difference
    int *q = &arr[4];
    printf("%ld\n", q - p);  // 3 (number of int-sized steps)

### 1.3 Pointer Types and Sizes

| Type | Size on 64-bit | What it points to |
|------|----------------|-------------------|
| int * | 8 bytes | int |
| char * | 8 bytes | char |
| double * | 8 bytes | double |
| void * | 8 bytes | anything (generic) |

Pointer arithmetic scales by the size of the pointed-to type:
- char *ptr: ptr+1 advances 1 byte
- int *ptr: ptr+1 advances 4 bytes
- double *ptr: ptr+1 advances 8 bytes

### Exercise Set 1: Pointers

#### Exercise 1.1 - Swap Two Variables (Easy)

Problem: Write a function void swap(int *a, int *b) that swaps the values of two integers.

Acceptance Criteria:
- [ ] Uses pointer parameters (not return values)
- [ ] Works correctly for all integer values
- [ ] Handles swapping a pointer with itself (swap(&x, &x))

Big-O Analysis:
- Time: O(1)
- Space: O(1)

#### Exercise 1.2 - Pointer Maze (Medium)

Problem: Given an array of integers, write a function that:
- Finds the minimum value using pointer arithmetic (no array indexing)
- Finds the maximum value using pointer arithmetic
- Reverses the array in-place using two pointers

Acceptance Criteria:
- [ ] No [] indexing used - only pointer arithmetic
- [ ] Handles empty arrays
- [ ] Handles single-element arrays
- [ ] Returns both min and max through pointer parameters

Big-O Analysis:
- Find min/max: O(n) time, O(1) space
- Reverse: O(n/2) = O(n) time, O(1) space

#### Exercise 1.3 - Generic Swap (Hard)

Problem: Write a function void generic_swap(void *a, void *b, size_t size) that swaps any two memory regions of the given size.

Acceptance Criteria:
- [ ] Uses memcpy or a byte-by-byte loop with char pointers
- [ ] Works for ints, doubles, structs, arrays
- [ ] Handles size = 0 gracefully
- [ ] No memory allocation (in-place swap using a temp buffer)

---

## Phase 2: Arrays and Strings (Day 2)

### 2.1 Arrays and Pointer Relationship

    int arr[5] = {1, 2, 3, 4, 5};

    // arr decays to a pointer to the first element
    int *p = arr;       // same as int *p = &arr[0];

    // arr[i] is equivalent to *(arr + i)
    // &arr[i] is equivalent to arr + i

    // sizeof difference:
    sizeof(arr);       // 20 (5 * sizeof(int))
    sizeof(p);         // 8 (size of a pointer on 64-bit)

### 2.2 Strings as char Arrays

A C string is a char array terminated by a null byte ('\0').

    char str[] = "Hello";   // 6 bytes: H,e,l,l,o,\0
    char *p = "World";      // string literal (read-only)

    // Common string functions (implement these yourself!)
    size_t strlen(const char *s);
    char *strcpy(char *dst, const char *src);
    char *strcat(char *dst, const char *src);
    int strcmp(const char *s1, const char *s2);
    char *strdup(const char *s);

### 2.3 2D Arrays

    int matrix[3][4] = {
        {1, 2, 3, 4},
        {5, 6, 7, 8},
        {9, 10, 11, 12}
    };

    // Access: matrix[row][col]
    // Memory is contiguous: row-major order
    // matrix[i][j] is at address: base + (i * 4 + j) * sizeof(int)

### Exercise Set 2: Arrays and Strings

#### Exercise 2.1 - String Library (Easy to Medium)

Problem: Reimplement these standard string functions:
- ft_strlen - string length
- ft_strcpy - string copy
- ft_strncpy - bounded string copy
- ft_strcmp - string compare
- ft_strncmp - bounded string compare
- ft_strcat - string concatenation
- ft_strdup - string duplicate (uses malloc)

Acceptance Criteria:
- [ ] All functions match standard behavior exactly
- [ ] No warnings with -Wall -Wextra -Werror
- [ ] ft_strdup uses malloc and returns NULL on failure
- [ ] Each function has at least 3 test cases

Big-O Analysis:
- ft_strlen: O(n) time, O(1) space
- ft_strcpy: O(n) time, O(1) space
- ft_strcmp: O(n) time, O(1) space
- ft_strdup: O(n) time, O(n) space (allocates memory)

#### Exercise 2.2 - Matrix Operations (Medium)

Problem: Implement matrix operations using 2D arrays:
- matrix_add(a, b, result, rows, cols)
- matrix_multiply(a, b, result, a_rows, a_cols, b_cols)
- matrix_transpose(a, result, rows, cols)
- matrix_print(a, rows, cols)

Acceptance Criteria:
- [ ] Handles non-square matrices
- [ ] Validates dimension compatibility for multiplication
- [ ] Uses pointer arithmetic for inner loops (bonus)
- [ ] Prints matrix with aligned columns

Big-O Analysis:
- Add: O(m*n) time, O(1) extra space
- Multiply: O(m*n*p) time, O(1) extra space
- Transpose: O(m*n) time, O(1) extra space (if square, in-place)

#### Exercise 2.3 - Dynamic String Library (Hard - Real-World Project)

Problem: Build a dynamic string library with auto-resizing:
- t_string struct with char *data, size_t len, size_t capacity
- string_create() - initialize
- string_append(str, text) - append text, resize if needed
- string_insert(str, index, text) - insert at position
- string_delete(str, index, length) - delete a range
- string_free(str) - free memory
- Growth strategy: double capacity when full

Acceptance Criteria:
- [ ] No buffer overflow (always checks capacity)
- [ ] Growth factor is 2x (amortized O(1) append)
- [ ] Handles empty strings
- [ ] All memory is freed with string_free
- [ ] No memory leaks (verify with valgrind)

Big-O Analysis:
- Append: O(1) amortized, O(n) worst case (resize)
- Insert: O(n) (shift elements)
- Delete: O(n) (shift elements)
- Space: O(n) where n = string length

---

## Phase 3: Dynamic Memory (Day 3)

### 3.1 Memory Allocation Functions

    // malloc - allocate uninitialized memory
    int *arr = malloc(10 * sizeof(int));
    if (!arr) { /* handle error */ }

    // calloc - allocate zero-initialized memory
    int *arr = calloc(10, sizeof(int));
    // all elements are 0

    // realloc - resize existing allocation
    arr = realloc(arr, 20 * sizeof(int));
    // may move the block, old pointer is invalid

    // free - release memory
    free(arr);
    arr = NULL;  // good practice: avoid dangling pointer

### 3.2 Memory Layout

    +------------------+ High memory
    |    Stack         | (local variables, function frames)
    |      |           |
    |      v           |
    |                  |
    |      ^           |
    |      |           |
    |    Heap          | (malloc, calloc, realloc)
    |    BSS           | (uninitialized globals)
    |    Data          | (initialized globals)
    |    Text          | (program code)
    +------------------+ Low memory

### 3.3 Common Memory Bugs

| Bug | Description | Detection |
|-----|-------------|----------|
| Memory leak | malloc without free | valgrind |
| Dangling pointer | use after free | valgrind |
| Double free | free same pointer twice | valgrind |
| Buffer overflow | write past allocation | valgrind, ASan |
| Use of uninitialized | read before write | valgrind, ASan |

### Exercise Set 3: Dynamic Memory

#### Exercise 3.1 - Dynamic Array (Easy)

Problem: Implement a dynamic array that grows automatically:
- create(size) - allocate initial capacity
- push(arr, value) - add element, resize if needed
- get(arr, index) - get element at index
- free_array(arr) - free all memory

Acceptance Criteria:
- [ ] Growth factor is 2x
- [ ] Handles allocation failure
- [ ] No memory leaks (valgrind clean)
- [ ] Tracks both size and capacity

Big-O Analysis:
- Push: O(1) amortized, O(n) worst case
- Get: O(1)
- Space: O(n)

#### Exercise 3.2 - 2D Dynamic Array (Medium)

Problem: Allocate and free a 2D array dynamically:
- allocate_2d(rows, cols) - returns int**
- free_2d(arr, rows) - frees all rows and the row array
- set(arr, row, col, value)
- get(arr, row, col)

Acceptance Criteria:
- [ ] Each row is allocated separately
- [ ] Free order is correct (rows first, then row array)
- [ ] No memory leaks (valgrind clean)
- [ ] Handles 0x0 arrays

Comparative Analysis (3 Ways to Allocate 2D Array):

| Approach | Pros | Cons |
|---------|------|------|
| Row-by-row malloc | Flexible row sizes | Multiple malloc calls |
| Single malloc + math | One allocation, cache-friendly | Less flexible, manual indexing |
| Array of pointers to columns | Easy to swap rows | Non-contiguous memory |

#### Exercise 3.3 - Memory Pool Allocator (Hard - Real-World Project)

Problem: Build a simple memory pool allocator:
- pool_create(block_size, num_blocks) - pre-allocate a pool
- pool_alloc(pool) - return a block from the pool
- pool_free(pool, block) - return a block to the pool
- pool_destroy(pool) - free the entire pool
- Use a free list to track available blocks

Acceptance Criteria:
- [ ] O(1) allocation and deallocation
- [ ] No fragmentation (fixed block size)
- [ ] No memory leaks (valgrind clean)
- [ ] Handles pool exhaustion gracefully

---

## Phase 4: Advanced Pointers (Day 4)

### 4.1 Double Pointers

    int x = 42;
    int *p = &x;
    int **pp = &p;

    printf("%d\n", **pp);  // 42

    // Use case: modifying a pointer in a function
    void allocate_string(char **str) {
        *str = malloc(100);
        strcpy(*str, "Hello");
    }

    char *mystr;
    allocate_string(&mystr);
    printf("%s\n", mystr);  // Hello
    free(mystr);

### 4.2 Function Pointers

    // Declaration
    int (*compare)(const void *, const void *);

    // Assignment
    int ascending(const void *a, const void *b) {
        return *(int*)a - *(int*)b;
    }
    compare = ascending;

    // Use with qsort
    int arr[5] = {5, 3, 1, 4, 2};
    qsort(arr, 5, sizeof(int), compare);

    // Array of function pointers (dispatch table)
    int (*operations[5])(int, int) = {add, sub, mul, div, mod};
    int result = operations[0](10, 3);  // add(10, 3) = 13

### 4.3 Valgrind Usage

    # Compile with -g for debug symbols
    gcc -g -Wall -Wextra program.c -o program

    # Run with valgrind
    valgrind --leak-check=full --show-leak-kinds=all ./program

    # Expected output:
    # HEAP SUMMARY:
    #     in use at exit: 0 bytes in 0 blocks
    #   total heap usage: 5 allocs, 5 frees, 1,200 bytes allocated
    # All heap blocks were freed -- no leaks are possible

### Exercise Set 4: Advanced Pointers

#### Exercise 4.1 - Generic Sort (Medium)

Problem: Write a generic sort function that uses function pointers for comparison. Sort arrays of ints, floats, and strings.

Acceptance Criteria:
- [ ] Uses void * and function pointers
- [ ] Works with any data type
- [ ] Comparison function is passed as parameter
- [ ] No type-specific code in the sort function

#### Exercise 4.2 - Command Dispatch Table (Hard - Real-World Project)

Problem: Build a command dispatcher using function pointers:
- Define a struct: typedef struct { char *name; void (*handler)(int argc, char **argv); } command_t;
- Register commands in a table
- Parse user input and dispatch to the correct handler
- Implement at least 5 commands: help, echo, calc, time, exit

Acceptance Criteria:
- [ ] Uses an array of function pointers
- [ ] Handles unknown commands
- [ ] Each handler receives argc and argv
- [ ] Clean exit frees all resources

---

## Self-Evaluation and Peer Evaluation

### Self-Evaluation Checklist

- [ ] All malloc calls have NULL checks
- [ ] All allocated memory is freed (valgrind clean)
- [ ] No dangling pointers (set to NULL after free)
- [ ] No buffer overflows (check bounds before writing)
- [ ] Big-O analysis provided for every exercise
- [ ] Edge cases tested: NULL input, empty arrays, zero size

### Peer Evaluation Form

| Criterion | Score (1-5) | Notes |
|-----------|-------------|-------|
| Pointer usage is correct | | |
| Memory management is clean | | |
| Valgrind shows no leaks | | |
| Edge cases are handled | | |
| Big-O analysis is correct | | |
| Code is readable and safe | | |

---

## Summary

### What You Learned

- Pointers: declaration, dereferencing, pointer arithmetic
- Arrays and their relationship to pointers
- Strings as char arrays and reimplemented string functions
- Dynamic memory: malloc, calloc, realloc, free
- Memory leak detection with valgrind
- Double pointers and function pointers

### What's Next

Module 3: String Manipulation and Algorithms - You will learn string parsing, pattern matching, text processing, and build a mini JSON parser in C.

---

Piscine+ - Module 2: Pointers, Arrays and Memory
Built with Hamster