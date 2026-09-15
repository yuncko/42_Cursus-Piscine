# Final Exams: Practical Assessment Pack

**Piscine+ - Advanced C Programming Curriculum**
Duration: 2-3 days | Format: Timed exam + take-home project

---

## Overview

This exam pack tests your mastery of all 10 modules. It consists of:
1. **Timed practical exercises** (4 hours) - done in a controlled environment
2. **Take-home project** (48 hours) - a complete C project
3. **Peer evaluation** - review another student's exam

---

## Part 1: Timed Practical Exercises (4 Hours)

### Exercise 1: String Library (30 minutes)

Reimplement these functions without using any standard library string functions:
- ft_strlen, ft_strcpy, ft_strncpy, ft_strcmp, ft_strncmp
- ft_strcat, ft_strncat, ft_strdup, ft_strchr, ft_strrchr
- ft_strstr, ft_strnstr

Grading:
- Correctness: 40%
- Edge cases (NULL, empty, overflow): 30%
- Big-O analysis: 30%

### Exercise 2: Linked List Operations (30 minutes)

Implement a singly linked list with:
- ft_lstnew, ft_lstadd_front, ft_lstadd_back
- ft_lstsize, ft_lstlast, ft_lstdelone, ft_lstclear
- ft_lstiter, ft_lstmap
- ft_lstsort (merge sort on linked list)

Grading:
- Correctness: 40%
- Memory management (no leaks): 30%
- Big-O analysis: 30%

### Exercise 3: Sorting Algorithm (45 minutes)

Implement quicksort with:
- Lomuto partition scheme
- Median-of-three pivot selection
- Insertion sort for small partitions (< 10 elements)
- In-place, O(1) extra space (excluding recursion stack)

Provide:
- Big-O proof for average case O(n log n)
- Explanation of worst case O(n^2) and how to avoid it
- Benchmark: sort 100,000 random integers and report time

### Exercise 4: Binary Tree (45 minutes)

Implement a binary search tree with:
- insert, search, delete (all three deletion cases)
- inorder, preorder, postorder, level-order traversals
- tree_height, tree_size, tree_count_leaves
- tree_is_bst (verify BST property)
- tree_free (no memory leaks)

### Exercise 5: File Processing (45 minutes)

Write a program that:
- Reads a text file using low-level system calls (open, read, close)
- Counts: lines, words, characters, digits, uppercase, lowercase
- Finds the longest word and its position
- Prints a character frequency histogram
- Handles files up to 10MB
- No memory leaks (valgrind clean)

### Exercise 6: Mini Shell Extension (45 minutes)

Extend a basic shell (provided) to support:
- Pipe operator: cmd1 | cmd2
- Output redirection: cmd > file
- Input redirection: cmd < file
- Error redirection: cmd 2> file
- Background execution: cmd &

Grading:
- Correctness: 40%
- No fd leaks or zombie processes: 30%
- Error handling: 30%

---

## Part 2: Take-Home Project (48 Hours)

### Project: Build a Complete Text Editor in C

Build a command-line text editor with the following features:

### Core Features (Required)

1. Open and display a file
2. Insert and delete characters
3. Cursor movement (up, down, left, right, start, end)
4. Save file (Ctrl+S)
5. Exit (Ctrl+Q)
6. Status bar showing: filename, cursor position, modified flag
7. Scroll for files longer than the terminal

### Advanced Features (Bonus)

8. Search (Ctrl+F): find text, highlight matches, next/previous
9. Undo/redo (Ctrl+Z, Ctrl+Y)
10. Line numbers (toggle with a command)
11. Syntax highlighting for C keywords
12. Copy/paste (Ctrl+C, Ctrl+V)
13. Multiple file support (tabs)
14. Config file for key bindings

### Technical Requirements

- Use raw terminal mode (termios)
- Use a gap buffer or piece table for efficient editing
- No external libraries (only standard C and POSIX)
- Must compile with: gcc -Wall -Wextra -Werror
- No memory leaks (valgrind clean)
- Handle files up to 1MB
- Handle files with very long lines (no line length limit)

### Architecture

    // Gap buffer structure
    typedef struct {
        char *buffer;      // allocated buffer
        int gap_start;     // start of gap
        int gap_end;       // end of gap
        int size;          // total buffer size
    } gap_buffer;

    // Editor state
    typedef struct {
        gap_buffer *content;
        int cursor_row;
        int cursor_col;
        int scroll_row;
        int num_lines;
        char *filename;
        int modified;
    } editor_state;

### Grading Rubric

| Criterion | Points | Notes |
|-----------|--------|-------|
| Core features work | 40 | All 7 required features |
| Memory management | 15 | No leaks, valgrind clean |
| Code quality | 15 | Modular, readable, commented |
| Big-O analysis | 10 | For each major operation |
| Advanced features | 10 | Each bonus feature worth 2 points |
| Error handling | 10 | File errors, terminal errors |
| Total | 100 | |

---

## Part 3: Peer Evaluation

### Instructions

1. You will be assigned another student's exam to review
2. Run their code and verify it works
3. Check for memory leaks with valgrind
4. Verify Big-O analysis is correct
5. Fill out the evaluation form below

### Peer Evaluation Form

| Criterion | Score (1-5) | Notes |
|-----------|-------------|-------|
| Code compiles with -Wall -Wextra -Werror | | |
| All required features implemented | | |
| No memory leaks (valgrind) | | |
| Edge cases handled | | |
| Big-O analysis is correct | | |
| Code is readable and modular | | |
| Error handling is comprehensive | | |
| Test coverage is adequate | | |
| Documentation (README, comments) | | |
| Overall impression | | |

### Feedback Guidelines

- Be specific: point to exact line numbers
- Be constructive: suggest improvements, not just criticism
- Be honest: do not inflate scores
- Be thorough: test edge cases yourself

---

## Exam Rules

1. No internet access during the timed portion
2. No AI assistance (ChatGPT, Copilot, etc.)
3. You may use man pages and your own notes
4. You may use your own libft and previous work
5. All code must compile with -Wall -Wextra -Werror
6. All code must be valgrind-clean
7. Big-O analysis is mandatory for every exercise
8. Partial credit is given for incomplete solutions

---

## Self-Evaluation Checklist (Post-Exam)

- [ ] All timed exercises completed within 4 hours
- [ ] All code compiles with -Wall -Wextra -Werror
- [ ] No memory leaks in any exercise (valgrind clean)
- [ ] Big-O analysis provided for every exercise
- [ ] Edge cases tested and documented
- [ ] Take-home project has a README with build instructions
- [ ] Take-home project handles files up to 1MB
- [ ] Peer evaluation is thorough and honest

---

## Summary

### What This Exam Tests

- Module 0-1: Unix basics, C fundamentals, compilation
- Module 2: Pointers, arrays, memory management
- Module 3: String manipulation, algorithms
- Module 4: Data structures (linked lists, trees)
- Module 5: Sorting and searching algorithms
- Module 6: Recursion and divide-and-conquer
- Module 7: File I/O and system calls
- Module 8: Mini-projects (shell, file processing)
- Module 9: Optimization and systems programming

### Passing Criteria

- Timed exercises: 60% or higher
- Take-home project: 70% or higher
- Peer evaluation completed
- No memory leaks in any exercise
- Big-O analysis provided for all exercises

---

Piscine+ - Final Exams: Practical Assessment Pack
Built with Hamster