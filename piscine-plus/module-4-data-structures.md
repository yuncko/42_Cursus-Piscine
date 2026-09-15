# Module 4: Data Structures - Linked Lists and Trees

**Piscine+ - Advanced C Programming Curriculum**
Duration: 3-4 days | Language: C | Difficulty: Intermediate to Advanced

---

## Learning Objectives

By the end of this module, you will be able to:

1. Implement singly and doubly linked lists with all operations
2. Build stacks and queues using both arrays and linked lists
3. Create and traverse binary trees (inorder, preorder, postorder, level-order)
4. Implement binary search trees (BST): insertion, search, deletion
5. Compare data structure tradeoffs and choose the right one
6. Analyze time and space complexity of every operation

---

## Phase 1: Linked Lists (Day 1-2)

### 1.1 Singly Linked List

    typedef struct s_list {
        void *content;
        struct s_list *next;
    } t_list;

    // Core operations
    t_list *ft_lstnew(void *content);
    void ft_lstadd_front(t_list **lst, t_list *new);
    void ft_lstadd_back(t_list **lst, t_list *new);
    int ft_lstsize(t_list *lst);
    t_list *ft_lstlast(t_list *lst);
    void ft_lstdelone(t_list *lst, void (*del)(void *));
    void ft_lstclear(t_list **lst, void (*del)(void *));
    void ft_lstiter(t_list *lst, void (*f)(void *));
    t_list *ft_lstmap(t_list *lst, void *(*f)(void *), void (*del)(void *));

### 1.2 Doubly Linked List

    typedef struct s_dlist {
        void *content;
        struct s_dlist *prev;
        struct s_dlist *next;
    } t_dlist;

### 1.3 Circular Linked List

In a circular list, the last node points back to the first node instead of NULL.

### Exercise Set 1: Linked Lists

#### Exercise 1.1 - List Operations (Easy to Medium)

Problem: Implement all core singly linked list operations listed in 1.1.

Acceptance Criteria:
- [ ] All functions work correctly with NULL lists
- [ ] No memory leaks (valgrind clean)
- [ ] ft_lstmap creates a new list (does not modify original)
- [ ] Uses function pointers for content-specific operations

Big-O Analysis:

| Operation | Time | Space |
|-----------|------|-------|
| lstnew | O(1) | O(1) |
| lstadd_front | O(1) | O(1) |
| lstadd_back | O(n) | O(1) |
| lstsize | O(n) | O(1) |
| lstlast | O(n) | O(1) |
| lstclear | O(n) | O(1) |

#### Exercise 1.2 - List Reversal and Merge (Medium)

Problem: Implement:
- ft_lstreverse - reverse a list in-place
- ft_lstmerge - merge two lists into one
- ft_lstsort - sort a list using merge sort
- ft_lstremove - remove a node by content comparison

Acceptance Criteria:
- [ ] Reversal is in-place (no new allocations)
- [ ] Merge does not copy content (relinks nodes)
- [ ] Sort is stable and O(n log n)
- [ ] Remove handles first, middle, last, and single-node cases

Big-O Analysis:
- Reverse: O(n) time, O(1) space
- Merge: O(n+m) time, O(1) space
- Sort (merge sort): O(n log n) time, O(log n) space (recursion)
- Remove: O(n) time, O(1) space

#### Exercise 1.3 - Polynomial Arithmetic (Hard - Real-World Project)

Problem: Represent polynomials as linked lists and implement:
- polynomial_add(a, b) - add two polynomials
- polynomial_subtract(a, b) - subtract
- polynomial_multiply(a, b) - multiply
- polynomial_print(p) - print as "3x^2 + 2x + 1"
- polynomial_evaluate(p, x) - evaluate at a given x

Each node stores a term: coefficient and exponent.

Acceptance Criteria:
- [ ] Handles zero polynomials
- [ ] Combines like terms in addition
- [ ] Multiplication is correct for all degrees
- [ ] Terms are stored in descending order of exponent

---

## Phase 2: Stacks and Queues (Day 2)

### 2.1 Stack (LIFO - Last In, First Out)

    // Array-based stack
    typedef struct {
        int *data;
        int top;
        int capacity;
    } stack;

    void push(stack *s, int value);
    int pop(stack *s);
    int peek(stack *s);
    int is_empty(stack *s);

    // Linked-list-based stack
    typedef struct s_stack_node {
        int value;
        struct s_stack_node *next;
    } stack_node;

### 2.2 Queue (FIFO - First In, First Out)

    // Array-based circular queue
    typedef struct {
        int *data;
        int front;
        int rear;
        int capacity;
        int size;
    } queue;

    void enqueue(queue *q, int value);
    int dequeue(queue *q);
    int front(queue *q);
    int is_empty(queue *q);
    int is_full(queue *q);

### Exercise Set 2: Stacks and Queues

#### Exercise 2.1 - Stack and Queue Implementation (Easy to Medium)

Problem: Implement both array-based and linked-list-based stacks and queues.

Acceptance Criteria:
- [ ] Array-based stack handles overflow (resize or error)
- [ ] Circular queue handles wraparound correctly
- [ ] Linked-list versions have no fixed capacity
- [ ] All operations are O(1)

Big-O Analysis:

| Operation | Array-based | Linked-list-based |
|-----------|-------------|-------------------|
| push/enqueue | O(1) amortized | O(1) |
| pop/dequeue | O(1) | O(1) |
| peek/front | O(1) | O(1) |
| Space | O(n) | O(n) |

#### Exercise 2.2 - Parentheses Checker (Medium)

Problem: Use a stack to check if parentheses/brackets/braces are balanced.

Examples:
- "()" -> balanced
- "()[]{}" -> balanced
- "([)]" -> not balanced
- "{[]}" -> balanced

Acceptance Criteria:
- [ ] Handles (), [], {}
- [ ] Handles nested brackets
- [ ] Handles strings with other characters (ignore non-brackets)
- [ ] Returns 1 if balanced, 0 if not

Big-O Analysis:
- Time: O(n) where n = string length
- Space: O(n) worst case (all opening brackets)

#### Exercise 2.3 - Task Scheduler (Hard - Real-World Project)

Problem: Build a task scheduler using a priority queue:
- Each task has: name, priority (1-10), duration
- Tasks are executed in priority order (highest first)
- Tasks with the same priority are FIFO
- Support: add_task, get_next_task, peek_next, list_all

Acceptance Criteria:
- [ ] Uses a priority queue (heap or sorted list)
- [ ] Handles task preemption (optional)
- [ ] No memory leaks
- [ ] Prints execution order

---

## Phase 3: Binary Trees (Day 3)

### 3.1 Binary Tree Structure

    typedef struct s_tree {
        int value;
        struct s_tree *left;
        struct s_tree *right;
    } t_tree;

    // Core operations
    t_tree *tree_create(int value);
    void tree_free(t_tree *root);
    int tree_height(t_tree *root);
    int tree_size(t_tree *root);
    int tree_count_leaves(t_tree *root);

### 3.2 Tree Traversals

    // Depth-First Traversals
    void preorder(t_tree *root)   { /* root, left, right */ }
    void inorder(t_tree *root)    { /* left, root, right */ }
    void postorder(t_tree *root)  { /* left, right, root */ }

    // Breadth-First Traversal (level-order)
    void levelorder(t_tree *root) { /* uses a queue */ }

### 3.3 Binary Search Tree (BST)

A BST maintains the invariant: left child < parent < right child.

    t_tree *bst_insert(t_tree *root, int value);
    t_tree *bst_search(t_tree *root, int value);
    t_tree *bst_delete(t_tree *root, int value);
    t_tree *bst_min(t_tree *root);
    t_tree *bst_max(t_tree *root);

### Exercise Set 3: Binary Trees

#### Exercise 3.1 - Tree Traversals (Easy to Medium)

Problem: Implement all four traversals (preorder, inorder, postorder, level-order) for a binary tree.

Acceptance Criteria:
- [ ] Recursive implementations for DFS traversals
- [ ] Iterative implementation for level-order (uses queue)
- [ ] Handles empty trees
- [ ] Handles single-node trees

Big-O Analysis:
- All traversals: O(n) time, O(h) space (h = height, for recursion stack)
- Level-order: O(n) time, O(w) space (w = max width, for queue)

#### Exercise 3.2 - BST Operations (Medium to Hard)

Problem: Implement a full BST with insert, search, delete, min, max.

Deletion cases:
1. Node is a leaf -> just remove it
2. Node has one child -> replace with child
3. Node has two children -> replace with inorder successor (or predecessor)

Acceptance Criteria:
- [ ] All three deletion cases work correctly
- [ ] Tree remains a valid BST after each operation
- [ ] Handles deleting from an empty tree
- [ ] Handles deleting the root node

Big-O Analysis:

| Operation | Average | Worst (skewed) |
|-----------|---------|----------------|
| Insert | O(log n) | O(n) |
| Search | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Min/Max | O(log n) | O(n) |

#### Exercise 3.3 - Expression Evaluator (Hard - Real-World Project)

Problem: Build an expression tree evaluator:
- Parse arithmetic expressions: "3 + 4 * 2"
- Build an expression tree respecting operator precedence
- Evaluate the tree to get the result
- Support: +, -, *, /, parentheses
- Print the expression in prefix, infix, and postfix notation

Acceptance Criteria:
- [ ] Correctly handles operator precedence
- [ ] Handles parentheses
- [ ] Handles division by zero
- [ ] All traversals produce correct output

---

## Phase 4: Advanced Tree Topics (Day 4)

### 4.1 AVL Trees (Self-Balancing)

AVL trees maintain balance by tracking the balance factor of each node and performing rotations when it exceeds 1 or -1.

Rotations:
- Left rotation (LL case)
- Right rotation (RR case)
- Left-Right rotation (LR case)
- Right-Left rotation (RL case)

### 4.2 Tree Comparison

| Structure | Insert | Search | Delete | Balance |
|-----------|--------|--------|--------|---------|
| Unsorted BST | O(log n) avg | O(log n) avg | O(log n) avg | No |
| AVL Tree | O(log n) | O(log n) | O(log n) | Yes |
| Hash Table | O(1) avg | O(1) avg | O(1) avg | N/A |

### Exercise Set 4: Advanced Trees

#### Exercise 4.1 - AVL Tree (Hard)

Problem: Implement an AVL tree with insert, delete, and search. Include all four rotation types.

Acceptance Criteria:
- [ ] Tree remains balanced after every operation
- [ ] All rotation cases are handled
- [ ] Height is always O(log n)
- [ ] No memory leaks

#### Exercise 4.2 - Tree Serialization (Medium - Real-World Project)

Problem: Serialize and deserialize a binary tree:
- serialize(root) -> string representation
- deserialize(string) -> tree
- Use level-order or preorder with null markers

Acceptance Criteria:
- [ ] Round-trip: deserialize(serialize(tree)) produces identical tree
- [ ] Handles empty trees
- [ ] Handles unbalanced trees
- [ ] No memory leaks

---

## Self-Evaluation and Peer Evaluation

### Self-Evaluation Checklist

- [ ] All data structures handle empty/NULL cases
- [ ] All memory is freed (valgrind clean)
- [ ] Big-O analysis provided for every operation
- [ ] Edge cases tested: single node, empty structure, large input
- [ ] Function pointers used where appropriate (for generic content)

### Peer Evaluation Form

| Criterion | Score (1-5) | Notes |
|-----------|-------------|-------|
| List operations are correct | | |
| Stack/Queue implementations work | | |
| Tree traversals are correct | | |
| BST operations handle all cases | | |
| Memory management is clean | | |
| Big-O analysis is correct | | |

---

## Summary

### What You Learned

- Singly and doubly linked lists with all operations
- Stacks and queues (array-based and linked-list-based)
- Binary trees: creation, traversal, height, size
- Binary search trees: insert, search, delete
- AVL trees: self-balancing with rotations
- Data structure tradeoffs and selection criteria

### What's Next

Module 5: Algorithms - Sorting and Searching - You will implement and benchmark multiple sorting algorithms, prove their Big-O complexity, and build a file indexer with efficient search.

---

Piscine+ - Module 4: Data Structures - Linked Lists and Trees
Built with Hamster