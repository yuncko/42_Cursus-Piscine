# Module 0: Unix Basics and Shell Mastery

**Piscine+ - Advanced C Programming Curriculum**
Duration: 3-4 days | Language: C and Bash | Difficulty: Foundation

---

## Learning Objectives

By the end of this module, you will be able to:

1. Navigate the Unix filesystem confidently using command-line tools
2. Manage file permissions, ownership, and manipulation operations
3. Write Bash scripts with variables, loops, conditionals, and functions
4. Create and understand Makefiles with rules, variables, and pattern rules
5. Use Git for version control: init, add, commit, branch, merge, remote
6. Work efficiently in a terminal-based editing workflow (Vim)
7. Analyze the time and space complexity of shell operations

---

## Phase 1: Unix Filesystem and Navigation (Day 1)

### 1.1 Filesystem Hierarchy

The Unix filesystem is a single tree rooted at /. Understanding the standard directory layout is essential:

    /
    |-- bin/    (Essential user binaries)
    |-- etc/    (Configuration files)
    |-- home/   (User home directories)
    |-- lib/    (Shared libraries)
    |-- tmp/    (Temporary files)
    |-- usr/    (User programs and data)
    |-- var/    (Variable data - logs, mail, etc.)

### 1.2 Essential Commands

| Command | Purpose | Example |
|---------|---------|---------|
| pwd | Print working directory | pwd -> /home/user |
| cd | Change directory | cd /tmp |
| ls | List directory contents | ls -la |
| mkdir | Create directory | mkdir -p src/utils |
| rm | Remove files/directories | rm -rf build/ |
| cp | Copy files | cp -r src/ backup/ |
| mv | Move/rename | mv old.c new.c |
| find | Find files | find . -name *.c |
| grep | Search text | grep -rn TODO . |
| chmod | Change permissions | chmod 755 script.sh |
| chown | Change ownership | chown user:group file |

### 1.3 File Permissions

Every file has three permission sets: owner, group, and others. Each set has read (r), write (w), and execute (x).

    $ ls -l
    -rwxr-xr-- 1 user group 4096 Sep 15 10:00 script.sh

Numeric representation:

| Number | Permission | Symbol |
|--------|------------|--------|
| 7 | rwx | read + write + execute |
| 6 | rw- | read + write |
| 5 | r-x | read + execute |
| 4 | r-- | read only |
| 0 | --- | no access |

### Exercise Set 1: Filesystem Operations

#### Exercise 1.1 - Directory Explorer (Easy)

Problem: Write a Bash script that takes a directory path as an argument and prints:
- Total number of files
- Total number of directories
- Total size in bytes
- The 5 largest files

Acceptance Criteria:
- [ ] Script accepts a directory path as $1
- [ ] Handles non-existent paths gracefully (prints error to stderr, exits with code 1)
- [ ] Output is formatted and readable
- [ ] Works with nested directories

Big-O Analysis:
- Time: O(n) where n = total files/directories in the tree
- Space: O(d) where d = max depth (for recursion stack)

Self-Evaluation Checklist:
- [ ] Did you handle paths with spaces?
- [ ] Did you handle symlinks (follow or skip)?
- [ ] Did you handle permission-denied errors?
- [ ] What happens with an empty directory?

Common Mistakes:
1. Not quoting variables: $dir vs "$dir" - paths with spaces break
2. Using ls output in a loop - use find instead
3. Forgetting set -e for error handling

#### Exercise 1.2 - Permission Manager (Medium)

Problem: Write a Bash script that recursively sets permissions based on file type:
- Directories -> 755
- Regular files -> 644
- Executable files -> 755
- Symlinks -> skip (do not modify)

Acceptance Criteria:
- [ ] Uses find with -type filters
- [ ] Handles symlinks correctly (skips them)
- [ ] Supports --dry-run flag to preview changes
- [ ] Prints a summary of changes made

Big-O Analysis:
- Time: O(n) where n = total files in the tree
- Space: O(1) - no additional data structures needed

Comparative Analysis (Solve 3 Ways):

| Approach | Pros | Cons |
|---------|------|------|
| find -exec chmod | One-liner, efficient | Less readable, hard to debug |
| find + while read loop | More control, can log | Slower for large trees |
| Recursive function | Full control, educational | Risk of stack overflow on deep trees |

Error Analysis:
- What if a file is owned by another user? chmod fails silently
- What if the path contains newlines? Use find -print0 + read -d ''
- What if a file changes type during iteration? Race condition, acceptable for scripts

#### Exercise 1.3 - File Manager (Hard - Real-World Project)

Problem: Build a simple file manager in Bash that supports:
- list <dir> - list files with sizes and permissions
- create <dir> <name> - create a new file/directory
- delete <path> - delete a file (with confirmation)
- search <dir> <pattern> - search for files by name pattern
- stats <dir> - show directory statistics

Acceptance Criteria:
- [ ] Implements a command dispatcher pattern
- [ ] All commands have input validation
- [ ] Uses functions for each command
- [ ] Includes a help system (--help)
- [ ] Handles errors gracefully with meaningful messages

Big-O Analysis:
- list: O(n) for n files in directory
- search: O(n * m) where n = files, m = pattern matching cost
- stats: O(n) for traversal

---

## Phase 2: Shell Scripting with Bash (Day 2)

### 2.1 Variables and Data Types

Bash has no types - everything is a string. Understanding quoting rules is critical:

    name="World"
    echo "Hello, $name"        # Double quotes: variable expansion
    echo 'Hello, $name'        # Single quotes: no expansion
    echo Hello $name           # No quotes: expansion, but word splitting

### 2.2 Control Flow

    # If-else
    if [ "$1" == "--help" ]; then
      show_help
      exit 0
    fi

    # For loop
    for file in *.c; do
      echo "Compiling $file..."
      gcc -Wall -Wextra "$file" -o "${file%.c}"
    done

    # While loop
    while read -r line; do
      process_line "$line"
    done < input.txt

    # Case statement
    case "$1" in
      start)  start_service ;;
      stop)   stop_service ;;
      restart) stop_service; start_service ;;
      *) echo "Usage: $0 {start|stop|restart}"; exit 1 ;;
    esac

### 2.3 Functions

    log_info() {
      echo "[INFO] $(date '+%Y-%m-%d %H:%M:%S') $1" >&2
    }

    log_error() {
      echo "[ERROR] $(date '+%Y-%m-%d %H:%M:%S') $1" >&2
    }

### Exercise Set 2: Bash Scripting

#### Exercise 2.1 - Argument Parser (Easy)

Problem: Write a Bash script that parses command-line arguments:
- --input <file> - input file path
- --output <file> - output file path
- --verbose - enable verbose output
- --help - show usage

Acceptance Criteria:
- [ ] Uses a while loop with case for parsing
- [ ] Handles unknown arguments with an error
- [ ] Validates that required arguments are provided
- [ ] --help prints usage and exits 0

Big-O Analysis:
- Time: O(n) where n = number of arguments
- Space: O(1) - only stores parsed values

#### Exercise 2.2 - Log Analyzer (Medium - Real-World Project)

Problem: Write a Bash script that analyzes a log file and reports:
- Total lines
- Number of ERROR lines
- Number of WARNING lines
- Top 5 most frequent error messages
- Time range (first and last timestamp)

Acceptance Criteria:
- [ ] Uses grep, awk, sort, uniq in pipelines
- [ ] Handles empty log files
- [ ] Handles malformed log lines
- [ ] Output is formatted as a table

Big-O Analysis:
- Counting lines: O(n)
- Finding top 5: O(n log n) due to sort
- Space: O(k) where k = unique error messages

Comparative Analysis:

| Approach | Pros | Cons |
|---------|------|------|
| grep | wc -l | Simple, fast | Limited to counting |
| awk single-pass | One pass, flexible | More complex syntax |
| Pure Bash loop | No external tools | Very slow for large files |

#### Exercise 2.3 - Build Automation Script (Hard)

Problem: Write a Bash script that:
- Compiles all .c files in a directory
- Links them into a single binary
- Runs tests if a tests/ directory exists
- Reports success/failure with colored output
- Supports --clean to remove build artifacts

Acceptance Criteria:
- [ ] Uses gcc with -Wall -Wextra -Werror
- [ ] Handles compilation errors gracefully
- [ ] Color-coded output (green=success, red=failure)
- [ ] Exit code reflects success (0) or failure (1)

---

## Phase 3: Build Tools and Version Control (Day 3)

### 3.1 Makefile Fundamentals

A Makefile defines rules for building a project:

    # Variables
    CC = gcc
    CFLAGS = -Wall -Wextra -Werror
    NAME = myprogram

    # Object files
    SRCS = main.c utils.c parser.c
    OBJS = $(SRCS:.c=.o)

    # Main rule
    $(NAME): $(OBJS)
        $(CC) $(CFLAGS) $(OBJS) -o $(NAME)

    # Pattern rule
    %.o: %.c
        $(CC) $(CFLAGS) -c $< -o $@

    # Clean rule
    clean:
        rm -f $(OBJS) $(NAME)

    # Phony targets
    .PHONY: clean all fclean re

    all: $(NAME)

    fclean: clean

    re: fclean all

Key concepts:
- Automatic variables: $@ (target), $< (first dependency), $^ (all dependencies)
- Pattern rules: %.o: %.c matches any .o file to its .c source
- Phony targets: .PHONY prevents conflicts with files of the same name

### 3.2 Git Basics

    # Initialize
    git init
    git add .
    git commit -m "Initial commit"

    # Branching
    git branch feature/login
    git checkout feature/login
    git checkout -b feature/login  # Create + switch

    # Merging
    git checkout main
    git merge feature/login

    # Remote
    git remote add origin git@github.com:user/repo.git
    git push -u origin main
    git pull origin main

    # Status and log
    git status
    git log --oneline --graph --all

### Exercise Set 3: Makefiles and Git

#### Exercise 3.1 - Multi-File Makefile (Easy)

Problem: Create a Makefile for a project with this structure:

    project/
    |-- Makefile
    |-- includes/
    |   |-- myheader.h
    |-- src/
    |   |-- main.c
    |   |-- utils.c
    |   |-- parser.c
    |-- lib/
        |-- libft/
            |-- *.c

Acceptance Criteria:
- [ ] Compiles all .c files from src/ and lib/libft/
- [ ] Includes includes/ directory with -I flag
- [ ] Has all, clean, fclean, re rules
- [ ] Uses variables for compiler and flags
- [ ] Object files are created in a build/ directory

#### Exercise 3.2 - Git Workflow Simulator (Medium)

Problem: Write a Bash script that simulates a Git workflow:
- Creates a new branch
- Makes 3 commits with descriptive messages
- Merges back to main
- Shows the commit graph
- Cleans up the branch

Acceptance Criteria:
- [ ] Uses real Git commands
- [ ] Commit messages follow conventional commits format
- [ ] Handles merge conflicts with a resolution example
- [ ] Prints each step with explanations

#### Exercise 3.3 - Build System Generator (Hard - Real-World Project)

Problem: Write a Bash script that auto-generates a Makefile for any C project:
- Scans for .c files recursively
- Detects header directories
- Generates a complete Makefile with all standard rules
- Supports --debug flag to add -g flag
- Supports --strict flag to add -Werror

---

## Phase 4: Integration and Peer Evaluation (Day 4)

### 4.1 Vim Workflow

Essential Vim commands for C development:

| Command | Action |
|---------|--------|
| i | Insert mode |
| Esc | Normal mode |
| :w | Save |
| :q | Quit |
| :wq | Save and quit |
| dd | Delete line |
| yy | Yank (copy) line |
| p | Paste |
| /pattern | Search |
| :%s/old/new/g | Replace all |
| v | Visual mode |
| Ctrl+v | Visual block mode |

### 4.2 Integration Challenge

Final Challenge - Build a Project from Scratch:

1. Create a new Git repository
2. Write a Bash script that generates a C project skeleton:
   - src/ directory with a main.c template
   - includes/ directory with a header template
   - Makefile with standard rules
   - README.md with project description
3. Commit everything to Git with proper commit messages
4. Push to a remote repository

### 4.3 Peer Evaluation Form

Evaluate your peer's work on the following criteria (1-5 scale):

| Criterion | Score | Notes |
|-----------|-------|-------|
| Script handles edge cases | | |
| Code is readable and commented | | |
| Error messages are helpful | | |
| Makefile is correct and complete | | |
| Git workflow is clean | | |
| Big-O analysis is correct | | |
| Self-evaluation checklist is thorough | | |

---

## Summary

### What You Learned

- Unix filesystem navigation and file permissions
- Bash scripting: variables, loops, conditionals, functions
- Makefile fundamentals: rules, variables, pattern rules
- Git basics: init, add, commit, branch, merge, remote
- Vim editing workflow
- Big-O analysis for shell operations

### What's Next

Module 1: C Fundamentals and I/O - You will learn the C compilation pipeline, variables, types, control flow, functions, and basic I/O. This is where you start writing real C programs.

---

Piscine+ - Module 0: Unix Basics and Shell Mastery
Built with Hamster