# Module 1: C Fundamentals and I/O

**Piscine+ - Advanced C Programming Curriculum**
Duration: 3-4 days | Language: C | Difficulty: Beginner to Intermediate

---

## Learning Objectives

By the end of this module, you will be able to:

1. Understand the C compilation pipeline: preprocessing, compilation, assembly, linking
2. Use C variables, types, operators, and expressions correctly
3. Implement control flow: if/else, switch, loops (while, for, do-while)
4. Write and call functions with parameters and return values
5. Use basic I/O: printf, scanf, read, write system calls
6. Handle command-line arguments (argc, argv)
7. Create and use header files with #include directives
8. Analyze time and space complexity of every solution

---

## Phase 1: The C Compilation Pipeline (Day 1)

### 1.1 From Source to Binary

C code goes through four stages before becoming an executable:

    Source (.c) -> Preprocessor -> Compiler -> Assembler -> Linker -> Executable
                       (.i)          (.s)        (.o)       (binary)

    # See each stage
    gcc -E main.c -o main.i     # Preprocessing (expands macros, includes headers)
    gcc -S main.i -o main.s     # Compilation (generates assembly)
    gcc -c main.s -o main.o     # Assembly (generates object file)
    gcc main.o -o main           # Linking (creates executable)

    # Or all at once
    gcc -Wall -Wextra main.c -o main

### 1.2 Key Compiler Flags

| Flag | Purpose |
|------|---------|
| -Wall | Enable most warnings |
| -Wextra | Enable extra warnings |
| -Werror | Treat warnings as errors |
| -g | Include debug symbols |
| -O2 | Optimization level 2 |
| -I path | Add include directory |
| -o file | Output file name |

### Exercise Set 1: Compilation

#### Exercise 1.1 - Hello, World (Easy)

Problem: Write a C program that prints "Hello, World!" to stdout. Compile it using each stage separately.

Acceptance Criteria:
- [ ] Program compiles with -Wall -Wextra -Werror with zero warnings
- [ ] Output is exactly Hello, World! followed by newline
- [ ] Returns 0 from main
- [ ] You can explain what each compilation stage does

Big-O Analysis:
- Time: O(1) - constant time, single output operation
- Space: O(1) - constant space, fixed string

#### Exercise 1.2 - Preprocessor Explorer (Medium)

Problem: Write a C program that uses macros, #include, #ifdef, and #ifndef. Compile with -E and examine the preprocessed output.

Requirements:
- Define a macro MAX(a, b) that returns the maximum of two values
- Use #ifdef DEBUG to conditionally print debug info
- Include a custom header file
- Examine the .i file and explain what the preprocessor did

Acceptance Criteria:
- [ ] Macro works correctly for integers and floats
- [ ] #ifdef DEBUG controls debug output
- [ ] Preprocessed output is examined and understood
- [ ] No macro side effects (e.g., double evaluation in MAX)

Common Mistakes:
1. Macro double evaluation: MAX(i++, j++) increments twice
2. Missing parentheses in macro definitions
3. Forgetting #ifndef guards in header files

#### Exercise 1.3 - Build System (Hard)

Problem: Create a Makefile that compiles a multi-file C project with proper dependency tracking.

---

## Phase 2: Variables, Types and Operators (Day 1-2)

### 2.1 Primitive Types

| Type | Size (bytes) | Range |
|------|-------------|-------|
| char | 1 | -128 to 127 (or 0 to 255) |
| unsigned char | 1 | 0 to 255 |
| short | 2 | -32,768 to 32,767 |
| int | 4 | -2,147,483,648 to 2,147,483,647 |
| unsigned int | 4 | 0 to 4,294,967,295 |
| long | 8 | -2^63 to 2^63-1 |
| float | 4 | ~7 decimal digits |
| double | 8 | ~15 decimal digits |

### 2.2 Operators

    // Arithmetic: + - * / %
    int a = 10, b = 3;
    int quotient = a / b;    // 3 (integer division)
    int remainder = a % b;  // 1

    // Comparison: == != < > <= >=
    // Logical: && || !
    // Bitwise: & | ^ ~ << >>

    // Bitwise examples
    int x = 0b1010;  // 10
    int y = 0b1100;  // 12
    printf("%d & %d = %d\n", x, y, x & y);  // 8  (1000)
    printf("%d | %d = %d\n", x, y, x | y);  // 14 (1110)
    printf("%d ^ %d = %d\n", x, y, x ^ y);  // 6  (0110)
    printf("~%d = %d\n", x, ~x);            // -11 (two's complement)
    printf("%d << 2 = %d\n", x, x << 2);    // 40 (101000)

### Exercise Set 2: Types and Operators

#### Exercise 2.1 - Type Sizes (Easy)

Problem: Write a program that prints the size of each primitive type using sizeof.

Acceptance Criteria:
- [ ] Uses sizeof operator (not hardcoded values)
- [ ] Output is formatted as a table
- [ ] Uses printf with proper format specifiers (%zu for size_t)

#### Exercise 2.2 - Bitwise Calculator (Medium)

Problem: Write a program that takes two integers and an operation (&, |, ^, <<, >>, ~) and prints the result in binary, decimal, and hexadecimal.

Acceptance Criteria:
- [ ] Handles all 6 bitwise operations
- [ ] Binary output is padded to 32 bits
- [ ] Handles negative numbers correctly (two's complement)
- [ ] Input validation for operation choice

Big-O Analysis:
- Time: O(1) - all operations are constant time
- Space: O(1) - fixed number of variables

Comparative Analysis (3 Ways to Print Binary):

| Approach | Pros | Cons |
|---------|------|------|
| Bit-by-bit loop | Simple, educational | O(32) iterations |
| Lookup table | Fast, O(1) per nibble | Uses extra memory |
| itoa with base 2 | Reusable | Non-standard, may not be available |

#### Exercise 2.3 - Base Converter (Hard)

Problem: Write a program that converts a number from one base to another (supports bases 2-16).

Acceptance Criteria:
- [ ] Handles bases 2, 8, 10, 16
- [ ] Handles large numbers (up to unsigned long long)
- [ ] Validates input (digits valid for the source base)
- [ ] Prints result in all supported bases

---

## Phase 3: Control Flow (Day 2)

### 3.1 Conditionals

    // if-else
    if (score >= 90) {
        grade = 'A';
    } else if (score >= 80) {
        grade = 'B';
    } else {
        grade = 'F';
    }

    // switch (only for integers/chars)
    switch (operation) {
        case '+': result = a + b; break;
        case '-': result = a - b; break;
        case '*': result = a * b; break;
        case '/':
            if (b == 0) {
                fprintf(stderr, "Error: Division by zero\n");
                return 1;
            }
            result = a / b;
            break;
        default:
            fprintf(stderr, "Error: Unknown operation\n");
            return 1;
    }

### 3.2 Loops

    // while
    int i = 0;
    while (i < 10) {
        printf("%d ", i);
        i++;
    }

    // for
    for (int i = 0; i < 10; i++) {
        printf("%d ", i);
    }

    // do-while (runs at least once)
    int choice;
    do {
        printf("Enter choice (1-5): ");
        scanf("%d", &choice);
    } while (choice < 1 || choice > 5);

### Exercise Set 3: Control Flow

#### Exercise 3.1 - FizzBuzz (Easy)

Problem: Print numbers 1-100. For multiples of 3, print "Fizz". For multiples of 5, print "Buzz". For multiples of both, print "FizzBuzz".

Acceptance Criteria:
- [ ] Correct output for all 100 numbers
- [ ] No division by zero
- [ ] Handles edge case: 0 and negative numbers (if extended)

Big-O Analysis:
- Time: O(n) where n = 100
- Space: O(1)

#### Exercise 3.2 - Multiplication Table (Medium)

Problem: Write a program that takes an integer n and prints an n x n multiplication table with aligned columns.

Acceptance Criteria:
- [ ] Columns are right-aligned
- [ ] Handles n from 1 to 20
- [ ] Handles invalid input (0, negative, non-numeric)
- [ ] Column width adjusts to the largest number

Big-O Analysis:
- Time: O(n^2) - nested loop
- Space: O(1) - no extra storage

#### Exercise 3.3 - Prime Number Checker (Hard)

Problem: Write a program that checks if a number is prime. Then extend it to find all primes up to n using:
1. Trial division
2. Sieve of Eratosthenes
3. Compare performance

Acceptance Criteria:
- [ ] Both algorithms work correctly
- [ ] Handles edge cases: 0, 1, 2, negative numbers
- [ ] Performance comparison with timing
- [ ] Big-O analysis for both approaches

Big-O Analysis:

| Algorithm | Time | Space |
|-----------|------|-------|
| Trial division | O(n*sqrt(n)) | O(1) |
| Sieve of Eratosthenes | O(n log log n) | O(n) |

Error Analysis:
- Forgetting that 2 is prime
- Integer overflow in i * i <= n check (use i <= n / i instead)
- Not handling n < 2 cases

---

## Phase 4: Functions and I/O (Day 3)

### 4.1 Functions

    // Declaration (prototype)
    int add(int a, int b);

    // Definition
    int add(int a, int b) {
        return a + b;
    }

    // Function with no return value
    void print_welcome(const char *name) {
        printf("Welcome, %s!\n", name);
    }

    // Function with no parameters
    int get_random(void) {
        return rand() % 100;
    }

### 4.2 Input/Output

    // printf format specifiers
    printf("%d\n", 42);        // Integer
    printf("%f\n", 3.14);      // Float
    printf("%c\n", 'A');       // Character
    printf("%s\n", "Hello");   // String
    printf("%p\n", &x);        // Pointer
    printf("%x\n", 255);       // Hexadecimal
    printf("%5d\n", 42);       // Width 5, right-aligned
    printf("%-5d|\n", 42);     // Width 5, left-aligned
    printf("%.2f\n", 3.14159); // 2 decimal places

    // scanf
    int age;
    printf("Enter your age: ");
    scanf("%d", &age);

    // read/write system calls
    #include <unistd.h>
    char buf[1024];
    ssize_t bytes = read(STDIN_FILENO, buf, sizeof(buf));
    write(STDOUT_FILENO, buf, bytes);

### 4.3 Command-Line Arguments

    int main(int argc, char *argv[]) {
        if (argc < 2) {
            fprintf(stderr, "Usage: %s <input>\n", argv[0]);
            return 1;
        }
        printf("Program: %s\n", argv[0]);
        printf("Arguments: %d\n", argc - 1);
        for (int i = 1; i < argc; i++) {
            printf("  argv[%d] = %s\n", i, argv[i]);
        }
        return 0;
    }

### Exercise Set 4: Functions and I/O

#### Exercise 4.1 - CLI Calculator (Easy to Medium)

Problem: Write a CLI calculator that takes two numbers and an operation as command-line arguments.

    $ ./calc 10 + 5
    15
    $ ./calc 20 / 4
    5

Acceptance Criteria:
- [ ] Supports +, -, *, /, %
- [ ] Handles division by zero
- [ ] Handles invalid arguments (non-numeric, wrong count)
- [ ] Uses functions for each operation
- [ ] Returns 0 on success, 1 on error

Big-O Analysis:
- Time: O(1) - all operations are constant time
- Space: O(1)

#### Exercise 4.2 - File Statistics (Medium - Real-World Project)

Problem: Write a program that reads a file and reports:
- Number of lines
- Number of words
- Number of characters
- Number of digits
- Number of uppercase/lowercase letters

Acceptance Criteria:
- [ ] Takes filename as command-line argument
- [ ] Handles file not found error
- [ ] Handles empty files
- [ ] Uses read() system call (not fscanf)
- [ ] Output is formatted as a table

Big-O Analysis:
- Time: O(n) where n = file size in bytes
- Space: O(1) - processes one character at a time

#### Exercise 4.3 - Interactive Number Guessing Game (Hard)

Problem: Write a number guessing game where:
- The program picks a random number 1-100
- The user guesses, and gets "too high" / "too low" / "correct"
- Track number of attempts
- Option to play again
- High score tracking (stored in a file)

Acceptance Criteria:
- [ ] Uses rand() with srand(time(NULL))
- [ ] Input validation (handles non-numeric input)
- [ ] Play again loop
- [ ] High score saved to file
- [ ] Displays high score at start

---

## Phase 5: Header Files and Project Structure (Day 4)

### 5.1 Header Files

    // myheader.h
    #ifndef MYHEADER_H
    #define MYHEADER_H

    // Function prototypes
    int my_function(int a, int b);
    void my_helper(void);

    // Constants
    #define MAX_SIZE 100
    #define PI 3.14159265358979

    // Type definitions
    typedef struct {
        int x;
        int y;
    } Point;

    #endif // MYHEADER_H

### 5.2 Project Structure

    project/
    |-- includes/
    |   |-- myheader.h
    |-- src/
    |   |-- main.c
    |   |-- utils.c
    |-- Makefile
    |-- README.md

### Exercise Set 5: Project Structure

#### Exercise 5.1 - Multi-File Project (Medium)

Problem: Create a project with:
- includes/utils.h - function declarations
- src/utils.c - function definitions
- src/main.c - main program using utils functions
- Makefile - builds the project

Acceptance Criteria:
- [ ] Header file has include guards
- [ ] Functions are declared in header, defined in .c file
- [ ] Makefile compiles each .c separately and links them
- [ ] make clean removes all build artifacts
- [ ] No compiler warnings with -Wall -Wextra -Werror

#### Exercise 5.2 - Personal Library (Hard - Real-World Project)

Problem: Build a personal C library (libft) with reimplemented standard functions:
- ft_putchar, ft_putstr, ft_putnbr
- ft_strlen, ft_strcmp, ft_strcpy, ft_strdup
- ft_atoi, ft_itoa
- ft_memset, ft_memcpy
- Create a header file libft.h with all declarations
- Create a Makefile that builds a static library (libft.a)

Acceptance Criteria:
- [ ] All functions match the behavior of their standard counterparts
- [ ] Header file has include guards
- [ ] Makefile produces libft.a using ar
- [ ] Each function has a corresponding test file
- [ ] All tests pass

---

## Self-Evaluation and Peer Evaluation

### Self-Evaluation Checklist

- [ ] All programs compile with -Wall -Wextra -Werror without warnings
- [ ] All functions have proper prototypes in header files
- [ ] All input is validated (no undefined behavior)
- [ ] Big-O analysis is provided for every exercise
- [ ] Edge cases are tested and documented
- [ ] Error messages are printed to stderr
- [ ] Return codes are correct (0 = success, non-zero = error)

### Peer Evaluation Form

| Criterion | Score (1-5) | Notes |
|-----------|-------------|-------|
| Code compiles cleanly | | |
| Functions are well-structured | | |
| Input validation is thorough | | |
| Big-O analysis is correct | | |
| Error handling is comprehensive | | |
| Header files are correct | | |
| Makefile is complete | | |
| Code is readable and commented | | |

---

## Summary

### What You Learned

- The C compilation pipeline (preprocessing, compilation, assembly, linking)
- Variables, types, operators (including bitwise)
- Control flow: conditionals, loops, switch
- Functions: declaration, definition, parameters, return values
- I/O: printf, scanf, read, write
- Command-line arguments (argc, argv)
- Header files and project structure

### What's Next

Module 2: Pointers, Arrays and Memory - You will learn the most powerful and dangerous feature of C: pointers. You will understand memory layout, dynamic allocation, and how to avoid memory leaks.

---

Piscine+ - Module 1: C Fundamentals and I/O
Built with Hamster