# Piscine+ - Advanced C Programming Curriculum

Inspired by 42's Piscine, but more advanced and analytical.

## Overview

Piscine+ is a 10-module intensive curriculum that takes you from Unix basics to advanced C systems programming. Each module is designed for 3-4 days of self-study with peer evaluation elements.

## What makes this different from 42's original Piscine?

| Feature | 42 Piscine | Piscine+ |
|--------|-----------|----------|
| Big-O analysis | Not required | Mandatory for every exercise |
| Comparative analysis | No | Solve it 3 ways, compare tradeoffs |
| Error analysis | No | Dedicated section after each exercise group |
| Real-world projects | Abstract exercises | Mini-projects simulating real problems |
| Self-evaluation | Peer only | Structured checklist + peer evaluation |

## Modules

| # | Module | Topic |
|---|--------|-------|
| 0 | Unix Basics and Shell Mastery | Filesystem, permissions, bash, Makefile, Git |
| 1 | C Fundamentals and I/O | Compilation, types, control flow, functions, I/O |
| 2 | Pointers, Arrays and Memory | Pointers, arrays, dynamic memory, valgrind |
| 3 | String Manipulation and Algorithms | Parsing, pattern matching, text processing |
| 4 | Data Structures | Linked lists, stacks, queues, trees, BSTs |
| 5 | Algorithms - Sorting and Searching | Comparison sorts, non-comparison sorts, benchmarks |
| 6 | Recursion and Divide-and-Conquer | Recursion, backtracking, divide-and-conquer |
| 7 | File I/O and System Calls | File descriptors, read/write, dup, pipes |
| 8 | Mini-Projects - Building Real Tools | Shell, calculator, file manager, JSON parser |
| 9 | Advanced C - Optimization and Systems | Bit manipulation, inline assembly, profiling |
| Exam | Final Exams | Practical assessment pack |

## How to Convert to PDF

Using pandoc:

    pandoc piscine-plus/module-0-unix-basics.md -o module-0.pdf

Or convert all at once:
    for f in piscine-plus/module-*.md; do pandoc "$f" -o "${f%.md}.pdf"; done

## Language

All content is in English. Code, commands, and technical terms remain in English throughout.

---

Built with Hamster - AI-assisted curriculum design.