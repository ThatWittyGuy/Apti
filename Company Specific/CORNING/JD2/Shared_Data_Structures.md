# Shared — Data Structures MCQs
### Applies to: JD2 (Data & AI Engineering), any JD mentioning CS fundamentals, data structures, algorithms
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What is a "data structure"?
- A) A format for storing data in a CSV file
- B) A way of organizing and storing data in a computer so it can be accessed and modified efficiently
- C) A database schema definition
- D) A type of algorithm

**Answer: B**

---

**Q2.** 🟢 What is an array?
- A) A collection of key-value pairs
- B) A contiguous block of memory storing elements of the same type, accessed by integer index in O(1) time
- C) A dynamically resizing list of any type
- D) A linked sequence of nodes

**Answer: B**

---

**Q3.** 🟢 What is the time complexity of accessing an element in an array by index?
- A) O(n)
- B) O(1) — constant time, since arrays use direct index-based memory addressing
- C) O(log n)
- D) O(n²)

**Answer: B**

---

**Q4.** 🟢 What is a "stack" data structure?
- A) A collection where the first element inserted is the first removed (FIFO)
- B) A collection following LIFO (Last In, First Out) — elements are pushed onto and popped from the top; like a stack of plates
- C) A sorted collection of elements
- D) A key-value storage structure

**Answer: B**

---

**Q5.** 🟢 What is a "queue" data structure?
- A) A LIFO collection where the last element added is removed first
- B) A FIFO (First In, First Out) collection — elements enqueue at the back and dequeue from the front; like a line of people
- C) A priority-based collection
- D) A collection with no defined order

**Answer: B**

---

**Q6.** 🟢 What is a "linked list"?
- A) A fixed-size array with links to child arrays
- B) A sequence of nodes where each node stores data and a pointer to the next node — enabling O(1) insertion/deletion at known positions but O(n) access by index
- C) A doubly indexed array
- D) A list of URLs linking to data sources

**Answer: B**

---

**Q7.** 🟢 What is a "hash table" (dictionary/hashmap)?
- A) A sorted array of key-value pairs
- B) A data structure mapping keys to values using a hash function, enabling average O(1) lookup, insertion, and deletion
- C) A table stored on a hash-partitioned disk
- D) A linked list of key-value pairs

**Answer: B**

---

**Q8.** 🟢 What is a "binary tree"?
- A) A tree where each node has exactly two children
- B) A hierarchical data structure where each node has at most two children (left and right)
- C) A tree storing only binary (0/1) data
- D) A balanced tree used for database indexing only

**Answer: B**

---

**Q9.** 🟡 What is the difference between a singly and doubly linked list?
- A) Singly stores integers; doubly stores any type
- B) Singly linked list nodes have one pointer (to next); doubly linked list nodes have two pointers (to next and previous) — enabling O(1) backward traversal at the cost of extra memory
- C) Doubly linked lists are sorted; singly are unsorted
- D) They are identical in performance

**Answer: B**

---

**Q10.** 🟡 What is "Big O notation"?
- A) A notation system for writing large numbers
- B) A mathematical notation describing the upper bound of an algorithm's time or space complexity as input size grows — e.g., O(n) means linear growth
- C) A grading system for algorithm quality
- D) A notation for binary operations

**Answer: B**

---

**Q11.** 🟡 What is the time complexity of binary search, and what does it require?
- A) O(n); works on any unsorted array
- B) O(log n); requires the array to be sorted — repeatedly halves the search space by comparing the middle element
- C) O(1); requires a hash table
- D) O(n log n); requires a balanced tree

**Answer: B**

---

**Q12.** 🟡 What is the time complexity of linear search?
- A) O(1)
- B) O(n) — in the worst case, every element must be checked
- C) O(log n)
- D) O(n²)

**Answer: B**

---

**Q13.** 🟡 What is a "Binary Search Tree" (BST)?
- A) A tree where every node has exactly two children
- B) A binary tree where the left child is smaller than the parent and the right child is larger — enabling O(log n) average search, insert, and delete
- C) A tree used only for binary data
- D) A balanced tree with guaranteed O(log n) operations

**Answer: B**

---

**Q14.** 🟡 What is a "graph" data structure?
- A) A chart used for data visualization
- B) A collection of nodes (vertices) and edges connecting them — used to represent networks, relationships, and paths; can be directed or undirected, weighted or unweighted
- C) A hierarchical tree structure
- D) A 2D matrix of values

**Answer: B**

---

**Q15.** 🟡 What is the difference between BFS (Breadth-First Search) and DFS (Depth-First Search)?
- A) BFS uses a stack; DFS uses a queue
- B) BFS explores all neighbors at the current depth before going deeper (uses a queue — good for shortest path); DFS explores as far as possible along each branch before backtracking (uses a stack/recursion — good for cycle detection, topological sort)
- C) BFS is for trees only; DFS is for graphs
- D) They produce identical traversal orders

**Answer: B**

---

**Q16.** 🟡 What is a "heap" data structure?
- A) A pile of unsorted data in memory
- B) A complete binary tree satisfying the heap property — max-heap: parent ≥ children; min-heap: parent ≤ children; used for priority queues and heap sort; O(log n) insert/delete, O(1) peek
- C) A dynamic array that expands automatically
- D) A hash table with overflow buckets

**Answer: B**

---

**Q17.** 🟡 What is a "priority queue"?
- A) A queue that serves VIP users first
- B) An abstract data type where each element has a priority and elements with higher priority are dequeued before lower-priority ones — typically implemented with a heap
- C) A queue with two ends (front and back)
- D) A sorted linked list

**Answer: B**

---

**Q18.** 🟡 What is "recursion" in programming?
- A) Repeating a loop multiple times
- B) A technique where a function calls itself with a smaller subproblem until a base case is reached — used in tree traversal, divide-and-conquer algorithms, and backtracking
- C) A type of iteration using while loops
- D) Calling multiple functions in sequence

**Answer: B**

---

**Q19.** 🟡 What is the time complexity of bubble sort in the worst case?
- A) O(n log n)
- B) O(n²) — each element may be compared and swapped with every other element
- C) O(n)
- D) O(log n)

**Answer: B**

---

**Q20.** 🟡 What is merge sort and what is its time complexity?
- A) A sort that merges two sorted arrays; O(n)
- B) A divide-and-conquer sorting algorithm that recursively splits arrays in half, sorts each half, and merges them — O(n log n) in all cases, stable, but requires O(n) extra space
- C) A sort that merges duplicate values; O(n²)
- D) An in-place sorting algorithm; O(log n)

**Answer: B**

---

**Q21.** 🟡 What is "quick sort" and what is its average time complexity?
- A) A sorting algorithm using a queue; O(n²)
- B) A divide-and-conquer sort that picks a pivot, partitions elements around it, and recursively sorts partitions — O(n log n) average, O(n²) worst case (bad pivot), O(log n) space (in-place)
- C) The fastest sort in all cases; O(n)
- D) A stable sort using a min-heap; O(n log n)

**Answer: B**

---

**Q22.** 🟡 What is a "trie" (prefix tree)?
- A) A type of binary search tree
- B) A tree-based data structure where each node represents a character — used for efficient string prefix operations like autocomplete and spell check; O(m) lookup where m = string length
- C) A graph traversal algorithm
- D) A compressed hash table for strings

**Answer: B**

---

**Q23.** 🟡 What is "dynamic programming" (DP)?
- A) Programming that dynamically allocates memory
- B) An optimization technique that solves complex problems by breaking them into overlapping subproblems, solving each once, and storing results (memoization/tabulation) to avoid redundant computation
- C) A type of recursive algorithm without a base case
- D) A programming paradigm for real-time systems

**Answer: B**

---

**Q24.** 🟡 What is the difference between "memoization" and "tabulation" in dynamic programming?
- A) Memoization is bottom-up; tabulation is top-down
- B) Memoization is top-down — solve recursively and cache results; tabulation is bottom-up — iteratively fill a table from base cases up to the final answer
- C) They are identical approaches
- D) Memoization uses arrays; tabulation uses hash maps

**Answer: B**

---

**Q25.** 🟡 What is a "deque" (double-ended queue)?
- A) A queue with priority levels
- B) A data structure allowing insertion and deletion at both front and back in O(1) — combining features of both stack and queue; used in sliding window problems
- C) A doubly linked queue with sorted elements
- D) A queue used only for graph BFS

**Answer: B**

---

**Q26.** 🔴 What is an AVL tree?
- A) A binary tree with arbitrary structure
- B) A self-balancing BST that maintains a balance factor (height difference between left and right subtrees ≤ 1) through rotations — guaranteeing O(log n) search, insert, and delete
- C) A tree where all leaves are at the same depth
- D) A type of B-tree used in databases

**Answer: B**

---

**Q27.** 🔴 What is a "B-tree" and where is it commonly used?
- A) A binary tree with a branching factor of 2
- B) A self-balancing, multi-way search tree with many keys per node — minimizing disk I/O by keeping tree height small; widely used in database indexes and file systems
- C) A tree balanced using AVL rotations
- D) A tree structure used exclusively in SAP HANA

**Answer: B**

---

**Q28.** 🔴 What is "hash collision" and how is it resolved?
- A) Two hash tables storing the same data
- B) When two different keys produce the same hash value — resolved by chaining (linked list at that bucket) or open addressing (probing for the next empty slot)
- C) A security vulnerability in hash functions
- D) An error when a hash table is full

**Answer: B**

---

**Q29.** 🔴 What is "amortized time complexity"? Give an example.
- A) The average time across multiple algorithm runs
- B) The average cost per operation over a sequence of operations — even if individual operations are sometimes expensive; example: dynamic array append is O(1) amortized because occasional O(n) resizes are spread across n operations
- C) The time complexity after compiler optimization
- D) The time complexity of an algorithm when memory is limited

**Answer: B**

---

**Q30.** 🔴 What is Dijkstra's algorithm used for?
- A) Sorting a graph's nodes by degree
- B) Finding the shortest path from a source node to all other nodes in a weighted graph with non-negative edge weights — using a priority queue (min-heap), O((V+E) log V)
- C) Detecting cycles in a directed graph
- D) Finding the minimum spanning tree of a graph

**Answer: B**

---

**Q31.** 🔴 What is a "minimum spanning tree" (MST)?
- A) The smallest possible binary tree for a dataset
- B) A subset of edges in a weighted undirected graph that connects all vertices with the minimum total edge weight and no cycles — found by Kruskal's or Prim's algorithm
- C) A tree with the minimum number of nodes
- D) A balanced BST with minimum height

**Answer: B**

---

**Q32.** 🔴 What is "topological sort" and when is it used?
- A) Sorting a graph's nodes alphabetically
- B) A linear ordering of nodes in a Directed Acyclic Graph (DAG) such that for every edge u→v, u appears before v — used for dependency resolution (build systems, task scheduling, pipeline ordering)
- C) A sort that arranges tree nodes by depth
- D) A graph traversal producing a sorted array

**Answer: B**

---

**Q33.** 🔴 What is the space complexity of DFS on a graph with V vertices and E edges?
- A) O(E)
- B) O(V) — the call stack (or explicit stack) can hold at most V frames/nodes in the worst case (a path graph)
- C) O(V + E)
- D) O(1)

**Answer: B**

---

**Q34.** 🔴 What is a "disjoint set" (Union-Find) data structure used for?
- A) Storing disjoint (non-overlapping) arrays
- B) Tracking which elements belong to which disjoint sets — with near O(1) amortized union and find operations (with path compression and union by rank); used for cycle detection and Kruskal's MST
- C) A set that does not allow duplicate values
- D) A data structure for range queries

**Answer: B**

---

**Q35.** 🔴 What is "space-time tradeoff" in algorithm design?
- A) Choosing between execution speed and code readability
- B) The design decision of using more memory to reduce time complexity (e.g., caching/memoization) or using less memory at the cost of more computation — fundamental in DP, hash tables vs. sorted arrays
- C) The tradeoff between cloud storage cost and processing time
- D) The tradeoff between algorithm correctness and speed

**Answer: B**

---
*35 Questions | Shared across CS-fundamentals-focused JDs.*
