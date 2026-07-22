# Operating System Numerical MCQs (Placement / On-Campus Assessment)

## 1) CPU Scheduling (FCFS)

Processes arrive at time 0 in the order:

| Process | Burst Time |
| ------- | ---------- |
| P1      | 5          |
| P2      | 3          |
| P3      | 8          |
| P4      | 6          |

What is the average waiting time using FCFS?

a) 8.5  
b) 7.5  
c) 6.5  
d) 9

### Solution

Waiting times:

```
P1 = 0
P2 = 5
P3 = 5 + 3 = 8
P4 = 5 + 3 + 8 = 16
```

Average waiting time:

\[
\frac{0+5+8+16}{4}=7.25
\]

**Answer: None of the above (if option exists)**

---

# 2) SJF Scheduling

Processes:

| Process | Burst Time |
| ------- | ---------- |
| P1      | 6          |
| P2      | 8          |
| P3      | 7          |
| P4      | 3          |

Find average waiting time using non-preemptive SJF.

a) 7  
b) 8  
c) 6  
d) 5

### Solution

Execution order:

```
P4 → P1 → P3 → P2
```

Waiting times:

```
P4 = 0
P1 = 3
P3 = 9
P2 = 16
```

Average:

\[
\frac{0+3+9+16}{4}=7
\]

**Answer: a) 7**

---

# 3) Round Robin Scheduling

Time Quantum = 2 ms

Processes:

| Process | Burst Time |
| ------- | ---------- |
| P1      | 5          |
| P2      | 4          |

What is the completion time of P1?

a) 7  
b) 8  
c) 9  
d) 10

### Solution

Execution:

```
0-2   P1
2-4   P2
4-6   P1
6-8   P2
8-9   P1
```

P1 completes at:

```
9 ms
```

**Answer: c) 9**

---

# 4) Page Replacement (FIFO)

Reference string:

```
1 2 3 4 1 2 5 1 2 3 4 5
```

Number of frames = 3

Number of page faults?

a) 9  
b) 10  
c) 8  
d) 7

### Solution

FIFO simulation:

| Reference | Result |
| --------- | ------ |
| 1         | Fault  |
| 2         | Fault  |
| 3         | Fault  |
| 4         | Fault  |
| 1         | Fault  |
| 2         | Fault  |
| 5         | Fault  |
| 1         | Fault  |
| 2         | Fault  |
| 3         | Fault  |
| 4         | Fault  |
| 5         | Fault  |

Total page faults:

```
12
```

**Answer: 12 (if option exists)**

---

# 5) LRU Page Replacement

Frames = 3

Reference string:

```
7 0 1 2 0 3 0 4
```

Number of page faults?

a) 6  
b) 7  
c) 5  
d) 8

### Solution

Simulation:

```
7 → Fault
0 → Fault
1 → Fault
2 → Fault (replace 7)
0 → Hit
3 → Fault (replace 1)
0 → Hit
4 → Fault (replace 2)
```

Total faults:

```
6
```

**Answer: a) 6**

---

# 6) Banker's Algorithm

Available resources:

```
A B C
3 3 2
```

Process P1 needs:

```
1 2 2
```

Can P1 execute?

a) Yes  
b) No

### Solution

Check:

```
1 ≤ 3
2 ≤ 3
2 ≤ 2
```

All conditions satisfy.

**Answer: a) Yes**

---

# 7) Deadlock Detection

A system has:

- 5 processes
- Each process requires 2 resources
- Total resources = 9

Can deadlock occur?

a) Yes  
b) No

### Solution

Maximum requirement:

\[
5 \times 2 = 10
\]

Available resources:

```
9
```

Since resources are insufficient:

**Answer: a) Yes**

---

# 8) Memory Allocation (First Fit)

Memory blocks:

```
100 KB, 500 KB, 200 KB, 300 KB
```

Process sizes:

```
212 KB, 417 KB
```

Using First Fit, where is 212 KB allocated?

a) 100 KB  
b) 500 KB  
c) 200 KB  
d) 300 KB

### Solution

Check blocks:

```
100 KB → Too small
500 KB → Fits
```

**Answer: b) 500 KB**

---

# 9) Disk Scheduling (FCFS)

Disk request queue:

```
98, 183, 37, 122, 14, 124
```

Initial head position:

```
53
```

Find total head movement.

a) 640  
b) 620  
c) 630  
d) 600

### Solution

Movement:

\[
|53-98|+|98-183|+|183-37|+|37-122|+|122-14|+|14-124|
\]

\[
45+85+146+85+108+110
\]

\[
=579
\]

**Answer: 579**

---

# 10) Paging Address Translation

Page size = 1 KB

Logical address = 2500 bytes

Find page number and offset.

a) Page 2, Offset 452  
b) Page 3, Offset 452  
c) Page 2, Offset 500  
d) Page 1, Offset 400

### Solution

Page size:

```
1 KB = 1024 bytes
```

Page number:

\[
2500 / 1024 = 2
\]

Offset:

\[
2500-(2 \times 1024)=452
\]

**Answer: a) Page 2, Offset 452**

---

# Important OS Numerical Topics for Placements

1. FCFS / SJF / Round Robin scheduling
2. Waiting time and turnaround time calculation
3. FIFO / LRU / Optimal page replacement
4. Banker's algorithm
5. Deadlock detection
6. Disk scheduling (FCFS, SSTF, SCAN)
7. Memory allocation (First Fit, Best Fit, Worst Fit)
8. Paging and address translation
9. Page table calculations
10. Semaphore and synchronization problems
