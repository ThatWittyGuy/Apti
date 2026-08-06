# 1. Bubble Sort

### Idea

Compare adjacent elements and swap if they are in the wrong order. After each pass, the largest element moves to the end.

### Dry Run

```text
5 3 4 1

Pass 1:
3 5 4 1
3 4 5 1
3 4 1 5

Pass 2:
3 4 1 5
3 1 4 5

Pass 3:
1 3 4 5
```

### C++

```cpp
void bubbleSort(vector<int>& arr) {
    int n = arr.size();

    // Run n-1 passes
    for(int i=0;i<n-1;i++){

        bool swapped = false;

        // Compare adjacent elements
        for(int j=0;j<n-i-1;j++){

            if(arr[j] > arr[j+1]){
                swap(arr[j], arr[j+1]);
                swapped = true;
            }
        }

        // Stop if already sorted
        if(!swapped) break;
    }
}
```

### Time & Space Complexity

| Best | Average | Worst | Space |
| ---- | ------- | ----- | ----- |
| O(n) | O(n²)   | O(n²) | O(1)  |

- **Best:** Array is already sorted (no swaps occur due to `swapped` flag).
- **Average:** Randomly ordered array.
- **Worst:** Reverse sorted array.

---

# 2. Selection Sort

### Idea

Find the smallest element from the unsorted part and place it at the beginning.

### Dry Run

```text
64 25 12 22 11

11 25 12 22 64

11 12 25 22 64

11 12 22 25 64
```

### C++

```cpp
void selectionSort(vector<int>& arr){

    int n = arr.size();

    // Select minimum for every position
    for(int i=0;i<n-1;i++){

        int minIndex = i;

        // Find minimum
        for(int j=i+1;j<n;j++){

            if(arr[j] < arr[minIndex])
                minIndex = j;
        }

        // Put minimum at correct position
        swap(arr[i],arr[minIndex]);
    }
}
```

### Time & Space Complexity

| Best  | Average | Worst | Space |
| ----- | ------- | ----- | ----- |
| O(n²) | O(n²)   | O(n²) | O(1)  |

- **Best:** Already sorted array (still scans entire unsorted part).
- **Average:** Randomly ordered array.
- **Worst:** Reverse sorted array.

> Best, Average, and Worst all take **O(n²)** because the number of comparisons never changes.

---

# 3. Insertion Sort

### Idea

Insert each element into its correct position in the sorted part.

### Dry Run

```text
5 2 4 6 1

2 5 4 6 1

2 4 5 6 1

2 4 5 6 1

1 2 4 5 6
```

### C++

```cpp
void insertionSort(vector<int>& arr){

    int n = arr.size();

    // Start from second element
    for(int i=1;i<n;i++){

        int key = arr[i];
        int j = i-1;

        // Shift greater elements
        while(j>=0 && arr[j]>key){
            arr[j+1]=arr[j];
            j--;
        }

        // Insert key
        arr[j+1]=key;
    }
}
```

### Time & Space Complexity

| Best | Average | Worst | Space |
| ---- | ------- | ----- | ----- |
| O(n) | O(n²)   | O(n²) | O(1)  |

- **Best:** Already sorted array (only one comparison per element).
- **Average:** Randomly ordered array.
- **Worst:** Reverse sorted array (maximum shifts).

---

## 4. Merge Sort

### Idea

Divide the array into two halves, sort each half recursively, then merge the sorted halves.

### Dry Run

```text
38 27 43 3

Split:
38 27 | 43 3

38 | 27 | 43 | 3

Merge:
27 38 | 3 43

Final:
3 27 38 43
```

### C++

```cpp
void merge(vector<int>& arr, int l, int mid, int r){

    vector<int> temp;
    int i=l, j=mid+1;

    // Merge two sorted halves
    while(i<=mid && j<=r){
        if(arr[i] <= arr[j])
            temp.push_back(arr[i++]);
        else
            temp.push_back(arr[j++]);
    }

    // Copy remaining left half
    while(i<=mid) temp.push_back(arr[i++]);

    // Copy remaining right half
    while(j<=r) temp.push_back(arr[j++]);

    // Copy back
    for(int k=l;k<=r;k++)
        arr[k]=temp[k-l];
}

void mergeSort(vector<int>& arr,int l,int r){

    // Base case
    if(l>=r) return;

    int mid=l+(r-l)/2;

    // Sort left half
    mergeSort(arr,l,mid);

    // Sort right half
    mergeSort(arr,mid+1,r);

    // Merge both halves
    merge(arr,l,mid,r);
}
```

### Time & Space Complexity

| Best       | Average    | Worst      | Space |
| ---------- | ---------- | ---------- | ----- |
| O(n log n) | O(n log n) | O(n log n) | O(n)  |

- **Best:** Already sorted array.
- **Average:** Randomly ordered array.
- **Worst:** Reverse sorted array.

> Merge Sort always divides and merges the array, so all cases are **O(n log n)**.

---

# 5. Quick Sort

### Idea

Choose a pivot, place smaller elements to its left and larger elements to its right, then recursively sort both sides.

### Dry Run

```text
7 2 1 6 8 5 3 4

Pivot = 4

2 1 3 |4| 7 6 8 5

↓

1 2 3 4 5 6 7 8
```

### C++

```cpp
int partition(vector<int>& arr,int low,int high){

    // Choose last element as pivot
    int pivot=arr[high];

    int i=low-1;

    for(int j=low;j<high;j++){

        // Place smaller elements before pivot
        if(arr[j]<pivot){
            i++;
            swap(arr[i],arr[j]);
        }
    }

    // Place pivot correctly
    swap(arr[i+1],arr[high]);

    return i+1;
}

void quickSort(vector<int>& arr,int low,int high){

    if(low<high){

        int pi=partition(arr,low,high);

        // Sort left
        quickSort(arr,low,pi-1);

        // Sort right
        quickSort(arr,pi+1,high);
    }
}
```

### Time & Space Complexity

| Best       | Average    | Worst | Space    |
| ---------- | ---------- | ----- | -------- |
| O(n log n) | O(n log n) | O(n²) | O(log n) |

- **Best:** Pivot always divides the array into two equal halves.
- **Average:** Pivot divides the array reasonably evenly.
- **Worst:** Pivot is always the smallest or largest element (e.g., already sorted array if using first/last element as pivot).

---

# 6. Heap Sort

### Idea

Build a Max Heap, repeatedly remove the maximum element and place it at the end.

### Dry Run

```text
4 10 3 5 1

Build Heap

10 5 3 4 1

Swap Root with Last

5 4 3 1 |10

↓

4 1 3 |5 10

↓

1 3 |4 5 10

↓

1 3 4 5 10
```

### C++

```cpp
void heapify(vector<int>& arr,int n,int i){

    int largest=i;
    int left=2*i+1;
    int right=2*i+2;

    // Find largest
    if(left<n && arr[left]>arr[largest])
        largest=left;

    if(right<n && arr[right]>arr[largest])
        largest=right;

    // Fix heap
    if(largest!=i){
        swap(arr[i],arr[largest]);
        heapify(arr,n,largest);
    }
}

void heapSort(vector<int>& arr){

    int n=arr.size();

    // Build max heap
    for(int i=n/2-1;i>=0;i--)
        heapify(arr,n,i);

    // Extract maximum
    for(int i=n-1;i>0;i--){

        swap(arr[0],arr[i]);

        heapify(arr,i,0);
    }
}
```

### Time & Space Complexity

| Best       | Average    | Worst      | Space |
| ---------- | ---------- | ---------- | ----- |
| O(n log n) | O(n log n) | O(n log n) | O(1)  |

- **Best:** Already sorted array.
- **Average:** Randomly ordered array.
- **Worst:** Reverse sorted array.

> Heap construction and extraction always take nearly the same work, so all cases are **O(n log n)**.

---

# 7. Counting Sort

### Idea

Count the frequency of every element and rebuild the sorted array.

### Dry Run

```text
Input

4 2 2 8 3 3 1

Frequency

1→1
2→2
3→2
4→1
8→1

Output

1 2 2 3 3 4 8
```

### C++

```cpp
void countingSort(vector<int>& arr){

    int mx=*max_element(arr.begin(),arr.end());

    vector<int> count(mx+1,0);

    // Count frequency
    for(int x:arr)
        count[x]++;

    int index=0;

    // Write sorted values
    for(int i=0;i<=mx;i++){

        while(count[i]--){
            arr[index++]=i;
        }
    }
}
```

### Time & Space Complexity

| Best   | Average | Worst  | Space |
| ------ | ------- | ------ | ----- |
| O(n+k) | O(n+k)  | O(n+k) | O(k)  |

- **Best:** Small value range (`k`) with any input order.
- **Average:** Random input with small range.
- **Worst:** Large value range (`k`) compared to `n`.

> Complexity depends on the range (`k`), not the input order.

---

# 8. Radix Sort

### Idea

Sort numbers digit by digit (LSD → MSD) using Counting Sort.

### Dry Run

```text
170 45 75 90 802 24 2 66

1's digit

170 90 802 2 24 45 75 66

10's digit

802 2 24 45 66 170 75 90

100's digit

2 24 45 66 75 90 170 802
```

### C++

```cpp
void countingSortExp(vector<int>& arr,int exp){

    int n=arr.size();

    vector<int> output(n);
    int count[10]={0};

    // Count digits
    for(int x:arr)
        count[(x/exp)%10]++;

    // Prefix sum
    for(int i=1;i<10;i++)
        count[i]+=count[i-1];

    // Build output
    for(int i=n-1;i>=0;i--){

        int digit=(arr[i]/exp)%10;

        output[count[digit]-1]=arr[i];

        count[digit]--;
    }

    arr=output;
}

void radixSort(vector<int>& arr){

    int mx=*max_element(arr.begin(),arr.end());

    // Sort every digit
    for(int exp=1;mx/exp>0;exp*=10)
        countingSortExp(arr,exp);
}
```

### Time & Space Complexity

| Best      | Average   | Worst     | Space  |
| --------- | --------- | --------- | ------ |
| O(d(n+k)) | O(d(n+k)) | O(d(n+k)) | O(n+k) |

- **Best:** Small number of digits.
- **Average:** Numbers with moderate digits.
- **Worst:** Numbers with many digits.

> Complexity depends on the number of digits (`d`), not the input order.

---

# 9. Bucket Sort

### Idea

Distribute elements into buckets, sort each bucket, then merge them.

### Dry Run

```text
0.42 0.32 0.23 0.52 0.25

Buckets

0 : 0.23 0.25

1 : 0.32

2 : 0.42

3 : 0.52

Output

0.23 0.25 0.32 0.42 0.52
```

### C++

```cpp
void bucketSort(vector<float>& arr){

    int n=arr.size();

    vector<vector<float>> bucket(n);

    // Put elements into buckets
    for(float x:arr){

        int idx=n*x;

        bucket[idx].push_back(x);
    }

    // Sort each bucket
    for(int i=0;i<n;i++)
        sort(bucket[i].begin(),bucket[i].end());

    int index=0;

    // Merge buckets
    for(int i=0;i<n;i++)
        for(float x:bucket[i])
            arr[index++]=x;
}
```

### Time & Space Complexity

| Best | Average | Worst | Space |
| ---- | ------- | ----- | ----- |
| O(n) | O(n)    | O(n²) | O(n)  |

- **Best:** Uniformly distributed elements across buckets.
- **Average:** Nearly uniform distribution.
- **Worst:** All elements fall into the same bucket.

---

# 10. Shell Sort

### Idea

Perform insertion sort on elements separated by a gap, reducing the gap until it becomes 1.

### Dry Run

```text
12 34 54 2 3

Gap = 2

12 2 54 34 3

Gap = 1

2 3 12 34 54
```

### C++

```cpp
void shellSort(vector<int>& arr){

    int n=arr.size();

    // Reduce gap
    for(int gap=n/2;gap>0;gap/=2){

        // Gapped insertion sort
        for(int i=gap;i<n;i++){

            int temp=arr[i];
            int j=i;

            while(j>=gap && arr[j-gap]>temp){

                arr[j]=arr[j-gap];
                j-=gap;
            }

            arr[j]=temp;
        }
    }
}
```

### Time & Space Complexity

| Best         | Average    | Worst | Space |
| ------------ | ---------- | ----- | ----- |
| ≈ O(n log n) | ≈ O(n^1.5) | O(n²) | O(1)  |

- **Best:** Nearly sorted array.
- **Average:** Randomly ordered array.
- **Worst:** Reverse sorted array (depends on the chosen gap sequence).

---

## T&C Comparison

| Algorithm | Best                 | Average    | Worst      | Stable  |
| --------- | -------------------- | ---------- | ---------- | ------- |
| Bubble    | O(n)                 | O(n²)      | O(n²)      | ✅      |
| Selection | O(n²)                | O(n²)      | O(n²)      | ❌      |
| Insertion | O(n)                 | O(n²)      | O(n²)      | ✅      |
| Merge     | O(n log n)           | O(n log n) | O(n log n) | ✅      |
| Quick     | O(n log n)           | O(n log n) | O(n²)      | ❌      |
| Heap      | O(n log n)           | O(n log n) | O(n log n) | ❌      |
| Counting  | O(n+k)               | O(n+k)     | O(n+k)     | ✅\*    |
| Radix     | O(d(n+k))            | O(d(n+k))  | O(d(n+k))  | ✅      |
| Bucket    | O(n)                 | O(n)       | O(n²)      | Depends |
| Shell     | ~O(n log n)–O(n^1.5) | ~O(n^1.5)  | O(n²)      | ❌      |
