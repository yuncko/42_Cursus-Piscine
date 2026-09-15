# Module 8: Mini-Projects - Building Real Tools

**Piscine+ - Advanced C Programming Curriculum**
Duration: 3-4 days | Language: C | Difficulty: Advanced (Capstone)

---

## Learning Objectives

By the end of this module, you will be able to:

1. Combine all previous skills into complete, working programs
2. Build a mini shell with command execution, pipes, and redirection
3. Build a calculator with expression parsing and evaluation
4. Build a file manager with directory traversal and file operations
5. Build a JSON parser with full type support and error handling
6. Structure a C project with proper modularity and headers
7. Write comprehensive tests for your own code

---

## Project 1: Mini Shell (Day 1-2)

### Overview
Build a simplified Unix shell that can execute commands, handle pipes, and redirect I/O.

### Requirements

1. Command parsing: tokenize input into command + arguments
2. Command execution: use fork + execvp to run commands
3. Built-in commands: cd, exit, pwd, echo, export, unset
4. Pipe operator: cmd1 | cmd2 | cmd3
5. Redirection: >, <, >>, 2>
6. Background execution: cmd &
7. Command history: store and recall previous commands
8. Signal handling: Ctrl+C does not exit the shell

### Architecture

    // Tokenizer: split input into tokens
    typedef struct {
        char **tokens;
        int count;
    } t_tokens;

    // Command: single command with args and I/O settings
    typedef struct {
        char **args;
        char *input_file;
        char *output_file;
        int append_mode;
        int background;
    } t_command;

    // Pipeline: array of commands connected by pipes
    typedef struct {
        t_command *commands;
        int count;
    } t_pipeline;

### Acceptance Criteria

- [ ] Executes simple commands: ls, cat, echo
- [ ] Handles pipes: ls -la | grep .c | wc -l
- [ ] Handles redirection: ls > files.txt
- [ ] Handles background: sleep 10 &
- [ ] Built-in cd works correctly
- [ ] No zombie processes
- [ ] No file descriptor leaks
- [ ] Handles Ctrl+C gracefully
- [ ] Command history works
- [ ] No memory leaks (valgrind clean)

Big-O Analysis:
- Parsing: O(n) where n = input length
- Execution: depends on the command being run
- Memory: O(n) for tokens and command structures

### Testing

Test your shell with:
- Simple commands: ls, pwd, echo hello
- Pipes: ls | wc -l
- Redirection: echo hello > file.txt; cat file.txt
- Multiple pipes: cat file | grep pattern | sort | uniq -c
- Background: sleep 100 &
- Error cases: nonexistent command, permission denied
- Edge cases: empty input, multiple spaces, trailing pipe

---

## Project 2: Expression Calculator (Day 2)

### Overview
Build a calculator that parses and evaluates arithmetic expressions with proper operator precedence.

### Requirements

1. Tokenize input: numbers, operators, parentheses
2. Parse using recursive descent or shunting-yard algorithm
3. Evaluate with correct operator precedence: +, -, *, /, %, ^
4. Support parentheses for grouping
5. Support variables: x = 5; x + 3
6. Support functions: sin, cos, sqrt, pow
7. Error handling: division by zero, syntax errors, undefined variables

### Architecture

    typedef enum {
        TOKEN_NUMBER, TOKEN_OPERATOR, TOKEN_LPAREN,
        TOKEN_RPAREN, TOKEN_VARIABLE, TOKEN_FUNCTION, TOKEN_EOF
    } token_type;

    typedef enum { NODE_NUMBER, NODE_BINARY_OP, NODE_UNARY_OP, NODE_VARIABLE } node_type;

    typedef struct ast_node {
        node_type type;
        union {
            double number;
            struct { char op; struct ast_node *left, *right; } binary;
            struct { char op; struct ast_node *operand; } unary;
            char *var_name;
        };
    } ast_node;

### Acceptance Criteria

- [ ] Correct operator precedence
- [ ] Handles parentheses
- [ ] Handles unary minus: -5 + 3
- [ ] Handles division by zero (error message, not crash)
- [ ] Handles syntax errors: 3 + * 4
- [ ] Variables work: x = 5; x * 2 = 10
- [ ] Functions work: sqrt(16) = 4
- [ ] No memory leaks (AST is freed after evaluation)

---

## Project 3: File Manager (Day 3)

### Overview
Build a command-line file manager that can navigate, list, search, and manage files.

### Requirements

1. Navigate: cd, pwd, ls with options (-l, -a, -R)
2. File operations: create, delete, copy, move, rename
3. Search: find by name, find by content (grep-like)
4. Directory tree: print a visual tree of the directory structure
5. File info: size, permissions, modification time, type
6. Batch operations: copy directory recursively, delete with pattern
7. Trash system: move to trash instead of permanent delete

### Acceptance Criteria

- [ ] All commands work correctly
- [ ] Handles permission errors gracefully
- [ ] Handles paths with spaces
- [ ] Handles symlinks (follow or skip, documented)
- [ ] No memory leaks
- [ ] Directory tree is visually formatted
- [ ] Search is efficient (uses find-like approach)

---

## Project 4: JSON Parser (Day 3-4)

### Overview
Build a complete JSON parser that can parse, validate, and pretty-print JSON files.

### Requirements

1. Parse all JSON types: object, array, string, number, boolean, null
2. Handle escape sequences: \n, \t, \", \\, \uXXXX
3. Handle nested structures (objects in arrays, arrays in objects)
4. Pretty-print with configurable indentation
5. Query API: get value by key, get array element by index
6. Validation: detect and report errors with line/column
7. File I/O: read JSON from file, write to file

### Acceptance Criteria

- [ ] Parses all standard JSON types
- [ ] Handles deeply nested structures (100+ levels)
- [ ] Handles all escape sequences
- [ ] Error messages include line and column numbers
- [ ] Pretty-print with 2-space or 4-space indentation
- [ ] Query API works: json_get(obj, "key.subkey[0].name")
- [ ] No memory leaks (valgrind clean)
- [ ] Handles large files (1MB+)

Big-O Analysis:
- Parsing: O(n) time, O(d) space (d = nesting depth)
- Querying: O(k) where k = key path length
- Memory: O(n) for the parsed tree

---

## Integration and Testing (Day 4)

### Test-Driven Development

For each project, write tests BEFORE implementing:

1. Unit tests: test each function in isolation
2. Integration tests: test the full program with real input
3. Edge case tests: empty input, NULL, very large input, malformed input
4. Memory tests: run with valgrind and verify no leaks
5. Performance tests: measure time on large inputs

### Test Framework

    #define TEST(name) void name(int *passed, int *failed)
    #define ASSERT(cond) do { \
        if (cond) { (*passed)++; } \
        else { (*failed)++; printf("FAIL: %s:%d\n", __FILE__, __LINE__); } \
    } while (0)

    #define RUN_TEST(test) do { \
        printf("Running %s... ", #test); \
        test(&passed, &failed); \
        printf("OK\n"); \
    } while (0)

### Project Structure

    project/
    |-- Makefile
    |-- includes/
    |   |-- shell.h
    |   |-- parser.h
    |   |-- executor.h
    |-- src/
    |   |-- main.c
    |   |-- tokenizer.c
    |   |-- parser.c
    |   |-- executor.c
    |   |-- builtin.c
    |-- tests/
    |   |-- test_tokenizer.c
    |   |-- test_parser.c
    |   |-- test_executor.c
    |-- README.md

---

## Self-Evaluation and Peer Evaluation

### Self-Evaluation Checklist

- [ ] All projects compile with -Wall -Wextra -Werror
- [ ] All projects pass their own test suites
- [ ] No memory leaks (valgrind clean for all projects)
- [ ] No file descriptor leaks
- [ ] Error handling is comprehensive
- [ ] Code is modular and well-structured
- [ ] Each project has a README with build and usage instructions

### Peer Evaluation Form

| Criterion | Score (1-5) | Notes |
|-----------|-------------|-------|
| Shell handles all features | | |
| Calculator is correct | | |
| File manager is robust | | |
| JSON parser handles edge cases | | |
| Code quality and structure | | |
| Test coverage | | |
| No memory leaks | | |

---

## Summary

### What You Learned

- Built a mini shell with pipes, redirection, and background execution
- Built an expression calculator with proper parsing and evaluation
- Built a file manager with directory traversal and file operations
- Built a JSON parser with full type support and error handling
- Learned to structure C projects with proper modularity
- Wrote comprehensive tests using a simple test framework

### What's Next

Module 9: Advanced C - Optimization and Systems - You will learn bit manipulation tricks, inline assembly, profiling, and advanced optimization techniques.

---

Piscine+ - Module 8: Mini-Projects - Building Real Tools
Built with Hamster