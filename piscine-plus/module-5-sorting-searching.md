# Module 5: Algorithms - Sorting and Searching

**Piscine+ - Advanced C Programming Curriculum**
Duration: 3-4 days | Language: C | Difficulty: Advanced

---

## Learning Objectives

By the end of this module, you will be able to:

1. Implement searching algorithms: linear, binary, interpolation search
2. Implement sorting algorithms: bubble, selection, insertion, merge, quick, heap sort
3. Implement non-comparison sorts: counting sort, radix sort
4. Prove Big-O complexity formally (not just state it)
5. Benchmark and compare algorithms empirically
6. Use custom comparators for sorting complex data
7. Build a file indexer with efficient search

---

## Phase 1: Searching Algorithms (Day 1)

### 1.1 Linear Search

    int linear_search(int *arr, int n, int target) {
        for (int i = 0; i < n; i++)
            if (arr[i] == target) return i;
        return -1;
    }

Big-O: O(n) time, O(1) space

### 1.2 Binary Search

    int binary_search(int *arr, int n, int target) {
        int left = 0, right = n - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] == target) return mid;
            if (arr[mid] < target) left = mid + 1;
            else right = mid - 1;
        }
        return -1;
    }

Big-O: O(log n) time, O(1) space (iterative) or O(log n) space (recursive)

### 1.3 Interpolation Search

For uniformly distributed data, interpolation search can achieve O(log log n) average time.

### Exercise Set 1: Searching

#### Exercise 1.1 - Search Implementations (Easy)

Problem: Implement linear search, binary search (iterative and recursive), and interpolation search.

Acceptance Criteria:
- [ ] Binary search handles duplicates (returns any valid index)
- [ ] No integer overflow in mid calculation (use left + (right - left) / 2)
- [ ] Handles empty arrays
- [ ] Handles target not found (returns -1)

Big-O Analysis:

| Algorithm | Best | Average | Worst | Space |
|-----------|------|---------|-------|-------|
| Linear | O(1) | O(n) | O(n) | O(1) |
| Binary | O(1) | O(log n) | O(log n) | O(1) |
| Interpolation | O(1) | O(log log n) | O(n) | O(1) |

#### Exercise 1.2 - Search in Rotated Sorted Array (Medium to Hard)

Problem: Given a sorted array that has been rotated (e.g., [4,5,6,7,0,1,2]), search for a target in O(log n) time.

Acceptance Criteria:
- [ ] O(log n) time complexity
- [ ] Handles no rotation (normal sorted array)
- [ ] Handles duplicates (optional: O(n) worst case)
- [ ] Returns index or -1

#### Exercise 1.3 - File Indexer (Hard - Real-World Project)

Problem: Build a file indexer that:
- Reads all filenames in a directory
- Sorts them alphabetically
- Supports binary search for fast lookup
- Supports prefix search (all files starting with "test")
- Supports fuzzy search (Levenshtein distance <= k)

---

## Phase 2: Comparison Sorting (Day 2)

### 2.1 Simple Sorts (O(n^2))

#### Bubble Sort

    void bubble_sort(int *arr, int n) {
        for (int i = 0; i < n - 1; i++)
            for (int j = 0; j < n - i - 1; j++)
                if (arr[j] > arr[j + 1])
                    swap(&arr[j], &arr[j + 1]);
    }

Optimization: add a swapped flag to detect early termination.

#### Selection Sort

    void selection_sort(int *arr, int n) {
        for (int i = 0; i < n - 1; i++) {
            int min = i;
            for (int j = i + 1; j < n; j++)
                if (arr[j] < arr[min]) min = j;
            if (min != i) swap(&arr[i], &arr[min]);
        }
    }

#### Insertion Sort

    void insertion_sort(int *arr, int n) {
        for (int i = 1; i < n; i++) {
            int key = arr[i], j = i - 1;
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }
            arr[j + 1] = key;
        }
    }

### 2.2 Efficient Sorts (O(n log n))

#### Merge Sort

    void merge(int *arr, int left, int mid, int right) {
        int n1 = mid - left + 1, n2 = right - mid;
        int L[n1], R[n2];
        // Copy, merge, and copy back
    }

    void merge_sort(int *arr, int left, int right) {
        if (left < right) {
            int mid = left + (right - left) / 2;
            merge_sort(arr, left, mid);
            merge_sort(arr, mid + 1, right);
            merge(arr, left, mid, right);
        }
    }

#### Quick Sort

    int partition(int *arr, int low, int high) {
        int pivot = arr[high];
        int i = low - 1;
        for (int j = low; j < high; j++)
            if (arr[j] < pivot) { i++; swap(&arr[i], &arr[j]); }
        swap(&arr[i + 1], &arr[high]);
        return i + 1;
    }

    void quick_sort(int *arr, int low, int high) {
        if (low < high) {
            int pi = partition(arr, low, high);
            quick_sort(arr, low, pi - 1);
            quick_sort(arr, pi + 1, high);
        }
    }

#### Heap Sort

Uses a binary heap to sort in O(n log n) time, O(1) space.

### Exercise Set 2: Comparison Sorts

#### Exercise 2.1 - Implement All Sorts (Easy to Medium)

Problem: Implement all 6 comparison sorts: bubble, selection, insertion, merge, quick, heap.

Acceptance Criteria:
- [ ] All sorts produce correct output
- [ ] No warnings with -Wall -Wextra -Werror
- [ ] Each sort has a test with: sorted, reverse, random, empty, single-element arrays

#### Exercise 2.2 - Benchmark Suite (Medium to Hard)

Problem: Build a benchmarking suite that:
- Generates test arrays of various sizes (100, 1000, 10000, 100000)
- Times each algorithm using clock() or gettimeofday()
- Prints results in a table
- Tests with: random, sorted, reverse-sorted, nearly-sorted, many-duplicates

Acceptance Criteria:
- [ ] Uses accurate timing (microseconds)
- [ ] Tests multiple input distributions
- [ ] Results are printed in a formatted table
- [ ] Includes theoretical vs empirical comparison

Big-O Analysis:

| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Bubble | O(n) | O(n^2) | O(n^2) | O(1) | Yes |
| Selection | O(n^2) | O(n^2) | O(n^2) | O(1) | No |
| Insertion | O(n) | O(n^2) | O(n^2) | O(1) | Yes |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick | O(n log n) | O(n log n) | O(n^2) | O(log n) | No |
| Heap | O(n log n) | O(n log n) | O(n log n) | O(1) | No |

#### Exercise 2.3 - Formal Big-O Proofs (Hard)

Problem: Write formal Big-O proofs for:
1. Merge sort: prove the recurrence T(n) = 2T(n/2) + O(n) = O(n log n)
2. Quick sort: prove average case is O(n log n) and worst case is O(n^2)
3. Heap sort: prove heapify is O(n) and sort is O(n log n)

Use the Master Theorem or substitution method.

---

## Phase 3: Non-Comparison Sorts (Day 3)

### 3.1 Counting Sort

    void counting_sort(int *arr, int n, int max_val) {
        int count[max_val + 1];
        int output[n];
        // Count occurrences, compute prefix sum, place in output
    }

Big-O: O(n + k) time, O(n + k) space (k = range of values)

### 3.2 Radix Sort

    void radix_sort(int *arr, int n) {
        // Sort by each digit using counting sort as subroutine
        // Process LSD to MSD
    }

Big-O: O(d * (n + k)) time where d = number of digits, k = base

### Exercise Set 3: Non-Comparison Sorts

#### Exercise 3.1 - Counting and Radix Sort (Medium)

Problem: Implement counting sort and radix sort. Handle negative numbers in radix sort.

Acceptance Criteria:
- [ ] Counting sort handles range efficiently
- [ ] Radix sort handles negative numbers (split or offset)
- [ ] Both sorts are stable
- [ ] No memory leaks

#### Exercise 3.2 - Sort Comparator (Medium - Real-World Project)

Problem: Write a generic sort function that uses function pointers for comparison. Sort:
- Array of integers (ascending, descending)
- Array of strings (alphabetical, by length)
- Array of structs (by any field)

Acceptance Criteria:
- [ ] Uses void * and function pointers
- [ ] Works with any data type
- [ ] No type-specific code in the sort function

---

## Phase 4: Advanced Topics (Day 4)

### 4.1 Stability and In-Place Sorting

| Property | Definition | Which sorts have it |
|----------|-----------|---------------------|
| Stable | Equal elements keep relative order | Merge, Insertion, Bubble, Counting |
| In-place | O(1) extra space | Heap, Quick, Insertion, Bubble |
| Adaptive | Faster on nearly-sorted data | Insertion, Bubble (with flag) |

### 4.2 When to Use Which Sort

| Scenario | Recommended Sort | Why |
|----------|-----------------|------|
| Small arrays (<50) | Insertion | Low overhead, adaptive |
| Large random | Quick | Fast in practice, in-place |
| Need stability | Merge | Stable, guaranteed O(n log n) |
| Large, external | Merge | Sequential access pattern |
| Integer range known | Counting/Radix | O(n) linear time |
| Nearly sorted | Insertion | O(n) on sorted input |

### Exercise Set 4: Advanced

#### Exercise 4.1 - Hybrid Sort (Hard)

Problem: Implement a hybrid sort that uses quick sort for large partitions and insertion sort for small ones (like introsort or Timsort concepts).

Acceptance Criteria:
- [ ] Switches to insertion sort below a threshold (e.g., 16 elements)
- [ ] Outperforms pure quick sort on small arrays
- [ ] Maintains O(n log n) worst case

#### Exercise 4.2 - File Indexer with Search (Hard - Real-World Project)

Problem: Build a complete file indexer:
- Scan a directory recursively for files
- Store filenames in a sorted array (using your best sort)
- Implement binary search for exact match
- Implement prefix search (all files starting with a given string)
- Implement fuzzy search (Levenshtein distance)
- Benchmark search performance on 10000+ files

---

## Self-Evaluation and Peer Evaluation

### Self-Evaluation Checklist

- [ ] All sorts handle edge cases (empty, single, sorted, reverse)
- [ ] Benchmark results match theoretical Big-O
- [ ] Formal proofs are mathematically correct
- [ ] No memory leaks in any implementation
- [ ] Custom comparators work with multiple data types

### Peer Evaluation Form

| Criterion | Score (1-5) | Notes |
|-----------|-------------|-------|
| All sorts are correct | | |
| Big-O proofs are valid | | |
| Benchmark is thorough | | |
| Custom comparator works | | |
| Code is clean and modular | | |

---

## Summary

### What You Learned

- Searching: linear, binary, interpolation search
- Comparison sorts: bubble, selection, insertion, merge, quick, heap
- Non-comparison sorts: counting, radix
- Formal Big-O proofs using Master Theorem
- Benchmarking and empirical analysis
- Custom comparators for generic sorting

### What's Next

Module 6: Recursion and Divide-and-Conquer - You will master recursive thinking, implement backtracking algorithms, and solve classic divide-and-conquer problems.

---

Piscine+ - Module 5: Algorithms - Sorting and Searching
Built with Hamster