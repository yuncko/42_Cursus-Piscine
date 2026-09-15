# Module 7: File I/O and System Calls

**Piscine+ - Advanced C Programming Curriculum**
Duration: 3-4 days | Language: C | Difficulty: Advanced

---

## Learning Objectives

By the end of this module, you will be able to:

1. Use file descriptors and understand the Unix I/O model
2. Read and write files using low-level system calls (open, read, write, close)
3. Manipulate file descriptors with dup, dup2, and fcntl
4. Create and use pipes for inter-process communication
5. Redirect standard input/output/error
6. Build a simple shell with pipes and redirection
7. Understand buffered vs unbuffered I/O

---

## Phase 1: File Descriptors and Low-Level I/O (Day 1)

### 1.1 File Descriptors

A file descriptor (fd) is a non-negative integer that the kernel uses to identify an open file.

| FD | Name | Symbolic Constant |
|----|------|-------------------|
| 0 | Standard input | STDIN_FILENO |
| 1 | Standard output | STDOUT_FILENO |
| 2 | Standard error | STDERR_FILENO |
| 3+ | User-opened files | |

### 1.2 Core System Calls

    #include <fcntl.h>   // open flags
    #include <unistd.h>  // read, write, close

    // open: open a file, returns fd or -1 on error
    int fd = open("file.txt", O_RDONLY);
    int fd = open("file.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);
    int fd = open("file.txt", O_RDWR | O_APPEND);

    // read: read bytes from fd into buffer
    ssize_t bytes_read = read(fd, buffer, sizeof(buffer));
    // returns: number of bytes read, 0 on EOF, -1 on error

    // write: write bytes from buffer to fd
    ssize_t bytes_written = write(fd, buffer, count);
    // returns: number of bytes written, -1 on error

    // close: close a file descriptor
    close(fd);

### 1.3 Open Flags

| Flag | Purpose |
|------|---------|
| O_RDONLY | Read only |
| O_WRONLY | Write only |
| O_RDWR | Read and write |
| O_CREAT | Create if not exists |
| O_TRUNC | Truncate to 0 if exists |
| O_APPEND | Append to end |
| O_EXCL | Fail if file exists (with O_CREAT) |
| O_NONBLOCK | Non-blocking mode |

### Exercise Set 1: Basic File I/O

#### Exercise 1.1 - File Copy (Easy)

Problem: Write a program that copies a file using only open, read, write, and close.

Acceptance Criteria:
- [ ] Handles file not found (source)
- [ ] Handles permission errors
- [ ] Handles empty files
- [ ] Uses a buffer (not byte-by-byte)
- [ ] Checks return values of read and write

Big-O Analysis:
- Time: O(n) where n = file size
- Space: O(buffer_size) - typically 1024 or 4096

#### Exercise 1.2 - File Statistics (Medium)

Problem: Write a program that reports file metadata using fstat:
- File size
- File type (regular, directory, symlink)
- Permissions
- Last modified time
- Number of hard links

Acceptance Criteria:
- [ ] Uses stat or fstat
- [ ] Formats output clearly
- [ ] Handles errors (file not found, permission denied)

#### Exercise 1.3 - Hex Dump Tool (Medium - Real-World Project)

Problem: Write a hex dump utility (like xxd) that:
- Reads a file in binary mode
- Prints offset, hex bytes, and ASCII representation
- Supports configurable bytes per line
- Supports reading from stdin if no file given

---

## Phase 2: File Descriptor Manipulation (Day 2)

### 2.1 dup and dup2

    // dup: duplicate a fd, returns new lowest available fd
    int new_fd = dup(old_fd);

    // dup2: duplicate a fd to a specific number
    dup2(old_fd, new_fd);  // new_fd now refers to the same file as old_fd

    // Common pattern: redirect stdout to a file
    int fd = open("output.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);
    dup2(fd, STDOUT_FILENO);  // stdout now writes to output.txt
    close(fd);
    printf("This goes to the file\n");

### 2.2 Redirection

    // Redirect stdin from a file
    int fd = open("input.txt", O_RDONLY);
    dup2(fd, STDIN_FILENO);
    close(fd);

    // Redirect stderr to stdout
    dup2(STDOUT_FILENO, STDERR_FILENO);

### 2.3 fcntl

    // Get fd flags
    int flags = fcntl(fd, F_GETFL);

    // Set fd flags (e.g., non-blocking)
    fcntl(fd, F_SETFL, flags | O_NONBLOCK);

### Exercise Set 2: Redirection

#### Exercise 2.1 - Output Redirect (Easy)

Problem: Write a program that redirects its own output to a file specified as a command-line argument.

Acceptance Criteria:
- [ ] Uses dup2 for redirection
- [ ] All printf output goes to the file
- [ ] Handles file open errors
- [ ] Restores original stdout (optional: save with dup first)

#### Exercise 2.2 - Tee Implementation (Medium - Real-World Project)

Problem: Implement the tee command: read from stdin and write to both stdout and a file.

Acceptance Criteria:
- [ ] Writes to both stdout and file simultaneously
- [ ] Supports -a flag for append mode
- [ ] Handles write errors
- [ ] Uses a buffer for efficiency

#### Exercise 2.3 - Pipe-based Communication (Hard)

Problem: Create a pipe between parent and child process using fork() and pipe().
- Parent writes a message
- Child reads and prints it
- Handle EOF correctly

---

## Phase 3: Pipes and Inter-Process Communication (Day 3)

### 3.1 Pipes

    // Create a pipe: fd[0] = read end, fd[1] = write end
    int fd[2];
    pipe(fd);

    // Fork a child
    pid_t pid = fork();
    if (pid == 0) {
        // Child: close write end, read from read end
        close(fd[1]);
        char buf[100];
        read(fd[0], buf, sizeof(buf));
        printf("Child received: %s\n", buf);
        close(fd[0]);
    } else {
        // Parent: close read end, write to write end
        close(fd[0]);
        write(fd[1], "Hello, child!", 13);
        close(fd[1]);
        wait(NULL);
    }

### 3.2 Named Pipes (FIFOs)

    // Create a named pipe
    mkfifo("/tmp/myfifo", 0644);

    // One process writes
    int fd = open("/tmp/myfifo", O_WRONLY);
    write(fd, "Hello", 5);

    // Another process reads
    int fd = open("/tmp/myfifo", O_RDONLY);
    char buf[100];
    read(fd, buf, sizeof(buf));

### Exercise Set 3: Pipes

#### Exercise 3.1 - Pipe Chain (Medium)

Problem: Create a chain of processes connected by pipes (like a pipeline in shell):

    process1 | process2 | process3

Acceptance Criteria:
- [ ] Each process reads from the previous pipe and writes to the next
- [ ] All file descriptors are properly closed
- [ ] No deadlock (close unused ends!)
- [ ] Parent waits for all children

#### Exercise 3.2 - Simple Shell with Pipes (Hard - Real-World Project)

Problem: Build a simple shell that supports:
- Command execution with execvp
- Pipe operator: cmd1 | cmd2
- Redirection: cmd > file, cmd < file, cmd >> file
- Background execution: cmd &
- Built-in commands: cd, exit, pwd

Acceptance Criteria:
- [ ] Handles single commands
- [ ] Handles pipes with 2+ commands
- [ ] Handles input/output redirection
- [ ] Handles background processes
- [ ] No zombie processes (use wait/waitpid)
- [ ] No file descriptor leaks

---

## Phase 4: Advanced I/O (Day 4)

### 4.1 Buffered vs Unbuffered I/O

| Feature | System calls (read/write) | Standard library (fread/fwrite) |
|---------|--------------------------|----------------------------------|
| Buffering | Unbuffered (kernel buffer only) | User-space buffer |
| Speed | Slower (many syscalls) | Faster (fewer syscalls) |
| Control | Fine-grained | Less control |
| Portability | Unix-specific | Portable |

### 4.2 select and poll

    // select: wait for activity on multiple fds
    fd_set readfds;
    FD_ZERO(&readfds);
    FD_SET(fd1, &readfds);
    FD_SET(fd2, &readfds);
    int ready = select(max_fd + 1, &readfds, NULL, NULL, &timeout);

### Exercise Set 4: Advanced I/O

#### Exercise 4.1 - Buffered vs Unbuffered Benchmark (Medium)

Problem: Write two programs that copy a large file (1MB+):
1. Using read/write (unbuffered)
2. Using fread/fwrite (buffered)
Benchmark both and compare.

Acceptance Criteria:
- [ ] Accurate timing
- [ ] Tests with different buffer sizes (1, 64, 256, 1024, 4096)
- [ ] Results table showing time vs buffer size
- [ ] Explanation of why buffered is faster

#### Exercise 4.2 - Chat Server with Named Pipes (Hard - Real-World Project)

Problem: Build a simple chat system using named pipes:
- Server creates two FIFOs (one for each direction)
- Client connects and can send/receive messages
- Multiple clients can connect (use select or poll)
- Clean shutdown on Ctrl+C

---

## Self-Evaluation and Peer Evaluation

### Self-Evaluation Checklist

- [ ] All system call return values are checked
- [ ] All file descriptors are properly closed
- [ ] No file descriptor leaks (check with lsof)
- [ ] No zombie processes (use wait/waitpid)
- [ ] Big-O analysis provided for I/O operations
- [ ] Error handling for all failure cases

### Peer Evaluation Form

| Criterion | Score (1-5) | Notes |
|-----------|-------------|-------|
| System calls are used correctly | | |
| Redirection works properly | | |
| Pipes are correctly set up | | |
| No fd leaks or zombies | | |
| Shell handles edge cases | | |
| Error handling is comprehensive | | |

---

## Summary

### What You Learned

- File descriptors and the Unix I/O model
- Low-level system calls: open, read, write, close
- File descriptor manipulation: dup, dup2, fcntl
- Pipes for inter-process communication
- Redirection of stdin/stdout/stderr
- Building a simple shell with pipes and redirection
- Buffered vs unbuffered I/O comparison

### What's Next

Module 8: Mini-Projects - Building Real Tools - You will combine everything learned so far to build complete, real-world tools: a mini shell, a calculator, a file manager, and a JSON parser.

---

Piscine+ - Module 7: File I/O and System Calls
Built with Hamster