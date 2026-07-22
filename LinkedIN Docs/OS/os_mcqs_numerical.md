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

# Operating System Numerical MCQs (Placement / Online Assessment)

---

# 1) CPU Scheduling

## Q1. FCFS Waiting Time

Processes arrive in order:

| Process | Burst Time |
| ------- | ---------- |
| P1      | 6          |
| P2      | 4          |
| P3      | 2          |

What is the average waiting time using FCFS?

a) 4  
b) 5  
c) 6  
d) 7

### Solution

Waiting time:

```
P1 = 0
P2 = 6
P3 = 6 + 4 = 10
```

Average:

\[
\frac{0+6+10}{3}=5.33
\]

**Answer: b) 5 (approx)**

---

## Q2. SJF Average Turnaround Time

Processes:

| Process | Burst Time |
| ------- | ---------- |
| P1      | 7          |
| P2      | 4          |
| P3      | 1          |

Using non-preemptive SJF, average turnaround time is:

a) 6  
b) 7  
c) 8  
d) 9

### Solution

Execution order:

```
P3 → P2 → P1
```

Completion times:

```
P3 = 1
P2 = 5
P1 = 12
```

Average:

\[
\frac{1+5+12}{3}=6
\]

**Answer: a) 6**

---

## Q3. Round Robin Completion Time

Time quantum = 2 ms

Processes:

| Process | Burst |
| ------- | ----- |
| P1      | 5     |
| P2      | 3     |

Find completion time of P1.

a) 6  
b) 7  
c) 8  
d) 9

### Solution

Execution:

```
0-2   P1
2-4   P2
4-6   P1
6-7   P2
7-8   P1
```

P1 completes at:

```
8 ms
```

**Answer: c) 8**

---

## Q4. Priority Scheduling

Lower number = higher priority.

| Process | Burst | Priority |
| ------- | ----- | -------- |
| P1      | 5     | 2        |
| P2      | 3     | 1        |
| P3      | 4     | 3        |

Average waiting time?

a) 2.33  
b) 3.33  
c) 4.33  
d) 5.33

### Solution

Execution order:

```
P2 → P1 → P3
```

Waiting time:

```
P2 = 0
P1 = 3
P3 = 8
```

Average:

\[
\frac{0+3+8}{3}=3.66
\]

**Answer: b) 3.33 (closest)**

---

# 2) Paging

---

## Q5. FIFO Page Fault Count

Frames = 3

Reference string:

```
1 2 3 1 4 5
```

Number of faults?

a) 3  
b) 4  
c) 5  
d) 6

### Solution

```
1 → Fault
2 → Fault
3 → Fault
1 → Hit
4 → Fault
5 → Fault
```

Total faults:

```
5
```

**Answer: c) 5**

---

## Q6. LRU Page Replacement

Frames = 3

Reference:

```
7 0 1 2 0 3 0
```

Page faults?

a) 4  
b) 5  
c) 6  
d) 7

### Solution

```
7 → Fault
0 → Fault
1 → Fault
2 → Fault (replace 7)
0 → Hit
3 → Fault (replace 1)
0 → Hit
```

Total faults:

```
5
```

**Answer: b) 5**

---

## Q7. Optimal Page Replacement

Frames = 3

Reference:

```
1 2 3 4 1 2 5
```

Page faults?

a) 5  
b) 6  
c) 7  
d) 4

### Solution

```
1 → Fault
2 → Fault
3 → Fault
4 → Fault (replace 3)
1 → Hit
2 → Hit
5 → Fault
```

Total faults:

```
5
```

**Answer: a) 5**

---

## Q8. Belady's Anomaly

FIFO page replacement:

Reference:

```
1 2 3 4 1 2 5 1 2 3 4 5
```

Frames increased from 3 to 4.

What can happen?

a) Faults always decrease  
b) Faults may increase  
c) Faults become zero  
d) No change possible

**Answer: b) Faults may increase**

---

# 3) Memory Management

---

## Q9. First Fit Allocation

Memory blocks:

```
100 KB, 500 KB, 200 KB
```

Process size:

```
150 KB
```

Where allocated?

a) 100 KB  
b) 500 KB  
c) 200 KB  
d) Not allocated

### Solution

100 KB is too small.

500 KB fits.

**Answer: b) 500 KB**

---

## Q10. Best Fit Allocation

Blocks:

```
300 KB, 100 KB, 200 KB
```

Process:

```
90 KB
```

Best fit chooses:

a) 300  
b) 100  
c) 200  
d) None

Smallest block that fits:

```
100 KB
```

**Answer: b) 100 KB**

---

## Q11. Worst Fit Allocation

Blocks:

```
400 KB, 200 KB, 300 KB
```

Process:

```
150 KB
```

Worst fit chooses:

a) 400  
b) 200  
c) 300  
d) None

Largest block:

```
400 KB
```

**Answer: a) 400 KB**

---

## Q12. Logical Address Conversion

Page size = 1024 bytes

Logical address = 3500 bytes

Find page number and offset.

a) Page 3 Offset 428  
b) Page 2 Offset 452  
c) Page 4 Offset 200  
d) Page 1 Offset 500

### Solution

Page number:

\[
3500/1024=3
\]

Offset:

\[
3500-3072=428
\]

**Answer: a) Page 3 Offset 428**

---

# 4) Disk Scheduling

---

## Q13. FCFS Disk Scheduling

Queue:

```
82, 170, 43, 140
```

Initial head = 50

Total movement?

a) 350  
b) 367  
c) 400  
d) 420

### Solution

\[
|50-82|+|82-170|+|170-43|+|43-140|
\]

\[
32+88+127+97=344
\]

**Answer: 344**

---

## Q14. SSTF Disk Scheduling

Requests:

```
40, 10, 70, 90
```

Head = 50

First request served?

a) 10  
b) 40  
c) 70  
d) 90

Closest:

```
40
```

**Answer: b) 40**

---

## Q15. SCAN Disk Scheduling

Head starts at 50 moving upward.

Requests:

```
20, 40, 60, 90
```

Order?

a) 20,40,60,90  
b) 60,90,40,20  
c) 90,60,40,20  
d) 40,20,60,90

Moving upward:

```
60 → 90 → 40 → 20
```

**Answer: b) 60,90,40,20**

---

# 5) Deadlock

---

## Q16. Banker's Algorithm

Available:

```
A B C
3 2 2
```

Need:

```
1 2 1
```

Can process execute?

a) Yes  
b) No

Check:

```
1 ≤ 3
2 ≤ 2
1 ≤ 2
```

**Answer: a) Yes**

---

## Q17. Safe Sequence

Processes:

```
P1 P2 P3
```

Available:

```
3 resources
```

Only P2 can finish initially.

Which executes first?

a) P1  
b) P2  
c) P3  
d) None

**Answer: b) P2**

---

## Q18. Resource Allocation Graph

A cycle in RAG indicates:

a) Always deadlock  
b) Never deadlock  
c) Possible deadlock  
d) System crash

**Answer: c) Possible deadlock**

---

## Q19. Deadlock Conditions

Four conditions required:

1. Mutual exclusion
2. Hold and wait
3. No preemption
4. Circular wait

Number of conditions?

a) 2  
b) 3  
c) 4  
d) 5

**Answer: c) 4**

---

# Important OS Numerical Topics for Campus OA

1. FCFS / SJF / Round Robin scheduling
2. Waiting time and turnaround time calculation
3. FIFO / LRU / Optimal page replacement
4. Belady's anomaly
5. Banker's algorithm
6. Safe sequence
7. Disk scheduling (FCFS, SSTF, SCAN, C-SCAN)
8. Memory allocation (First Fit, Best Fit, Worst Fit)
9. Paging and address translation
10. Deadlock detection
