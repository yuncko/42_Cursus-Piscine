# Module 3: String Manipulation and Algorithms

**Piscine+ - Advanced C Programming Curriculum**
Duration: 3-4 days | Language: C | Difficulty: Intermediate

---

## Learning Objectives

By the end of this module, you will be able to:

1. Parse strings: tokenization, splitting, trimming
2. Implement pattern matching: strstr, strchr from scratch
3. Classify characters: isalpha, isdigit, isalnum
4. Convert strings to numbers and back: atoi, itoa, base conversion
5. Process text: word count, line parsing, CSV parsing
6. Understand regex concepts through manual implementation
7. Build a mini JSON parser in C
8. Analyze time and space complexity of every solution

---

## Phase 1: String Parsing (Day 1)

### 1.1 Tokenization

Tokenization is the process of splitting a string into meaningful units (tokens) based on delimiters.

    // Using strtok (standard library)
    char str[] = "Hello,World,42,Piscine";
    char *token = strtok(str, ",");
    while (token != NULL) {
        printf("%s\n", token);
        token = strtok(NULL, ",");
    }
    // Output: Hello / World / 42 / Piscine

    // Problem: strtok modifies the original string and is not thread-safe
    // Solution: implement your own tokenizer

### 1.2 Splitting and Trimming

    // Split: divide string by delimiter into array of strings
    // Trim: remove leading and trailing whitespace

    char *trim(char *str) {
        char *end;
        // Trim leading space
        while (isspace((unsigned char)*str)) str++;
        if (*str == 0) return str;  // all spaces
        // Trim trailing space
        end = str + strlen(str) - 1;
        while (end > str && isspace((unsigned char)*end)) end--;
        end[1] = '\0';
        return str;
    }

### Exercise Set 1: String Parsing

#### Exercise 1.1 - ft_split (Easy to Medium)

Problem: Write a function that splits a string by a given delimiter character and returns an array of strings (NULL-terminated).

    char **ft_split(const char *str, char delimiter);

Acceptance Criteria:
- [ ] Returns a NULL-terminated array of strings
- [ ] Handles consecutive delimiters (empty tokens or skip them - document your choice)
- [ ] Handles delimiter at start/end of string
- [ ] Handles empty string (returns array with just NULL)
- [ ] All memory is freed by a corresponding ft_free_split function

Big-O Analysis:
- Time: O(n) where n = string length (single pass to count, single pass to copy)
- Space: O(k) where k = number of tokens (for the array) + O(n) for the string copies

Common Mistakes:
1. Not counting tokens first (requires two passes or dynamic resizing)
2. Memory leak: allocating strings but not the array, or vice versa
3. Off-by-one: forgetting the NULL terminator in the array

#### Exercise 1.2 - CSV Parser (Medium - Real-World Project)

Problem: Write a CSV parser that handles:
- Comma-separated values
- Quoted fields (containing commas): "Hello, World",42
- Escaped quotes: "He said ""Hi"""
- Empty fields: a,,b
- Newlines within quoted fields

Acceptance Criteria:
- [ ] Handles all CSV edge cases above
- [ ] Returns a 2D array (rows x columns)
- [ ] Provides a function to free the parsed data
- [ ] Handles files with varying column counts

Big-O Analysis:
- Time: O(n) where n = total characters in the file
- Space: O(n) for storing all parsed data

Error Analysis:
- Unterminated quotes: how to handle? (error or read to EOF)
- Empty lines: skip or return empty row?
- BOM (byte order mark) at start of file

#### Exercise 1.3 - Advanced Tokenizer (Hard)

Problem: Write a tokenizer that supports multiple delimiters and returns delimiter information:
- ft_advanced_split(str, delimiters) - split by any of the delimiters
- Returns both the tokens and which delimiter was used
- Handles escape characters (e.g., \, means literal comma)

---

## Phase 2: Pattern Matching (Day 2)

### 2.1 String Search Functions

    // ft_strstr - find substring in string
    char *ft_strstr(const char *haystack, const char *needle) {
        if (!*needle) return (char *)haystack;
        for (; *haystack; haystack++) {
            const char *h = haystack;
            const char *n = needle;
            while (*h && *n && *h == *n) { h++; n++; }
            if (!*n) return (char *)haystack;
        }
        return NULL;
    }

    // ft_strchr - find character in string
    char *ft_strchr(const char *s, int c) {
        while (*s) {
            if (*s == c) return (char *)s;
            s++;
        }
        return (c == '\0') ? (char *)s : NULL;
    }

### 2.2 Pattern Matching Algorithms

| Algorithm | Time (avg) | Time (worst) | Space | Notes |
|-----------|-----------|--------------|-------|-------|
| Naive | O(n*m) | O(n*m) | O(1) | Simple, works for small strings |
| KMP | O(n+m) | O(n+m) | O(m) | Preprocesses pattern |
| Boyer-Moore | O(n/m) best | O(n*m) | O(m+alphabet) | Fastest in practice |
| Rabin-Karp | O(n+m) | O(n*m) | O(1) | Uses hashing |

### Exercise Set 2: Pattern Matching

#### Exercise 2.1 - ft_strstr and ft_strchr (Easy)

Problem: Reimplement strstr and strchr from scratch.

Acceptance Criteria:
- [ ] ft_strstr handles empty needle (returns haystack)
- [ ] ft_strstr handles needle not found (returns NULL)
- [ ] ft_strchr handles null terminator search (returns pointer to \0)
- [ ] No use of standard library string functions

Big-O Analysis:
- ft_strstr: O(n*m) time, O(1) space (naive approach)
- ft_strchr: O(n) time, O(1) space

#### Exercise 2.2 - KMP Algorithm (Medium to Hard)

Problem: Implement the Knuth-Morris-Pratt string matching algorithm.

Steps:
1. Build the failure function (partial match table)
2. Use the failure function to skip redundant comparisons

Acceptance Criteria:
- [ ] Failure function is computed correctly
- [ ] Search uses the failure function for skipping
- [ ] Handles overlapping matches (e.g., "aaa" in "aaaaa")
- [ ] Returns the index of the first match (or -1 if not found)

Big-O Analysis:
- Preprocessing: O(m) time, O(m) space
- Search: O(n) time
- Total: O(n+m) time, O(m) space

Comparative Analysis:

| Approach | Pros | Cons |
|---------|------|------|
| Naive | Simple, no preprocessing | O(n*m) worst case |
| KMP | Linear time, no backtracking | Requires O(m) preprocessing space |
| Boyer-Moore | Very fast in practice | Complex, O(m+alphabet) space |

#### Exercise 2.3 - Wildcard Matcher (Hard - Real-World Project)

Problem: Implement a wildcard pattern matcher that supports:
- ? matches any single character
- * matches any sequence of characters (including empty)
- All other characters match literally

Example: match("Hello World", "Hello *") -> true
Example: match("file.txt", "*.txt") -> true
Example: match("test", "t?st") -> true

Acceptance Criteria:
- [ ] Handles * at start, middle, and end
- [ ] Handles multiple * in pattern
- [ ] Handles ? correctly
- [ ] No regex library used

Big-O Analysis:
- Naive recursive: O(2^(n+m)) worst case (exponential)
- Dynamic programming: O(n*m) time, O(n*m) space
- Optimized DP: O(n*m) time, O(m) space (rolling array)

---

## Phase 3: Character Classification and Conversion (Day 2-3)

### 3.1 Character Classification

    // Reimplement these from scratch
    int ft_isalpha(int c);  // a-z, A-Z
    int ft_isdigit(int c);  // 0-9
    int ft_isalnum(int c);  // a-z, A-Z, 0-9
    int ft_isprint(int c);  // printable ASCII 32-126
    int ft_isspace(int c);  // space, tab, newline, etc.
    int ft_isupper(int c);  // A-Z
    int ft_islower(int c);  // a-z

### 3.2 String Conversion

    // ft_atoi - ASCII to integer
    int ft_atoi(const char *str) {
        int sign = 1, result = 0, i = 0;
        while (ft_isspace(str[i])) i++;
        if (str[i] == '-' || str[i] == '+') {
            if (str[i] == '-') sign = -1;
            i++;
        }
        while (ft_isdigit(str[i])) {
            result = result * 10 + (str[i] - '0');
            i++;
        }
        return result * sign;
    }

    // ft_itoa - integer to ASCII
    char *ft_itoa(int n) {
        // Handle negative numbers, count digits, allocate, fill
    }

    // ft_atoi_base - convert string in given base to integer
    int ft_atoi_base(const char *str, int base);

### Exercise Set 3: Classification and Conversion

#### Exercise 3.1 - Character Library (Easy)

Problem: Reimplement all character classification functions listed in 3.1.

Acceptance Criteria:
- [ ] Uses ASCII value comparisons (no standard library)
- [ ] Handles all edge cases (EOF, values outside ASCII range)
- [ ] Returns 1 for true, 0 for false (not the standard non-zero)

#### Exercise 3.2 - ft_atoi and ft_itoa (Medium)

Problem: Reimplement atoi and itoa. Extend atoi to handle:
- Leading whitespace
- Optional sign (+ or -)
- Overflow detection (return INT_MAX or INT_MIN on overflow)
- Non-numeric characters after digits (stop parsing)

Acceptance Criteria:
- [ ] Handles INT_MIN correctly (most negative integer)
- [ ] Detects overflow and handles it gracefully
- [ ] ft_itoa handles negative numbers
- [ ] ft_itoa returns a malloc'd string (caller must free)

Big-O Analysis:
- ft_atoi: O(n) time, O(1) space
- ft_itoa: O(log10(n)) time, O(log10(n)) space

#### Exercise 3.3 - Base Converter (Hard)

Problem: Write functions to convert between any bases (2-16):
- ft_atoi_base(str, base) - parse string in given base to int
- ft_itoa_base(n, base) - convert int to string in given base
- ft_convert_base(str, from_base, to_base) - full conversion

Acceptance Criteria:
- [ ] Handles bases 2, 8, 10, 16
- [ ] Handles uppercase and lowercase hex digits
- [ ] Handles negative numbers in all bases
- [ ] Validates input (rejects invalid digits for the base)

---

## Phase 4: Text Processing Algorithms (Day 3-4)

### 4.1 Word Count and Line Processing

    // Count words in a string (words are separated by whitespace)
    int word_count(const char *str) {
        int count = 0, in_word = 0;
        while (*str) {
            if (ft_isspace(*str)) {
                in_word = 0;
            } else if (!in_word) {
                in_word = 1;
                count++;
            }
            str++;
        }
        return count;
    }

### 4.2 Text Statistics

A text analysis tool can compute:
- Character frequency histogram
- Word frequency counter
- Line count, word count, character count (like wc)
- Average word length
- Most common words

### Exercise Set 4: Text Processing

#### Exercise 4.1 - wc Clone (Easy to Medium)

Problem: Write a program that replicates the wc (word count) command:
- Count lines, words, and characters
- Accept multiple files as arguments
- Print totals when multiple files are given

Acceptance Criteria:
- [ ] Output format matches wc: lines words characters filename
- [ ] Handles multiple files with totals
- [ ] Handles stdin when no file is given (use -)
- [ ] Handles file not found errors

Big-O Analysis:
- Time: O(n) where n = total file size
- Space: O(1) - single character processing

#### Exercise 4.2 - Word Frequency Analyzer (Medium - Real-World Project)

Problem: Write a program that:
- Reads a text file
- Counts frequency of each word
- Prints the top 10 most frequent words
- Handles punctuation (strip it before counting)
- Case-insensitive counting

Acceptance Criteria:
- [ ] Uses a hash table or sorted array for counting
- [ ] Handles punctuation correctly (strip .,!?;: etc.)
- [ ] Case-insensitive (convert to lowercase)
- [ ] Prints sorted by frequency (descending)

Big-O Analysis:
- Reading: O(n) where n = file size
- Counting: O(n) average with hash table, O(n*k) with array (k = unique words)
- Sorting: O(k log k) where k = unique words
- Space: O(k) for the word table

#### Exercise 4.3 - Simple Grep (Hard - Real-World Project)

Problem: Implement a simplified version of grep that:
- Searches for a pattern in a file
- Prints matching lines with line numbers
- Supports -i flag for case-insensitive search
- Supports -n flag for line numbers
- Supports -v flag for inverted matching (non-matching lines)
- Supports -c flag for count only

Acceptance Criteria:
- [ ] Handles all flags correctly
- [ ] Works with multiple files
- [ ] Handles binary files (skip or warn)
- [ ] Exit code: 0 if match found, 1 if no match, 2 on error

---

## Phase 5: Mini JSON Parser (Day 4 - Capstone Project)

### 5.1 JSON Grammar

JSON (JavaScript Object Notation) has a simple grammar:

    value = object | array | string | number | true | false | null
    object = {} | { members }
    members = pair | pair , members
    pair = string : value
    array = [] | [ elements ]
    elements = value | value , elements
    string = " chars "
    number = int frac? exp?

### 5.2 Parser Structure

    typedef enum {
        JSON_NULL, JSON_BOOL, JSON_NUMBER, JSON_STRING,
        JSON_ARRAY, JSON_OBJECT
    } json_type;

    typedef struct json_value {
        json_type type;
        union {
            int boolean;
            double number;
            char *string;
            struct { struct json_value **items; size_t count; } array;
            struct { char **keys; struct json_value **values; size_t count; } object;
        };
    } json_value;

    // Parser functions
    json_value *json_parse(const char *str);
    void json_free(json_value *value);
    void json_print(json_value *value, int indent);

### Exercise 5.1 - JSON Parser (Hard - Capstone Project)

Problem: Build a complete JSON parser in C that handles:
- Objects: {"key": value, ...}
- Arrays: [value, ...]
- Strings: "Hello\nWorld" (with escape sequences)
- Numbers: integers and floats (42, -3.14, 1e10)
- Booleans: true, false
- Null: null
- Whitespace: skip between tokens
- Nested structures: objects in arrays, arrays in objects

Acceptance Criteria:
- [ ] Parses all JSON types correctly
- [ ] Handles escape sequences in strings (\n, \t, \", \\, \uXXXX)
- [ ] Handles nested structures (at least 10 levels deep)
- [ ] Handles empty objects {} and empty arrays []
- [ ] Handles trailing commas (error or allow - document your choice)
- [ ] Provides json_free to free all allocated memory
- [ ] No memory leaks (valgrind clean)
- [ ] Provides json_print for pretty-printing

Big-O Analysis:
- Parsing: O(n) time, O(d) space for recursion stack (d = nesting depth)
- Memory: O(n) for storing the parsed tree

Error Analysis:
- Unterminated strings: return error with position
- Invalid escape sequences: return error
- Trailing commas: decide on policy (strict or lenient)
- Number overflow: detect and report
- Duplicate keys: decide on policy (last wins or error)

Comparative Analysis (3 Parsing Approaches):

| Approach | Pros | Cons |
|---------|------|------|
| Recursive descent | Clean, readable, easy to extend | Stack overflow on deeply nested input |
| State machine | No recursion, handles deep nesting | More complex, harder to extend |
| Jump table + goto | Fast, no recursion | Hard to maintain, error-prone |

---

## Self-Evaluation and Peer Evaluation

### Self-Evaluation Checklist

- [ ] All string functions handle NULL input gracefully
- [ ] All allocated memory is freed (valgrind clean)
- [ ] No buffer overflows (always check bounds)
- [ ] Big-O analysis provided for every exercise
- [ ] Edge cases tested: empty strings, NULL, very long strings
- [ ] Unicode/multibyte characters considered (at least documented)

### Peer Evaluation Form

| Criterion | Score (1-5) | Notes |
|-----------|-------------|-------|
| String functions are correct | | |
| Memory management is clean | | |
| Pattern matching is efficient | | |
| JSON parser handles edge cases | | |
| Big-O analysis is correct | | |
| Error handling is comprehensive | | |
| Code is readable and modular | | |

---

## Summary

### What You Learned

- String parsing: tokenization, splitting, trimming
- Pattern matching: naive, KMP, wildcard matching
- Character classification and string conversion
- Text processing: word count, frequency analysis, simple grep
- JSON parsing: recursive descent, error handling, memory management

### What's Next

Module 4: Data Structures - Linked Lists and Trees - You will learn to build linked lists, stacks, queues, and binary trees from scratch, with full Big-O analysis for each operation.

---

Piscine+ - Module 3: String Manipulation and Algorithms
Built with Hamster